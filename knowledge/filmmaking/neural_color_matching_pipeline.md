# ニューラル色合わせパイプライン (iPhone → FX30)

対象: 同一ステージを異なるカメラ (iPhone / FX30) で収録した映像の色味を揃える。LUT では対応できない時変ゲイン差がある場合に使う。

プロジェクト実績: プペル number/ゆかやねん (2026-09-05)

## 全体構成

```
① ホモグラフィー推定 → 対応ピクセルペア サンプリング
② 自動露出追跡 (ae_probe) → フレームごとの輝度時系列
③ 時変ゲイン曲線 (gain_t) 生成
④ ニューラル色変換モデル (PyTorch) 学習
⑤ render.py で適用 → ffmpeg H.265 HEVC 書き出し
```

## ① ホモグラフィー推定

- iPhone と FX30 の平均フレーム (複数枚平均) を用意
- OpenCV `findHomography` (RANSAC) でステージ面の対応を推定
- `H_refined.npy` として保存
- ステージ領域マスク (ポリゴン) で被写体・照明以外の面から対応点を取る

## ② ピクセルペアサンプリング

```python
# pairs.npz に X (iPhone), Y (FX30), T (frame), W (weight) を保存
# W はステージマスク内のみ 1.0
```

- サンプリングはランダムでなく均等グリッド (フレーム×位置) で取る
- 暗ピクセル (X.max(1) < 0.02) は外す (ノイズが多く学習を汚す)

## ③ 自動露出追跡 (ae_probe)

iPhone はシーン輝度変化に応じて AE が動き、フレームごとに全体ゲインが変わる。これをニューラルモデルに混ぜると色モデルが汚れるため、事前に分離する。

```python
# ae_probe.py / ae_probe2.npz
# ti: iPhone フレームの時刻 (秒)
# tf: FX30 フレームの時刻 (秒)
# imlum: iPhone 各フレームの中央値輝度 (マスク内)
# fxlum: FX30 各フレームの中央値輝度
```

- ステージ中央の固定ポリゴン (照明変化が少ない領域) で輝度を取る
- `gaussian_filter1d` で平滑化してから比率を計算
- `r_topn.npy`: 各フレームの上位 N% 輝度のパーセンタイル比 → 時変ゲイン

## ④ ニューラル色変換モデル

```python
# fit6.py (最終版)
# 入力: RGB (gamma 2.2 適用済み) + フレームインデックス埋め込み
# 出力: FX30 相当 RGB
# デバイス: MPS (Apple Silicon)
```

構造:
- 線形層 3 → 32 → 32 → 3 + フレーム別バイアス (`nn.Embedding(NF, 3)`)
- 入力は `x.clamp(1e-5, 1) ** (1/GAM)` (linear 化) → 色モデル → `** GAM` で戻す
- 損失: MSE (線形空間) + delta 正則化 (フレームバイアスの過学習防止)

暗ピクセル / 飽和ピクセルはウェイトを下げる (学習が汚れる)。

学習後 `model.pt` として保存。

## ⑤ 時変ゲイン曲線

```python
# floor_gain.py / r_topn.py
# gain_t: フレーム時刻 → ゲインスカラー
# 上位 N% 輝度比を時間平滑化 → scipy.ndimage.median_filter + gaussian_filter1d
```

- 上位 5%〜10% のパーセンタイル比が最も安定 (最大値は外れ値に引きずられる)
- 最小ゲインに floor を設ける (急激な暗転でゲインが発散しないよう)

## ⑥ render.py — 最終レンダー

```python
# python3 render.py <model.pt> <gain_t.npy> <out.mp4> [start_frame] [seg_len_sec] [codec]
```

処理フロー:
1. iPhone 映像をフレームごとに読み込み
2. ホモグラフィーでワープ (FX30 座標系に合わせる)
3. モデルで色変換 → ゲイン曲線でスケール
4. stdout に生 RGB を流す → ffmpeg で H.265 HEVC エンコード

ffmpeg エンコードオプション (libx265):
```bash
ffmpeg -f rawvideo -pix_fmt rgb24 -s WxH -r FPS -i pipe:0 \
  -c:v libx265 -crf 18 -preset slow \
  -color_range tv -colorspace bt709 ... \
  out.mp4
```

## モデルの反復改善履歴

| バージョン | 変更点 |
|-----------|-------|
| fit.py | 基本色変換 (per-channel linear) |
| fit2.py | 暗ピクセル除外・ウェイト導入 |
| fit3.py | フレーム別正規化 |
| fit4.py | MPS、フレーム埋め込み + 地域 (R) 特徴 |
| fit5.py | ae_probe 組み込み、gain_t 同時学習 |
| fit6.py | seed=1、delta 正則化強化、ae_probe2 使用 |
| model7 | r_topn (上位 N% 比) ゲイン曲線 + fit6 構造 |

---

✅ うまく行ったこと
- ホモグラフィーによるピクセル対応が色サンプリングの基盤として有効 (LUT より精密)
- AE 追跡 (ae_probe) でゲインを分離してから色モデルを学習すると収束が安定した
- 上位 N% パーセンタイル比 (r_topn) が最大値より外れ値に強く、安定したゲイン曲線になった
- render.py の stdout pipe → ffmpeg 構成でフレームを一時ファイルなしにエンコードできた
- MPS (Apple Silicon) で fit が数十秒〜数分で回せた

❌ 詰まったこと
- gamma 空間で MSE を取ると暗部に過学習する → linear 化してから損失計算
- clamp(0,1) の端での勾配消失 → clamp(1e-5, 1) に変更
- 最大値ゲインは照明 spike で発散 → パーセンタイル (上位 5%) に切り替え
- floor_gain の最小値設定がないと暗転フレームでゲインが 5× 超に発散

📋 次回同じことをするときのチェックリスト
1. 事前確認: iPhone と FX30 の fps 差・タイムコード有無 → 音声クロス相関で同期オフセット算出 (multicam_audio_sync_crosscorrelation)
2. ホモグラフィー: 静止フレーム複数枚を平均してから推定 (ブレを消す)。ステージ面のみをマスク
3. ペアサンプリング: 暗ピクセル (max < 0.02) と飽和ピクセルを除外してから学習
4. ae_probe: ステージ中央の固定ポリゴン領域で計測。周辺照明ラインが入ると spike が出る
5. ゲイン曲線: 上位 N% (5〜10%) パーセンタイル比 + median_filter + gaussian_filter1d + floor clamp
6. モデル学習: linear 空間で損失計算、gamma 変換は入出力だけ
7. render.py テスト: まず 3 秒セグメントで色と輝度を目視確認してから全尺実行
8. 出力コーデック: H.265 / libx265 / CRF 18〜22 / bt709 タグ付け
