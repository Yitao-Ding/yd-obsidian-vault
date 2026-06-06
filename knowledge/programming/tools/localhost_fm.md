---
type: knowledge
domain: programming/content-creation
tags: [youtube, bgm, suno, ffmpeg, librosa, localhost-fm]
created: 2026-06-05
---

# localhost FM — コーディングBGM動画の制作パイプライン

YouTubeチャンネル **localhost FM**(コーディングが捗るBGM)の動画を1本作る手順とスクリプト群。
正本=`/Users/ittou/projects/youtube bgm/`(`HANDOVER.md` / `PUBLISH.md`)。

## 全体の流れ

```
Suno(Pro)で短い曲を10〜15本生成(WAV)→ tracks/ に投入
  → ① analyze-tracks.py   : BPM/エネルギー/明るさを解析(並べ順の根拠)
  → ② normalize-order.py  : 並べ順を固定しつつ -14 LUFS に正規化 → staging/NN_*.wav
  → ③ make-mix.sh         : 6秒クロスフェードで1本に連結 → mix.wav
  → ④ gen-chapters.py     : クロスフェード重複を引いた正確なチャプター
  → ⑤ render-final.sh     : 背景静止画+右下点滅バグを1パス焼込 → mp4
  → PUBLISH.md のタイトル/説明/タグで公開
```

## 各スクリプト(`scripts/`)

- **analyze-tracks.py** — `uv run --with librosa --with numpy`。各曲の `bpm` `energy`(RMS平均)
  `brightness`(スペクトル重心)`head_sil`/`tail_sil` を JSON 出力。
- **normalize-order.py** — 先頭の `ORDER` リストで並び順を定義(=エナジーアーク)。各曲を
  linear二段階loudnormで `-14 LUFS` に揃え、`staging/01_… 13_*.wav` へ。NNプレフィックスが連結順を固定。
  `-2`版など同プロンプト曲は隣接させない。全角/半角ゆらぎ(ネオン端末１)も吸収。
- **make-mix.sh** — `staging/` を名前順に `acrossfade=d=XF:c1=tri:c2=tri` で鎖状連結。`-x` でxfade秒。
  ★ macOS標準bash 3.2 は `mapfile` 非対応 → `while read`+`arr+=()` で実装済。
- **gen-chapters.py** — 各曲開始 = `Σ(dur_i, i<k) − k*XF`(acrossfadeは曲間をXF秒重ねる)。
  重複曲は II 付与・日本語は英語ラベル化。YouTube概要欄にそのまま貼れる。
- **render-final.sh** — `-loop 1` 背景画像を1920×1080にscale/crop、`overlay-on/off.png` を
  `enable='lt(mod(t,CYC),HALF)'` / `gte(...)` で交互表示=点滅。1パスでH.264 yuv420p+AAC 320k+faststart。

## 効いたノウハウ

- **データ駆動の並べ替え**: BPMの山(slow→fast→slow)+明るさで“静→ピーク→着地”を設計。感覚より再現性が高い。
- **-14 LUFS 統一**: YouTube基準。揃えると再生時に音量を下げられない。TPは -1.5 以下でクリップ回避。
- **クロスフェード総尺**: N曲・xfade XF秒なら `Σdur − (N-1)*XF`。13曲/6秒で 54.5分→53.4分。
- **コーナーバグ焼込**: 右下 `localhost█` 点滅で全フレームにブランド。overlay PNGは透過RGBA+影で実写でも可読。
- **検証**: `ffmpeg -af ebur128=peak=true -f null -` で統合LUFS/TP/LRAを最終確認してから公開。

## Suno プロンプト量産メソッド(2026-06-06 追加)

**同曲問題**: 1つのStyleプロンプトで何回生成しても似た曲しか出ない。→ 曲数分の「別Style」を持つのが正解。手書きは非効率なので **パーツ表(モジュール)方式** でライブラリ化。

**プロンプト在庫**(正本=プロジェクト直下):
- `suno-prompts.md` A〜D(house/synthwave/phonk/lofi)+ **E章 amapiano 7本**
- `suno-prompt-library.md` **F〜K 50本**(afro-latin / dnb / city-pop / uk-garage / afro拡張 / melodic-club)+ 冒頭に差し替えパーツ表
- 計 **57本**、全部 別BPM・別キー・別楽器・別ムード / 全曲 instrumental・no vocals / Content IDセーフ(ジャンル要素のみ)

**きっかけ**: Burna Boy "Don't Let Me Drown"(映画F1サントラ / amapiano / 114 BPM / C#m / prod. Kooldrink)。log drum と裏拍シェイカーの“跳ね”が「のれる/雄大」の核 → afro/amapiano系を量産の軸に据えた。

**Suno設定(More Options)の効かせ方**:
- **Style Influence** = ジャンルの“枠”をどれだけ守るか(揃える役)。要素が多い時は 60〜70。
- **Weirdness** = 枠の中でどれだけ暴れるか(2曲を違える役)。まとまり重視は 35〜45、2曲を大きく違えたい時だけ 50〜60。基本いじるのはこっちだけ。
- Exclude styles `lead vocals, rap, spoken word, cheesy EDM, harsh distortion` で歌・安っぽさを遮断。

**展開を付ける(平坦さ対策)= タグ法**: Instrumental OFF にして歌詞欄に **セクションタグだけ**(歌詞の単語ゼロ)を貼る。`[Intro][Build][Groove][Breakdown][Switch][Drop][Outro]` 等で中盤の切り替えを強制+amapianoのボーカルチョップを楽器として温存。歌い出したら Style 先頭に `[instrumental]`。

**長さ**: セクションタグを **10〜12個** にし、Style末尾に `extended mix, long intro and outro` を足すと **3〜4分**。生成は **Custom Mode + v5**(尺が安定)、足りなければ **Extend** で継ぎ足し。

**2曲仕様**: Sunoは1生成で必ず2曲出る(止められない/クレジットは2曲分)。要らない方を **Trash** で実質1曲運用。むしろ「2テイクから当たりを選ぶ」と捉えると量産向き。

**mix02 以降**: 57本を使い、回ごとにジャンル比率を変えて各動画を差別化。アーク例は `suno-prompt-library.md` 末尾。

## 環境メモ(このMac固有)

- ffmpeg 8.1 は **drawtext/freetype 無し** → テキストは Pillow で画像化して overlay する。
- bash は 3.2 のみ(homebrew bash無し)→ mapfile等のbash4機能は使わない。
- 画像生成は higgsfield CLI(Z Image=安い)、フォントは `fonts/IBMPlexMono-*.ttf`。

## 関連
- [[2026-06-05_localhost-FM始動]] — 始動の意思決定と著作権/収益化方針
- [[vidkit]] — 動画前処理CLI(別系統)
