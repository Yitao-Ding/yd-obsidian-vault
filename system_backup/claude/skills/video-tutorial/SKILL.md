---
name: video-tutorial
description: チュートリアル動画 (YouTube URL / ローカル) を vidkit tutorial で前処理し、Claude Code が内容を自走実装できる PROMPT.md を生成する。「この動画の内容を実装して」「チュートリアル動画を解析して」の依頼で使用。
---

# video-tutorial — チュートリアル動画 → 自走実装プロンプト

`vidkit tutorial` は動画 (URL/ローカル自動判別) を文字起こし+シーン抽出し、Claude Code が内容を再現実装するための `PROMPT.md` を生成する。

> ⚠️ 2026-08-11 MacBook Neo 移行後の正パス: **`~/Projects/vidkit`**

## 基本コマンド

```bash
cd ~/Projects/vidkit

# YouTube チュートリアル
uv run vidkit tutorial "https://www.youtube.com/watch?v=xxx"

# ローカル動画
uv run vidkit tutorial ~/Movies/tutorial.mp4

# YouTube字幕が悪い/無い場合: MLX-Whisper で再文字起こし
uv run vidkit tutorial "https://..." --source whisper --whisper-model tiny

# プロンプトタイプを明示 (auto | implementation | setup | explainer)
uv run vidkit tutorial "https://..." --prompt-type setup

# 既存出力があるときの上書き
uv run vidkit tutorial "https://..." --force
```

## 主要オプション

- `--source`: `auto` (既定) / `youtube` (公式字幕) / `whisper` (MLX-Whisper 再文字起こし)
- `--prompt-type`: `auto` / `implementation` (コード実装) / `setup` (環境構築手順) / `explainer` (概念解説)
- `--lang auto` は HTTP 429 回避済み (`bd1a24b`)
- `--vault-path ~/ObsidianVault` → `<vault>/raw/vidkit/tutorial/...` に出力
- `--force`: 既存出力ディレクトリを確認なしで上書き

## 出力

`~/Downloads/vidkit_<日付>_tutorial_<id>/` に:

- `PROMPT.md` — Claude Code に渡す自走実装プロンプト (これが主産物)
- `transcript.md` / `scenes.md` / `frames/` / `chapters.md` / `meta.json`

## ワークフロー

1. vidkit tutorial を実行して PROMPT.md を生成
2. PROMPT.md を読み、動画の内容 (実装/セットアップ/解説) を把握
3. implementation 系なら新規プロジェクトとして自走実装、setup 系なら手順を YD の環境に合わせて実行提案
4. 不明点は frames/ のスクリーンショットと transcript.md で補完
