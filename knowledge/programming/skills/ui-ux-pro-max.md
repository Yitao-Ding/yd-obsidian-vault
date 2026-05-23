---
type: knowledge
domain: programming/skills
tool: ui-ux-pro-max
last_updated: 2026-05-21
---

# ui-ux-pro-max skill

NextLevelBuilder 社の「UI UX Pro Max」(v2.5.0)。AI design intelligence カタログ。設置場所: `~/.claude/skills/ui-ux-pro-max/SKILL.md`

公式サイト: https://uupm.cc
リポジトリ: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill (CLI 配布物、別途参照可)

## 概要

UI/UX 設計用の**カタログ系 skill**。下記を**検索可能なデータベース**として持つ:

- 67 UI styles (glassmorphism, brutalism, neumorphism, bento grid, ...)
- 96-161 color palettes
- 57 font pairings (Google Fonts URL 込み)
- 25 chart types + library 推奨
- 99 UX guidelines
- 13-15 tech stack (React, Next.js, Vue, Svelte, SwiftUI, React Native, Flutter, Tailwind, shadcn/ui, ...)

検索 CLI (リポジトリの `scripts/search.py`) で `--domain` (product/style/typography/color/landing/chart/ux) や `--stack` を指定して BM25 + regex で検索する。

## 設置情報

- 現行版: 2026-05-18 設置 (SKILL.md + scripts/ + data/ の Claude Code skill 形式)
- 設置パス: `~/.claude/skills/ui-ux-pro-max/SKILL.md`
- ソース参照: `~/projects/ui-ux-pro-max-source/` (GitHub git clone 版、skill.json 形式の CLI 配布物。skill 認識はされない。データベース更新時の reference 用に保管)

### 注意: 2 つの配布形式

1. **Claude Code skill 形式** (現行設置): SKILL.md + scripts/ + data/ の構成。これが skill として認識される
2. **CLI 配布形式** (GitHub repo): skill.json + cli/ + src/ + docs/ の構成。公式 install は `npx uipro-cli init --ai claude` 経由
   - これを直接 `~/.claude/skills/` に置いても SKILL.md が無いので skill 認識されない

## frontend-design との関係

[[frontend-design]] と**併用必須** (YD 指示)。詳しい運用ルールは [[index]] 参照。

ざっくり:
- ui-ux-pro-max = 具体素材 (palette / font / chart / stack guideline) のデータベース検索
- frontend-design = 方向性決定 + アンチパターン避け

## ✅ うまく行ったこと

- 2026-05-18 設置版 (SKILL.md 形式) は skill として安定認識されている
- 「product type → 推奨デザインシステム」の推論 (Design System Generator) が**1 コマンドで一発回答**を出す設計
- スタック別 (React / SwiftUI / Flutter ...) で別個の guideline を持つ → 多言語マルチプラットフォームの YD のプロジェクトと相性 ◎

## ❌ 詰まったこと

- 2026-05-21: GitHub から git clone したものは **CLI 配布物 (skill.json 形式)** で、`~/.claude/skills/` に置いても skill 認識されない。skill 一覧から `ui-ux-pro-max` が一時的に消えた (バックアップを戻して復旧)
  - 教訓: 公式 install は `npx uipro-cli init --ai claude`。git clone は**ソース参照専用**
  - 関連: [[claude_mistakes]] A-x 追加候補 (Claude Code skill とリポジトリ配布物の形式差異を見落とした)

## 📋 次回同じことをするときのチェックリスト

1. `~/.claude/skills/` に skill を設置する時は **SKILL.md (frontmatter 付き) 必須**。それ以外の形式は認識されない
2. 公式リポジトリから取る時は README の install 方法を**先に読む**。CLI ベース (`npx ...`) のものを git clone しない
3. 既存 skill を入れ替える前に**必ずバックアップ** (`mv <name> <name>.bak-<date>`)。差し替えに失敗してもバックアップ側が引き続き skill 認識される (skill 一覧で `<name>.bak-<date>` として見える)
4. UI タスクで使う時は必ず [[frontend-design]] と**併用** — index.md の運用ルールを毎回チェック
5. データ更新 (palette / font / etc.) を引きたい時は `~/projects/ui-ux-pro-max-source/` の git pull で最新を確認

## 📚 関連

- [[frontend-design]] — 併用相手
- [[index]] (knowledge/programming/skills/) — 運用ルール集約
