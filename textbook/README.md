---
type: textbook_index
last_updated: 2026-05-20
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
| 02 | [VercelとNext.jsでサイトを公開するとは何が起きてるのか](03_ai_engineering/02_vercel_nextjs_deploy.md) | ✅ 完成 (2026-05-20) |
| 03 | [AI はなぜ正しくて、なぜ間違えるか](03_ai_engineering/02_ai_capabilities_and_limitations.md) | ✅ 完成 (2026-05-20) — AI Capabilities and Limitations |
| 04 | [AI と仕事をする技術 — 4D フレームワーク](03_ai_engineering/03_ai_fluency_framework.md) | ✅ 完成 (2026-05-20) — AI Fluency: Framework & Foundations |
| 05 | [Claude の全機能を30分で把握する](03_ai_engineering/04_claude_101.md) | ✅ 完成 (2026-05-20) — Claude 101 |
| 06 | [Claude が自律的に仕事をする仕組み — Cowork 入門](03_ai_engineering/05_intro_to_claude_cowork.md) | ✅ 完成 (2026-05-20) — Introduction to Claude Cowork |

### 04_tools/ — 道具箱 (Vercel/Firebase/Supabase等)
| # | タイトル | 状態 |
|---|---------|-----|
| - | (Vercel / Firebase / Supabase / Git運用、今後追加) | 未着手 |

---

## 🎓 AI学習スプリント進捗 (2026-05-19 〜 06-02)

| Day | コース | 教科書 | 受講状態 |
|-----|--------|--------|---------|
| Day 1 (5/19) | AI Capabilities and Limitations | [03](03_ai_engineering/02_ai_capabilities_and_limitations.md) | 📖 教科書作成済・受講待ち |
| Day 1 (5/19) | AI Fluency: Framework & Foundations | [04](03_ai_engineering/03_ai_fluency_framework.md) | 📖 教科書作成済・受講待ち |
| Day 2 (5/20) | Claude 101 | [05](03_ai_engineering/04_claude_101.md) | 📖 教科書作成済・受講待ち |
| Day 2 (5/20) | Introduction to Claude Cowork | [06](03_ai_engineering/05_intro_to_claude_cowork.md) | 📖 教科書作成済・受講待ち |
| Day 3 (5/21) | Claude Code 101 | - | ⏳ 未着手 |
| Day 3 (5/21) | Claude Code in Action | - | ⏳ 未着手 |
| Day 4-7 | API → MCP → MCP Advanced → Agent Skills | - | ⏳ 未着手 |
| Week 2 | Cloud + AI Fluency 業界版 | - | ⏳ 未着手 |

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

---

## 🪴 第3号以降の提案リスト (執筆候補)

セッション02が並列実行中に思いついた次のテーマ。優先度はYDが決める。

### 候補A群 — 並列セッションの流れを延長
| 候補 | タイトル案 | 領域 | 根拠 |
|------|----------|-----|------|
| A-1 | Gitの「ブランチ」って何 ? — 並列セッションが衝突しない理由の続編 | 03_ai_engineering | 第1号で `git` を出したが深掘りしていない |
| A-2 | プレビューURLと本番URL — Vercelの「2つの世界」を使い分ける | 04_tools | 第2号でPreviewに少し触れたが詳細未説明 |
| A-3 | 環境変数とは何か — `.env` を絶対にGitHubに上げてはいけない理由 | 04_tools | セキュリティ系。Vercel運用で必須 |

### 候補B群 — Salamat WBS 実例ベース
| 候補 | タイトル案 | 領域 | 根拠 |
|------|----------|-----|------|
| B-1 | Reactの「コンポーネント」とは — Salamatの `Card` から覚える | 01_web_development | 第2号でReactは紹介済、もう一段深掘り |
| B-2 | カスタムドメインの繋ぎ方 — `salamat-jp.com` をVercelに向ける手順 | 04_tools | 本番運用での次のステップ |
| B-3 | フォーム送信とSupabase — お問い合わせを受け取る最小構成 | 01_web_development | Salamat次フェーズで必要 |

### 候補C群 — AI / ターミナル基礎
| 候補 | タイトル案 | 領域 | 根拠 |
|------|----------|-----|------|
| C-1 | Claude Code の権限ポリシー — `--dangerously-skip-permissions` って何 | 03_ai_engineering | 第1号で予告したが未着手 |
| C-2 | MCPとは何か — AIに「外の世界」を見せる標準 | 03_ai_engineering | 第1号の用語集で名前だけ出した |
| C-3 | `cron` 完全攻略 — 毎朝7:30に何かを動かす全パターン | 04_tools | 第1号で軽く触れたが詳細未説明 |

**推奨次回**: A-1 (Gitブランチ) → 第1号と第2号の橋渡しになる。
ただしYDがSalamat次フェーズを優先するならB-2 (カスタムドメイン) を推す。
