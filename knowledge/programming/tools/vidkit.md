---
type: knowledge
domain: programming/tools
created: 2026-05-18
last_updated: 2026-05-18
tags: [vidkit, video, claude-code, ffmpeg, fcpxml, davinci]
status: active
---

# vidkit — 動画前処理 CLI ツール

> 動画 → Claude Code が分析可能な素材に前処理するためのCLIツール
> YD専用、ローカル稼働、Claude Max プランの枠で運用 (API課金回避)

## 🎯 目的

YD用途の動画作業を、Claude Code が直接扱える形に変換する:

- ダンス映像 → シーン分割 + フレーム抽出 + メタデータ (`dance` モード)
- 講義動画 → 字幕 + 話者分離 + コーネル式まとめ (`lecture` モード) ※未完成
- 編集対象動画 → 無音カット済みFCPXML (`autocut` モード) 2026-05-18 追加

## 📂 プロジェクト構成

**パス**: `/Users/ittou/projects/vidkit`

```
vidkit/
├── pyproject.toml          # uv パッケージ管理
├── README.md
├── install.sh
├── .env.example            # HF_TOKEN 等
└── vidkit/
    ├── cli.py              # Typer CLI エントリポイント
    ├── pipeline.py         # asyncio.gather 並列処理オーケストレーター
    ├── modules/
    │   ├── fetch.py        # yt-dlp で動画取得
    │   ├── audio.py        # ffmpeg で音声抽出
    │   ├── transcript.py   # MLX-Whisper (large-v3) で文字起こし
    │   ├── scenes.py       # PySceneDetect でシーン検出
    │   ├── diarize.py      # pyannote.audio (community-1) で話者分離
    │   └── merge.py        # 結果統合
    ├── silence.py          # ffmpeg silencedetect ラッパ (autocut用) ★新規
    ├── fcpxml.py           # FCPXML 1.13 シリアライザ (autocut用) ★新規
    └── prompts/
        ├── dance.md
        ├── lecture_brief.md
        ├── standard.md
        └── deep.md
```

## 🛠 スタック

- **言語**: Python 3.x
- **パッケージ管理**: uv (Astral)
- **CLI**: Typer
- **並列処理**: asyncio.gather
- **動画取得**: yt-dlp
- **音声処理**: ffmpeg, ffprobe
- **文字起こし**: MLX-Whisper (large-v3 デフォルト)
- **話者分離**: pyannote.audio (community-1 モデル)
- **シーン検出**: PySceneDetect
- **無音検出**: ffmpeg silencedetect (autocut)
- **FCPXML生成**: 自作シリアライザ (autocut)

## 🎬 モード一覧

### 1. dance モード ✅完成

ダンスMV・パフォーマンス動画の分析向け。

**処理内容**:
- シーン検出メイン
- フレーム抽出 (1080p維持)
- 音声トラックも字幕化
- meta.json、scenes.md、frames/、PROMPT.md を出力

**実機テスト**:
- 2026-05-18: SaveClip.mp4 (9秒、tinyモデル)
- 2026-05-18: TIME_Instagram_最終２.mp4 (4K 60fps、15秒、large-v3)
  - 12カット検出、平均1.3s、最長3.8s、最短0.3s
  - 処理時間: 281秒 (モデルDL含む)

### 2. lecture モード ⏳未完成

講義・トーク動画の分析向け。

**処理内容 (予定)**:
- 字幕生成メイン
- 話者分離 (pyannote)
- コーネル式ノート補完用フォーマット

**残課題**:
- pyannote の HF_TOKEN セットアップ
- lecture_brief / standard / deep プロンプトの調整

### 3. autocut モード ✅完成 (2026-05-18 追加)

FCP用無音カット FCPXML 生成。

**処理内容**:
- ffmpeg silencedetect で無音区間を検出
- keep-segment を計算
- FCPXML 1.13 形式で出力 (フレーム精度の有理数時刻計算)

**プリセット**:

| プリセット | threshold | min silence | pad | 用途 |
|----------|-----------|-------------|-----|------|
| `lecture` | -30dB | 0.4s | 0.1s | 講義・トーク |
| `vlog` | -35dB | 0.8s | 0.2s | Vlog・インタビュー |

**プリセット上書き** (各フラグで個別指定可):
- `--silence-threshold` (例: -40)
- `--min-silence` (例: 0.6)
- `--pad`

**検証結果 (12秒テストクリップ、30fps、3発話+2無音)**:

| プリセット | keep数 | 残し時間 | 削除時間 |
|----------|-------|---------|---------|
| lecture | 3 | 6.4s | 5.6s |
| vlog | 3 | 6.8s | 5.2s |

