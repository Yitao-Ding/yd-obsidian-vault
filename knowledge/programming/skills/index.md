---
type: knowledge_index
domain: programming/skills
last_updated: 2026-05-21
---

# Claude Code Skills 領域

YD のローカル `~/.claude/skills/` に設置している独立 skill (plugin 経由ではなく直設置) のメモと、UI 制作時の運用ルールを集約する。

## 📦 設置済み skill 一覧 (独立版)

### Frontend (UI/UX) 系 — 3 フェーズ併用運用

| Skill 名 | 用途 | 制作/レビュー | ノート |
|---------|------|------------|-------|
| ui-ux-pro-max | UI/UX 設計の参照知識 (67 styles, 96 palettes, 57 font pairings, 25 charts, 13 stacks) | 制作 | [[ui-ux-pro-max]] |
| frontend-design | 「AI slop」回避、distinctive で production-grade な frontend を作る design 哲学 | 制作 | [[frontend-design]] |
| web-design-guidelines | Vercel Web Interface Guidelines に基づく UI コードレビュー (WebFetch で最新ルール取得) | レビュー | [[web-design-guidelines]] |

### Backend 系 — 単独運用

| Skill 名 | 用途 | ノート |
|---------|------|-------|
| backend-patterns | Backend アーキテクチャパターン (API 設計 / DB 最適化 / Caching / Auth / Rate Limit / Jobs / Logging) | [[backend-patterns]] |

### その他 (動画系)

| Skill 名 | 用途 | ノート |
|---------|------|-------|
| video-tutorial | 動画チュートリアル → 実装素材化 | (vidkit 連携) |
| fcp-autocut | Final Cut Pro 用 silence cut | (vidkit 連携) |
| fcp-tighten | FCP project tighten | (vidkit 連携) |

> Anthropic 公式の `example-skills:frontend-design` / `document-skills:frontend-design` / Vercel plugin 由来の `web-interface-guidelines` も system-reminder に出てくるが、別物 (plugin 経由)。本 index は **`~/.claude/skills/` 直下に置いている自分管理の skill** に絞る。
> `web-design-guidelines` と `web-interface-guidelines` は機能ほぼ同等 (両者とも vercel-labs/web-interface-guidelines を参照)。現状は併存運用。詳細は [[web-design-guidelines]] 参照。

## 🎨 UI 制作/レビュー時の運用ルール (YD 指示, 2026-05-21)

YD の明示指示:

> ui-ux-pro-max と frontend-design を**並行して**使う。
> どちらか片方ではなく、**両者から最適なものを取ってくる**。「いいとこ取り」運用。

2026-05-21 追加: レビュー段階に [[web-design-guidelines]] を追加 → **3 フェーズ運用** に拡張。

### 3 skill の役割分担

| Skill | フェーズ | 担当 |
|-------|---------|-----|
| frontend-design | 制作 (方向性決定) | 美学コミット + アンチパターン排除 (Inter/Space Grotesk/紫グラデ禁止) |
| ui-ux-pro-max | 制作 (素材調達) | palette/font/chart/stack guideline のデータベース検索 |
| web-design-guidelines | レビュー (compliance) | Vercel Web Interface Guidelines に WebFetch で照合 → `file:line` 違反列挙 |

### 各 skill の強み

**frontend-design の強み**:
- 「distinctive / bold / 記憶に残る」を絶対基準にする**美学的方向性**
- 「generic AI aesthetics は禁止」「Inter / Roboto / 紫グラデは使うな」など**回避すべきアンチパターン**
- Typography / Motion / Spatial Composition / Background の**意図性**を求める

**ui-ux-pro-max の強み**:
- 構造化されたデータベース (CSV) で具体的な palette / font pairing / chart type を**検索ベースで引ける**
- スタック別 (React / Next.js / Vue / Svelte / SwiftUI 等) の guideline を持つ
- 「Product type → 推奨デザインシステム」の推論 (Design System Generator)
- 数値・カタログ的に確からしい選択を**外す**ためのレファレンス

