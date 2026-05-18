---
type: textbook_index
last_updated: 2026-05-19
status: active
---

# 📚 YD専用教科書システム

> 「HTMLって何?」「ターミナル怖い」レベルから始める、YD専用のゼロ前提教科書。
> Markdownで書いて、`textbook-engine` で縦長PDFに変換し、iPhoneで通勤中に読む。

---

## 🎯 このシステムの目的

- **「動くもの」より「本物のクオリティ」** を求めるYDの哲学に沿った教材
- 専門用語は**初出時に必ず注釈**を入れる (ゼロ前提)
- 図解 (Mermaid) を必ず入れる
- 実コード + 日本語コメントで「使える知識」にする
- PDFはiPhone最適化された縦長レイアウト

---

## 📖 領域別目次

### 00_basics/ — まず最初にやるやつ
| # | タイトル | 状態 |
|---|---------|-----|
| - | (HTML / CSS / JS / Git / ターミナルの基礎、今後追加) | 未着手 |

### 01_web_development/ — Webアプリ作る系
| # | タイトル | 状態 |
|---|---------|-----|
| - | (Next.js / Vercel / WBS実例、今後追加) | 未着手 |

### 02_video_processing/ — 動画処理 (vidkit系)
| # | タイトル | 状態 |
|---|---------|-----|
| - | (vidkit / FCPXML / Python、今後追加) | 未着手 |

### 03_ai_engineering/ — AIを操る側になる
| # | タイトル | 状態 |
|---|---------|-----|
| 01 | [Claude Code 4並列で何が起きてるか](03_ai_engineering/01_claude_code_parallel.md) | ✅ 完成 (2026-05-19) |

### 04_tools/ — 道具箱 (Vercel/Firebase/Supabase等)
| # | タイトル | 状態 |
|---|---------|-----|
| - | (Vercel / Firebase / Supabase / Git運用、今後追加) | 未着手 |

---

## 🛠 教材を1冊PDF化する

```bash
# 初回セットアップ (一度だけ)
cd ~/projects/textbook-engine
./setup.sh

# 教材をPDFに変換
./build.sh ~/ObsidianVault/textbook/03_ai_engineering/01_claude_code_parallel.md
# → ~/ObsidianVault/textbook/_output_pdf/01_claude_code_parallel.pdf が生成される
```

詳細は `~/projects/textbook-engine/README.md` 参照。

---

## 📝 新しい教材を書くとき

1. `_template/textbook_template.md` をコピー
2. 対応する領域 (`00_basics/`, `03_ai_engineering/` 等) に置く
3. 番号付きファイル名にする (例: `02_xxx.md`)
4. 本README の目次表に追記
5. `build.sh` でPDF化
6. 完成PDFは `_output_pdf/` に出力される

---

## 📐 統一フォーマット (7セクション)

1. **何が起きた? (実例)** — 具体的なシーンから入る
2. **図解** — Mermaid 必須1つ以上
3. **キーコンセプト** — 3-5個
4. **コード解説** — 実コード + 日本語コメント
5. **次やる時のチェックリスト** — 再現可能な手順
6. **関連リンク** — 他教材・公式ドキュメント
7. **用語集** — 専門用語の注釈

---

## 🔗 関連

- [[textbook_engine]] — PDF生成パイプライン (`~/projects/textbook-engine/`)
- [[2026-05-19_AI学習スプリント開始]] — このシステム誕生の経緯