**XML検証**:
- ✅ xmllint --noout 通過
- ✅ frameDuration 100/3000s (30fps NDF)
- ✅ 全 offset/start/duration がフレーム整列 (100の倍数 / 3000)
- ✅ offset が累積和 (0 → 6300 → 12900)
- ✅ media-rep src が絶対 file:// URL
- ✅ audioRate が文字列 44.1k (sequence) と整数 44100 (asset)

**Skill登録**: `~/.claude/skills/fcp-autocut/SKILL.md`
- FCP/FCPXML/無音カット等のキーワードで自動ロード

**使い方**:
```bash
# Claude Code から
「~/Movies/talk.mp4 をFCP用に無音カットして」

# 内部実行
uv run vidkit autocut <video> --preset lecture
```

**FCP起動**: 自動起動しない。パス表示のみ。ユーザーが「開いて」と言ったら:
```bash
open -a "Final Cut Pro" /path/to/autocut.fcpxml
```

## 📤 出力先

`~/Downloads/vidkit_YYYY-MM-DD_<mode>_<id>/`

例: `~/Downloads/vidkit_2026-05-18_autocut_autocut-test/autocut.fcpxml`

将来的に `--vault-path` オプションで `~/ObsidianVault/raw/transcripts/` への直接出力を予定。

## 🔮 今後の拡張候補

### 優先度高: tutorial モード

Webサイト制作チュートリアル動画 → Markdown手順書 + Claude Code 実装まで一気通貫。
未設計。

### FCPXML ラウンドトリップ (2026-05-18 採用決定)

詳細は [[2026-05-18_FCPXML_ラウンドトリップ採用]] を参照。

候補オペレーション:

| オペレーション | 内容 | 優先度 |
|-------------|------|------|
| **tighten** | FCPで荒く並べた素材の各クリップ内の残り無音をさらに詰める | ★★★ |
| speaker-filter | 話者ベースで残す/削る (diarization要) | ★★ |
| marker-batch | トランスクリプトのキーワードでチャプターマーカー一括挿入 | ★★ |
| beat-snap | 既存カット点を音楽ビートにスナップ (蛹用途寄り) | ★ |
| roles-bulk | 全クリップに同じFCP rolesを付与 (ステム分離用) | ★ |

**最小単位**: FCPXMLリーダー + tighten オペレーション。これが「Claudeに FCP の中をいじってもらう」最初の実装。

### Obsidian Vault 連携

`--vault-path` オプションで `~/ObsidianVault/raw/transcripts/` への直接出力。
将来的に hook で「動画処理 → Vaultに自動保存」フローを構築。

## ⚠️ 注意点

### Mac環境 (M5 Max 36GB) 向け最適化済み
- 1080p フレーム維持
- large-v3 モデルデフォルト
- 大きなモデルでも余裕

### lecture モードを使うには HF_TOKEN が必要
`.env` ファイルに HuggingFace のトークンを設定 (pyannote使用のため)。
未設定だと話者分離が動かない。

### 動画ファイルパスの扱い
日本語・スペース・特殊文字を含むパスでも動作することを確認済み。

## 🔗 関連

- [[2026-05-18_FCPXML_ラウンドトリップ採用]] — FCP操作方式の意思決定
- [[claude_code]] (knowledge/programming/tools/) — Claude Code 全般
- `~/.claude/skills/fcp-autocut/SKILL.md` — Skill定義
- `/Users/ittou/projects/vidkit/` — プロジェクト本体

## 📝 メンテナンス

新機能追加・モード追加のたびにこのファイルを更新する。
意思決定が伴う場合は `decisions/` にも記録。

---

## ✅ うまく行ったこと

### 全体設計
- **uv + Typer の組み合わせ**: パッケージ管理とCLI構築が爆速で進んだ。新しいモード追加も既存パターンを踏襲するだけで済む。
- **asyncio.gather 並列処理**: 4Kの15秒動画を281秒で完走 (シーン検出 + 文字起こし + フレーム抽出が同時並行)。シーケンシャル処理なら倍以上かかってた。
- **モード制で機能拡張**: dance / lecture / autocut が同じCLIで使い分けられる。新機能追加時に既存機能を壊さない。

### dance モード
- **MLX-Whisper (large-v3) が爆速かつ高精度**: M5 Max のNeural Engine をフル活用、CPUより数倍速い。
- **PySceneDetect のシーン検出が想像以上に正確**: ダンスMVの細かいカット (0.3秒の最短カット) も拾えた。
- **4K 60fps 維持で1080pフレーム抽出**: 画質劣化なしで分析素材に変換。