**web-design-guidelines の強み**:
- **最新のガイドラインを WebFetch で取得**してから照合 (ローカルに古いルール集を抱えない)
- 出力は `file:line` の terse format → 修正対象が一発で分かる
- アクセシビリティ / UX / レイアウト / パフォーマンスの**事実ベースのチェック**

### Claude の動き方 (UI タスク時のチェックリスト)

#### 制作フェーズ
1. **方向性を 1 つに決める** (frontend-design): brutally minimal / maximalist / retro-futuristic / editorial / luxury / etc. から 1 つ commit
2. **その方向性に合う具体素材を引く** (ui-ux-pro-max): style / palette / font pairing / chart type を該当データベースで検索 (`python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain <domain>`)
3. **アンチパターンに引っかかってないか確認** (frontend-design): Inter / Space Grotesk / 紫グラデ / cookie-cutter になってないか
4. **実装は方向性に合わせて密度調整** (frontend-design): minimalist なら restraint, maximalist なら elaborate

#### レビューフェーズ (実装後 or 既存コード監査)
5. **web-design-guidelines を呼び出す** → 対象ファイル/パターンを指定 → 違反列挙を取得
6. 違反を **(a) frontend-design の美学優先で意図的に外した箇所** と **(b) 単純な見落とし** に仕分け
7. (b) は修正、(a) は意図メモを残す

### 禁止事項

- ui-ux-pro-max の検索結果を**そのまま貼る**だけの動き (= 方向性が無くて素材だけ並べる → generic 化)
- frontend-design の哲学だけで具体パレット/フォントを**毎回ゼロから捻り出す** (= 効率悪い、ui-ux-pro-max を使う意味が無い)
- 3 つの skill のうち**特定の skill しか参照せず**にタスク完了する
- 制作後に web-design-guidelines を**一度も通さず**「完成」と報告する (実装したら必ずレビューを 1 回回す)

## 🔧 Backend 作業時の運用ルール

[[backend-patterns]] は単独で動く skill。frontend 系のような併用相手はないが、以下を守る:

1. backend 作業 (API route / DB クエリ / 認証 / 認可 / cache / job) は **必ず 1 回呼ぶ**
2. **Rate limiting は絶対 in-memory にしない** (SKILL.md 明記の禁止事項、deploy reset / multi-instance split / serverless fail open)
3. N+1 が疑われるループを見たら、即座に batch fetch + Map 化
4. 新規 API は **Repository → Service → Route handler の 3 層分離**を最初から守る
5. プロジェクトのスタックが Supabase 以外 (Prisma / Drizzle / 別 DB) の場合は、Repository pattern の interface だけ流用、実装は読み替え

フルスタック (UI + API) タスクの時は、UI 側のファイルに 3 skill (frontend-design + ui-ux-pro-max + web-design-guidelines)、API 側のファイルに backend-patterns を**並列で適用**する。

## 📁 ソース管理

- `~/.claude/skills/ui-ux-pro-max/` — 稼働中 (2026-05-18 設置の SKILL.md 形式版)
- `~/.claude/skills/frontend-design/` — 稼働中 (2026-05-21 設置, YD 提供の SKILL.md)
- `~/.claude/skills/web-design-guidelines/` — 稼働中 (2026-05-21 設置, YD 提供の SKILL.md)
- `~/.claude/skills/backend-patterns/` — 稼働中 (2026-05-21 設置, YD 提供の SKILL.md, origin: ECC)
- `~/projects/ui-ux-pro-max-source/` — GitHub から git clone した CLI 配布物 (skill 本体ではなくソース参照用に退避)

## 📚 関連

- [[ui-ux-pro-max]] — UI/UX Pro Max skill の詳細
- [[frontend-design]] — frontend-design skill の詳細
- [[web-design-guidelines]] — web-design-guidelines skill の詳細
- [[backend-patterns]] — backend-patterns skill の詳細
