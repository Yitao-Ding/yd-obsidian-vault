---
name: fcp-tighten
description: 既存 Final Cut Pro プロジェクト (Export XML) の各クリップ内に残る無音をさらに詰める FCPXML ラウンドトリップ (vidkit tighten)。「ラフカットをもっと詰めたい」「FCPプロジェクトの無音を再カット」の依頼で使用。
---

# fcp-tighten — 既存 FCP プロジェクトの無音再カット

`vidkit tighten` は FCPXML → FCPXML のラウンドトリップ。autocut が「単一動画→FCPXML」なのに対し、こちらは FCP で並べ済みのプロジェクトが対象。

> ⚠️ 2026-08-11 MacBook Neo 移行後の正パス: **`~/Projects/vidkit`**
> 詳細手順の正本: `~/Projects/vidkit/docs/tighten-howto.md`

## 全体フロー

```
[FCP] File → Export XML (Cmd+E) → rough.fcpxml
   ↓
[shell] uv run vidkit tighten rough.fcpxml --preset lecture
   ↓
[FCP] File → Import → XML… で tighten.fcpxml を取り込み (新規 Event 推奨)
```

## コマンド

```bash
cd ~/Projects/vidkit

# 講義・トーク系
uv run vidkit tighten ~/Downloads/rough.fcpxml --preset lecture

# Vlog・対談 (緩め)
uv run vidkit tighten ~/Downloads/rough.fcpxml --preset vlog

# 閾値カスタム (例: -25dB / 0.3秒以上の無音のみ)
uv run vidkit tighten ~/Downloads/rough.fcpxml --preset lecture \
  --silence-threshold -25 --min-silence 0.3

# Vault へ直接出力
uv run vidkit tighten ~/Downloads/rough.fcpxml --preset lecture --vault-path ~/ObsidianVault
```

## FCP Export XML の注意 (YD に案内)

1. Browser で対象プロジェクトを選択してから Cmd+E
2. Metadata view は「Final Cut Pro Metadata View」のまま
3. 保存先は `~/Downloads/` などわかりやすい場所

## 出力

- デフォルト: `~/Downloads/vidkit_<日付>_tighten_<id>/tighten.fcpxml` + `meta.json` (per-clip 統計)
- 実行中に「N → M clips, removed X.Xs」の統計が出る

## 注意

- 素材動画ファイルにアクセスできる状態で実行する (無音解析に音声が必要)
- variable-fps 動画対応済み
- 単一動画からの初回カットは **fcp-autocut** スキルを使う
