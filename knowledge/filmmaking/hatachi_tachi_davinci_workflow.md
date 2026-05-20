---
name: hatachi-tachi-davinci-workflow
description: 平成たち祭 DaVinci Resolve 編集ステップバイステップ。朝起きたらこの順序でやる。
metadata:
  type: knowledge
  created: 2026-05-20
  project: 平成たち祭 動画制作
---

# 平成たち祭 DaVinci Resolve 編集ワークフロー

> 朝起きたら、このファイルを上から順番に実行するだけ
> 想定合計: 本編 roughcut まで 4-6 時間 / ファインカット + カラグレ + 書き出し 6-10 時間

---

## 前提: 外付けSSD を接続してから起動

1. 外付けSSD を MacBook に接続 (Thunderbolt or USB-C)
2. Finder でマウント確認 (`/Volumes/<SSD名>/`)
3. DaVinci Resolve を起動

---

## Step 1: プロジェクト初期化 (15分)

### 1-1 新規プロジェクト作成

```
DaVinci → New Project → 名前: 「平成たち祭_2026_REPLAYGROUND」
```

### 1-2 プロジェクト設定 (Project Settings)

```
Master Settings:
  Timeline resolution: 3840 × 2160 (4K UHD)
  Timeline frame rate: 24.000
  Playback frame rate: 24

Image Scaling:
  Input scaling: Scale full frame with crop (4:3素材はこれでOK)

Color management:
  Color science: DaVinci YRGB Color Managed
  → または手動: Resolve Color Management を OFF にして後でCST手動
```

推奨: Color Management は「OFF + 手動CST」のほうが細かく制御できる。

### 1-3 メディア保存場所の確認

```
Preferences → System → Media Storage Locations:
  外付けSSDのパスを追加しておく
```

---

## Step 2: 素材インポート・ビン整理 (30-60分)

### 2-1 ビン (フォルダ) 構造を作る

```
Media Pool (左パネル):
  ├── 01_R5       (Canon EOS R5 / C-Log3 / ダンスメイン)
  ├── 02_FX30     (Sony FX30 / S-Log3 / 観客・Bロール)
  ├── 03_GoPro    (GoPro / BTS素材)
  ├── 04_OsmoP3   (Osmo Pocket 3 / レクエリア)
  ├── 05_Insta360 (360度 / メイキング用)
  ├── 06_Audio    (BGM / SE / イベント楽曲)
  ├── 07_Graphics (LUT / テロップ / エンドカード素材)
  └── 08_Output   (書き出し確認用)
```

### 2-2 素材をインポート

```
Media Pool → 右クリック → Add Folder and Subfolders →
  外付けSSD の R5 / FX30 / GoPro / OsmoP3 / Insta360 フォルダを追加
```

⚠️ この時点でクリップが見つからない場合: SSD が認識されていない。Finder で確認。

### 2-3 クリップにフラグをつける (重要シーンを先にマーク)

```
各クリップをスクラブして、以下の「最優先確保素材」を Red Flag:
  - さくら木ビフォー / アフター
  - ダンサー後ろ姿 (ステージ直前)
  - Haruhi ⑭ソロ全カット
  - 観客涙 / 笑顔
  - 逆光シルエット
```

---

## Step 3: タイムライン作成・Roughcut (2-3時間)

### 3-1 タイムライン設定

```
Timelines → New Timeline → 「01_2min_REPLAYGROUND」
  → プロジェクト設定から: 24fps / 4K UHD
```

### 3-2 パート別の順序で Roughcut

**COLD OPEN (0:00〜0:08)** — 30分

```
1. さくら木ビフォー (3秒)
2. ステージ照明が落ちる瞬間 (3秒)
3. 観客横顔シルエット (2秒)
→ カット間: ハードカット (ディゾルブなし)
→ 音: 環境音のみ、音量 -20dB
```

**INTRO (0:08〜0:20)** — 30分

```
BGMのビートを先にタイムラインに置く → ビートに合わせてカットを並べる
1. 会場全体 (ビートIN)
2. 受付・おみくじ手元
3. 装飾・黒板ポスター
4. 入場する観客の足元
5. GoPro ダンサー後ろ姿
→ テキスト: 「REPLAYGROUND」を 0:10 付近に
```

**波① 溜め (0:20〜0:45)** — 45分

```
BTS を多めに配置 (緊張感を先に作る)
1. GoPro 緊張顔・深呼吸 (4秒)
2. GoPro ステージ直前後ろ姿 (3秒)
3. R5 板付き開始 (照明暗い) (4秒)
4. R5 指先クローズアップ (3秒 スロー)
5. FX30 観客横顔 (見上げている) (3秒)
6. R5 ダンス見せ場1 (4秒)
7. FX30 観客後ろ姿＋ステージ (4秒)
```

**波② 爆発 (0:45〜1:15)** — 60分

```
BGMのビートと完全同期 → カット割りを一気に速くする
1. R5 各演目見せ場 (合計15秒、計5-8カット)
2. R5 スピードランプ (120fps素材でラムプ)
3. R5 照明変化 (暗転→ON)
4. FX30 観客ノリノリ
5. FX30 顔を見合わせる瞬間
6. GoPro ハイタッチ・達成感
```

