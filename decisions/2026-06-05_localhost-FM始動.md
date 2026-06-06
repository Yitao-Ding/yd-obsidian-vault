---
type: decision
date: 2026-06-05
tags: [youtube, bgm, localhost-fm, suno, content-creation]
---

# localhost FM 始動 — コーディングBGM YouTubeチャンネル第1弾完成

## 決定

@productivityonyt 系の「コーディング/バイブコーディングが捗るBGM」YouTubeチャンネルを
**localhost FM** の名前で始動。第1弾動画 `localhost-fm-mix-01.mp4`(53.39分)を完成させ、公開直前まで到達。

- パス: `/Users/ittou/projects/youtube bgm`(正本=`HANDOVER.md` / 公開メタ=`PUBLISH.md`)
- 音源: **Suno(Pro)** で13曲生成(WAV)。商用利用はProティアのみ・public feed非投稿。
- 公開はYD手動(本決定時点で未公開)。

## なぜこの作り方にしたか

### 1曲ループではなく「短い曲13本を1時間に連結」
- ループより飽きにくく、Content-IDのループ判定も避けやすい(Productivity FM 等も同方式)。
- 13曲を **エナジーアーク**(静→フロー→ピーク→着地)で並べ、DJセット風に6秒クロスフェード連結。

### 並び順は感覚でなく実データで決定
- librosaで **BPM・RMSエネルギー・スペクトル重心(明るさ)** を全曲解析。
- 結果: 入口=After Hours Focus(最も静か)→ 123bpmフロー群 → Workbench Bloom II(最も明るい)で持ち上げ
  → Neon Fruit Market(136)で加速 → **143.6bpmピーク×3(ネオン端末→ハイプ最高潮→ネオン端末1)**
  → 降下 → Terminal Drift(108)で着地。
- `-2` 版(同プロンプト別テイク)は全ペア**非隣接**に配置(連続再生の単調さ回避)。

### 音量は -14 LUFS に統一(YouTube基準)
- 元は -13.3〜-16.6 LUFS とばらつき → linear二段階loudnormで全曲 -14.0 に。
- 連結後も統合 -14.0 LUFS / トゥルーピーク -1.4 dBFS(クリップ無し)/ LRA 3.7 LU を確認。
- -14に揃えるとYouTube側で勝手に音量を下げられない。

### ブランド一貫性
- 右下に点滅 `localhost█`(coralカーソル)を動画に焼込=どの瞬間を切り取ってもブランドが乗る。
- 背景=深夜デスク(MacBook+Monster+ヘッドホン、コード表示)。サムネ=同シリーズの「ship」ロックアップ。

## 収益化・著作権の方針(リサーチ結論)

- 公開前に **YouTubeの著作権チェッカーを必ず通す**(AI曲でも誤マッチがありうる)。
- **自分の曲をContent IDに登録しない**(AI生成物は権利主張不可・誤クレームの的になるだけ)。
- AI開示トグルは“現実誤認”用途向けで本動画は非該当。説明欄に「generated with Suno」明記で誠実運用。
- 生データ垂れ流しは2025-07の方針で非推奨 → オリジナル編成+独自ビジュアル+チャプター+編集で付加価値側に。

## 次

- YDがアップロード → 公開後URLで **02本目** の構成提案 / 9:16 Shorts抜粋(ピーク43:25〜縦切り)。

## 関連
- [[localhost_fm]] — 制作パイプライン詳細(知識ノート)
- `youtube bgm/HANDOVER.md` / `youtube bgm/PUBLISH.md`
- [[claude_design]](ブランドキット由来)
