---
name: fcp-autocut
description: 単一動画から無音区間を検出し、Final Cut Pro 用のカット済み FCPXML を生成する (vidkit autocut)。「無音カット」「ジェットカット」「FCPでカット編集の下ごしらえ」などの依頼で使用。
---

# fcp-autocut — 無音カット FCPXML 生成

`vidkit autocut` で単一動画の無音区間を検出し、無音を詰めたタイムラインの FCPXML (1.13) を生成する。YD はこれを FCP に Import して微調整→書き出しする。

> ⚠️ 2026-08-11 MacBook Neo 移行後の正パス: **`~/Projects/vidkit`** (旧 `/Users/ittou/projects/vidkit` は死んだパス)

## 基本コマンド

```bash
cd ~/Projects/vidkit

# 講義・トーク系 (タイトに詰める)
uv run vidkit autocut /path/to/video.mp4 --preset lecture

# Vlog・素のしゃべり (緩めに詰める)
uv run vidkit autocut /path/to/video.mp4 --preset vlog
```

## プリセット実値 (vidkit/silence.py)

| preset | noise_db | min_silence | pad | min_keep | merge_gap |
|--------|----------|-------------|-----|----------|-----------|
| lecture | -30dB | 0.4s | 0.10s | 0.30s | 0.20s |
| vlog | -35dB | 0.8s | 0.20s | 0.50s | 0.40s |

上書きオプション: `--silence-threshold -25` / `--min-silence 0.3` / `--pad` / `--merge-gap`

## 出力

- デフォルト: `~/Downloads/vidkit_<日付>_autocut_<id>/` に `autocut.fcpxml` + `meta.json` (統計)
- `--vault-path ~/ObsidianVault` 指定時: `<vault>/raw/vidkit/autocut/...`
- `--fcpxml-out` で FCPXML 出力パスを直接指定可

## FCP 側の手順 (YD に案内)

1. FCP → **File → Import → XML…** → 生成された `autocut.fcpxml` を選択
2. 取り込み先は新規 Event が安全
3. タイムラインを開いて微調整 (誤検出カットの復元など)

## 注意

- variable-fps 動画 (Zoom録画/画面収録) も対応済み (`40cef7d`)
- 詰めすぎ/緩すぎの調整は preset 切替 → だめなら閾値オプション、の順で
- 既存 FCP プロジェクトの再カットは autocut ではなく **fcp-tighten** スキルを使う