**波③ 静寂 (1:15〜1:30)** — 30分

```
BGM 音量 -6dB → テンポダウン演出
1. R5 Haruhi 表情 (60fps スロー 6秒)
2. R5 Haruhi 指先 (120fps スロー 4秒)
3. FX30 感動的な観客の顔 (5秒)
```

**REC FLASH (1:30〜1:40)** — 20分

```
高速カット (各2秒以内)
1. 内職チャレンジ笑顔
2. さくら貼る手元
3. おみくじ驚き
4. 懐かしイイネ展リアクション
5. 談笑シーン
```

**FINALE (1:40〜2:00)** — 30分

```
テンポを徐々に落とす
1. Haruhi 残りカット
2. 観客の涙 or 笑顔
3. 出演者全員が並ぶ
4. さくら木の完成形 (6秒、最終カット)
5. フェードアウト → エンドカード (3秒)
```

---

## Step 4: カラーグレーディング (2-4時間)

### 4-1 カラースペース変換 (CST) — 最初に必ず

```
Color ページ → 各クリップの先頭ノードに CST を入れる:

FX30 (S-Log3):
  Input: S-Gamut3.Cine / S-Log3
  Output: DaVinci Wide Gamut / DaVinci Intermediate

R5 (C-Log3):
  Input: Canon Cinema Gamut / Canon Log 3
  Output: DaVinci Wide Gamut / DaVinci Intermediate

GoPro / Osmo P3 (Rec.709):
  CST 不要 → 直接グレード
```

### 4-2 LUT 適用 (ベース)

```
Color → OpenFX → Apply LUT:
  全クリップに Leefilm Japan (or Kodak 2383) を薄め (50%) で適用
  → これがベース調。後でパート別に上書き
```

### 4-3 パート別グレード (CST後ノードに追加)

```
COLD OPEN:
  Curves: シャドウを青緑に (S字カーブ)
  Noise Reduction: 薄く

INTRO / FINALE:
  Color Wheels: Lift (+R) / Gain (+R -B) ウォームトーン
  Highlight Saturation: 少し上げる
  Grain: Resolve FX → Film Grain 5-8%

波①:
  Exposure: -0.3EV
  Saturation: 90%

波②:
  Contrast: +0.15
  Saturation: 115%
  Highlights: ステージ照明の色を強調

波③:
  Lift: +R +G (暖色)
  Gain: +R +G -B (ゴールド)
  Grain: 8-10%

REC FLASH:
  LUT: Fuji 3513 (50%) or ウォームLUT
  Grain: 10-12%
```

### 4-4 ショットマッチング

```
各パート内でカット間の露出・色温度をそろえる
Color → Shot Match (複数クリップ選択 → Shot Match)
→ 手動微調整で仕上げ
```

---

## Step 5: オーディオミキシング (1-2時間)

### 5-1 トラック構成

```
Timeline のオーディオトラック:
  A1: BGM (ステレオ)
  A2: 環境音・SE (ステレオ)
  A3: インタビュー / 本番音声 (ステレオ、波②で薄く敷く)
```

### 5-2 BGM 調整

```
- COLD OPEN: BGM なし / 環境音 -20dB
- INTRO (0:08〜): BGMイン / -6dB から徐々に +0dB
- 波①: BGM 0dB
- 波② → 静寂への切り替わり (1:15): BGM -6dB にフェードダウン
- FINALE (1:40〜): BGMを徐々に -30dB まで落とす
```

### 5-3 SE 配置

```
- 波①→② 切り替わり (0:45): ライズアップSE
- 拍手: 波②終わり / FINALE
- 歓声: 波②ピーク
- さくら木カット: 静寂 (SE なし)
```

### 5-4 Loudness 確認

```
Fairlight ページ → Loudness Meter:
  目標: -14 LUFS (YouTube / Instagram 推奨)
  ピーク: -1 dBFS
```

---

## Step 6: グラフィック・テロップ (1時間)

### 6-1 タイトル (INTRO)

```
Fusion ページ または Edit ページ:
  - テキスト: 「REPLAYGROUND」
  - フォント: Montserrat Bold or Bebas Neue
  - 色: 白 / サイズ: 画面幅の60%
  - アニメーション: Fade in (0.5秒)
  - タイミング: 0:10〜0:18
```

### 6-2 出演者名テロップ (波①〜②)

```
対象: ソロ演目の出演者
  - テキスト: 名前 (漢字 or 英字)
  - フォント: Noto Sans JP Light or M PLUS 1p
  - 色: 白 / サイズ小さめ (画面幅の25%)
  - 位置: 下部 (下から15%)
  - タイミング: カット冒頭に Fade in / カット終わりに Fade out
```

### 6-3 レク企画名テロップ (REC FLASH)

```
  - テキスト: 「懐かしイイネ展」「内職チャレンジ」「さくらメッセージ」
  - フォント: 手書き風 or ポップ
  - 色: 黄色 or ビビッドカラー
  - タイミング: 各カットの冒頭 1秒だけ表示
```