### autocut モード (新規)
- **既存 vidkit インフラの流用**: ffmpeg, pipeline.py, cli.py を流用、ゼロから書いたのは silence.py と fcpxml.py だけ。
- **FCPXML 1.13 シリアライザがバグなく一発で動作**: 有理数時刻計算 (frameDuration 100/3000s) もフレーム整列も完璧。
- **xmllint --noout 通過**: XMLの構造的正しさが保証された。
- **2プリセット (lecture/vlog) で実用カバー率高い**: トーク用 (-30dB / 0.4s) と Vlog用 (-35dB / 0.8s) で大半のユースケース対応。
- **Skill 化で自動ロード成功**: `~/.claude/skills/fcp-autocut/SKILL.md` に置くだけで、FCP/FCPXML/無音カット等のキーワードでClaude Codeが自動ロード。

### Plan エージェント (Opus 4.7) の活用
- 実装前に **設計を Plan エージェントに検証してもらった** → 後の手戻りゼロ。

## ❌ 詰まったこと

### dance モード
- **初回 large-v3 モデルのダウンロードが遅い**: 281秒のうち実質5分はモデルDL。初回だけなので2回目以降は問題ないが、最初焦った。
- **pyannote の HF_TOKEN セットアップが面倒**: HuggingFace でゲート付きモデルのリクエスト承認待ちが発生。lecture モード未完成の主因。

### autocut モード
- **FCPXML の audioRate の型が場所によって違う**: sequence では文字列 "44.1k"、asset では整数 44100。これは FCPXML 1.13 の仕様で、最初は統一しようとしてXMLパースエラーになった。
- **frameDuration の有理数表現**: 30fps NDF は 100/3000s と書く必要がある。1/30 と書くとフレームずれが発生。
- **絶対 file:// URL 必須**: media-rep src を相対パスにすると FCP が見つけられない。

### 一般的な詰まり
- **「FCP内でClaudeが操作」の幻想**: AppleScript はFCPでは事実上使えない、computer-use MCP もピクセル精密制御は脆い → 結局 FCPXML ラウンドトリップしか実用解がない、と気づくのに時間がかかった (詳細: [[2026-05-18_FCPXML_ラウンドトリップ採用]])。

## 📋 次回同じことをするときのチェックリスト

### vidkit の新モードを追加するとき

- [ ] **既存パターンを踏襲する**: `cli.py` に新モード追加 → `pipeline.py` に分岐追加 → `modules/` か直下に新モジュール追加
- [ ] **prompts/ に対応プロンプトを置く**: モードごとに使うClaude向け指示
- [ ] **Skill登録するか判断**: 頻繁に使うなら `~/.claude/skills/<name>/SKILL.md` を作る (Claude Codeが自動ロード)
- [ ] **テストクリップを用意**: ffmpegの lavfi で小さな合成テスト動画を作って検証 (実動画前)
- [ ] **xmllint で検証**: XML出力するモードなら必須
- [ ] **プリセットを2つ以上用意**: 1つだとカスタマイズが面倒、3つ以上だと選択肢過多

### autocut を本番動画で使うとき

- [ ] 動画ファイルパスに**日本語/スペース/特殊文字が含まれてないか**確認 (一応動くが、念のため)
- [ ] プリセット選択: トーク・講義系 → `lecture`、Vlog・インタビュー → `vlog`
- [ ] 必要なら閾値カスタマイズ: `--silence-threshold` (dB)、`--min-silence` (秒)、`--pad` (秒)
- [ ] 出力された FCPXML は **新規プロジェクトとして** FCP にインポート (既存に上書きしない)
- [ ] FCPで開く: `open -a "Final Cut Pro" /path/to/autocut.fcpxml`
- [ ] **FCPの自動起動はしない仕様**なので、起動忘れに注意

### FCPXML 周りで新機能を作るとき

- [ ] `audioRate` の型: sequence は文字列、asset は整数 (混同すると壊れる)
- [ ] `frameDuration` は有理数表記: 30fps NDF なら `100/3000s`、24fps なら `100/2400s`
- [ ] `media-rep src` は**必ず絶対 file:// URL**
- [ ] `offset` は前クリップの累積和、`start` は元動画内の開始時刻、`duration` はクリップの長さ
- [ ] フレーム整列: `duration` は frameDuration の整数倍にする
- [ ] 検証: `xmllint --noout` で構造的正しさ確認 + 実機 FCP でインポートテスト

### lecture モードを完成させるとき (将来)

- [ ] HuggingFace で pyannote/speaker-diarization-community-1 のリクエスト承認を待つ (数時間〜数日)
- [ ] `.env` に `HF_TOKEN=<token>` を設定
- [ ] テストクリップで動作確認: 複数話者音声を用意
- [ ] プロンプト (lecture_brief.md, standard.md, deep.md) を実講義音声で調整

---

## 🔗 関連 (再掲)
