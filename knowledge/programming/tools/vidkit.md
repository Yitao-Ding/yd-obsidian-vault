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