### 6-4 エンドカード (FINALE 末尾 3秒)

```
  - テキスト:
      「平成たち祭 -REPLAYGROUND-」
      「2026.05.06 | in the house 西早稲田」
      「@次回告知 SNS アカウント」
  - デザイン: シンプル / 黒背景 or さくら木のフェード上に重ねる
```

---

## Step 7: ファインカット (1-2時間)

```
Edit ページで全体を通し再生:
  □ BGMのビートとカットが合っているか
  □ 波② の盛り上がりで気持ちいいか
  □ 波③ で静かになった後に FINALE が自然に来るか
  □ 全体の尺が 2:00 ± 5秒に収まるか
  □ テロップに誤字・位置ズレがないか
  □ 音量の急変がないか (特に COLD OPEN → INTRO の切り替わり)
  □ スピードランプが滑らかか
  □ エンドカードが読める速さで表示されているか
```

---

## Step 8: 書き出し (30分)

### 8-1 本編 (YouTube 横動画)

```
Deliver ページ:
  Format: H.264 (または H.265)
  Resolution: 3840 × 2160 (4K)
  Frame Rate: 24
  Quality: Restrict to: 50,000 Kbps (高品質)
  Audio: AAC 320kbps / 48kHz / Stereo
  ファイル名: 「平成たち祭_REPLAYGROUND_2026_4K_v01.mp4」
  出力先: 外付けSSD/Output/ or ~/Downloads/02_映像写真プロジェクト/平成たち祭/書き出し/
```

### 8-2 SNS 縦動画用 (9:16)

```
Timeline を複製 → 縦動画用タイムライン「02_shorts_1080x1920」を作成:
  Resolution: 1080 × 1920
  同じシーケンスをリフレームして縦動画に対応

Deliver:
  Format: H.264
  Resolution: 1080 × 1920
  Frame Rate: 30 (SNS推奨)
  Quality: 15,000 Kbps
  ファイル名: 「平成たち祭_SHORT01_BTS.mp4」など
```

### 8-3 書き出しチェックリスト

```
  □ 本編 4K 書き出し完了
  □ Short #1〜5 それぞれ書き出し完了
  □ ファイルサイズが異常に小さくないか確認
  □ 最初と最後の数秒を再生して書き出し確認
  □ 音声が入っているか確認
```

---

## タイムスケジュール案 (1日集中編集)

| 時間帯 | 作業 | 想定時間 |
|------|------|--------|
| 朝 9:00〜 | Step 1-2 (プロジェクト初期化 + インポート) | 1時間 |
| 10:00〜 | Step 3 Roughcut (COLD OPEN〜INTRO〜波①) | 1.5時間 |
| 11:30〜 | Step 3 Roughcut (波②〜③〜REC FLASH〜FINALE) | 1.5時間 |
| 昼休み 13:00〜 | 昼食 + 頭をリフレッシュ | 1時間 |
| 14:00〜 | Step 4 カラーグレーディング (全パート) | 3時間 |
| 17:00〜 | Step 5 オーディオミキシング | 1時間 |
| 18:00〜 | Step 6 グラフィック・テロップ | 1時間 |
| 19:00〜 | Step 7 ファインカット (通し確認) | 1時間 |
| 20:00〜 | Step 8 書き出し (本編4K) | 30分 |
| 20:30 | 完成 🎬 |  |

SNS Shorts (Step 8-2) は翌日以降でも可。

---

## トラブルシューティング

| 問題 | 対処 |
|-----|------|
| クリップが見つからない | SSD接続確認 → Resolve でリンク先を再指定 |
| S-Log3 の色が暴れる | CST ノードを確認 → Input 設定が正しいか |
| 音声が出ない | タイムライン オーディオトラックのミュートを確認 |
| 書き出しが遅い | Deliver → GPU 処理を有効に (Preferences → GPU) |
| 4K 再生が重い | Proxy 作成 (Media Pool → 右クリック → Generate Proxy Media) |

---

## ✅ うまく行ったこと

- 撮影スクリプトにパート別のカラーグレーディング方針が書いてあったので DaVinci の設定が迷いなく決まる
- BGM先置き → カット割り後付けの順序は「音とカット割りがズレない」最強パターン

## ❌ 詰まったこと

- DaVinci の CST 設定はカメラ別に異なる。FX30 (S-Log3) と R5 (C-Log3) を混在させると初心者は混乱しやすい
- 120fps 素材のスピードランプは「Retime Controls」から Speed Point を追加する手順を間違えやすい

## 📋 次回同じことをするときのチェックリスト

- [ ] DaVinci 起動前に外付けSSD 接続
- [ ] Step 1 でフレームレートを 24fps に設定 (デフォルトが違う場合あり)
- [ ] Step 2 でビン構造を作ってからインポート (後から整理するのは地獄)
- [ ] カラーグレーディングは必ず CST 先 → LUT → カスタムの順
- [ ] 書き出しファイル名に `_v01` をつける (再書き出し時に上書き事故を防ぐ)
- [ ] 書き出し後: YouTube / Insta の推奨コーデック設定と一致しているか確認
