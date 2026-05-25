---
type: current_state
last_updated: 2026-05-21
priority: highest
update_frequency: スキル/MCP追加時
---

# 利用可能な機能カタログ (スキル + MCPコネクタ)

> 「忘れる」問題を防ぐため、毎セッション開始時に必ず読む。
> 当日のアクティブタスクに対して、ここから機能候補をマッピングする演習を行う。
>
> ★ スキル/MCP は毎セッション system-reminder で**自動注入**される。本ファイルは「いつ何を使うか」のマッピング表として永続化する。
> 詳細仕様は session 内 system-reminder の最新を参照すること。

---

## 🛠 グローバルスキル

### 動画・映像系

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `video-tutorial` | YouTube URL / ローカル動画 → Claude Code が自走実装する素材セット (字幕+シーン分割+代表フレーム) | チュートリアル動画, follow along, 動画見て作って, YouTube URL, ライブコーディング, screencast |
| `fcp-autocut` | 単一動画から無音区間除去 → FCPXML 1.13 出力 | FCP, FCPX, 無音カット, ジャンプカット, talking head, vlog, lecture, podcast 編集 |
| `fcp-tighten` | FCP内クリップの残り無音を再カット (Export XML → tighten → Import XML) | FCP tighten, クリップ内無音, ラフカット詰め直し, FCPXML ラウンドトリップ |

### UI/UX 設計 (2026-05-21 拡張、3 フェーズ併用運用)

> 詳細運用ルール: [[index]] (knowledge/programming/skills/) — 制作: frontend-design + ui-ux-pro-max / レビュー: web-design-guidelines を**必ず3つとも参照**。片方だけで完成報告は禁止。

| スキル名 | フェーズ | 用途 | トリガー語彙 |
|---------|---------|------|------------|
| `ui-ux-pro-max` | 制作 (素材) | 67スタイル/96パレット/57フォントペア/13スタック (React/Next/Vue/Svelte/SwiftUI/RN/Flutter/Tailwind/shadcn) のデータベース検索 | UI/UX, ダッシュボード, ランディング, glassmorphism, brutalism, bento, shadcn, color palette |
| `frontend-design` | 制作 (方向性) | 「AI slop」回避、distinctive で production-grade な美学コミット (Inter/Roboto/Space Grotesk/紫グラデ禁止) | distinctive, bold, AI slop 回避, 美学方向性, brutally minimal, maximalist, editorial |
| `web-design-guidelines` | レビュー | Vercel Web Interface Guidelines に WebFetch で照合 → `file:line` 形式で違反列挙 | UI レビュー, アクセシビリティチェック, デザイン監査, UX 見て, best practices |

### Backend 開発 (2026-05-21 追加、単独運用)

> 詳細: [[backend-patterns]]。origin: ECC = "Everything Claude Code" (`affaan-m/everything-claude-code`, Anthropic Hackathon Winner 2026/2月、60 agents + 232 skills 中の1つ)。

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `backend-patterns` | Backend アーキテクチャパターン (API設計 / DB最適化 / N+1防止 / Caching / Auth / Rate Limit / Background Jobs / Structured Logging)。Supabase + Next.js App Router 中心。**Rate limit は絶対 in-memory 禁止** (deploy reset/multi-instance split/serverless fail open) | API設計, REST endpoint, DB クエリ最適化, N+1, キャッシュ層, JWT, RBAC, rate limit, Repository pattern |

### Claude / API

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `claude-api` | Anthropic SDK 利用 (プロンプトキャッシュ込み)、モデル間マイグレーション (4.5→4.7) | anthropic SDK, claude-* model, prompt caching, batch API, Anthropic Academy 実装 |

### Claude Code ネイティブ機能 (実験的、2026-05-21 追加)

> `~/.claude/settings.json` の `env` で有効化する Claude Code 本体の実験的機能。スキルではなく**機能**で、deferred tools として注入される。詳細運用は [[claude_code_agent_teams]] を参照、棲み分けはバッチ系=[[parallel_claude]] / 議論系=エージェントチーム。

| 機能 | 用途 | 有効化 | トリガー語彙 |
|------|------|-------|------------|
| **エージェントチーム** | リーダー + メンバー型の並列セッション (互いに直接通信可、共有タスクリスト、独立コンテキスト)。`TeamCreate` / `SendMessage` / `TeamDelete` の deferred tools が利用可能 | `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` (v2.1.32+)、分割ペインに tmux/iTerm2+it2 必要 | エージェントチーム, 複数視点で議論, devil's advocate, 5人で並列調査, multi-perspective, 相互検証 |

### 自動化・運用

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `loop` | プロンプト/スラッシュコマンドを定期実行 | check every X minutes, keep running, polling, babysit |
| `schedule` | 遠隔 Claude Code エージェントを cron 実行 (routines) | cron, 定期実行, scheduled agent, 朝に動かす |
| `update-config` | ~/.claude/settings.json 編集 (hooks, permissions, env) | from now on when X, allow X, hook を追加, env var |
| `keybindings-help` | ~/.claude/keybindings.json 編集 | キーバインド, rebind, chord shortcut |
| `simplify` | 変更コードの再利用・品質・効率レビュー → 修正 | simplify, refactor, コードクリーンアップ |
| `fewer-permission-prompts` | transcripts からよく使う read-only Bash/MCP を allowlist | permission prompt 多い, allow X |
| `init` | CLAUDE.md を新規作成 | init project, CLAUDE.md create |
| `review` | プルリクレビュー | PR review |
| `security-review` | 現ブランチ差分のセキュリティレビュー | security review, XSS, SQL injection, OWASP |

### Anthropic 公式 Agent Skills (2026-05-21 追加、`example-skills:*` / `document-skills:*` 両 namespace で同内容)

> マーケットプレイス: `anthropics/skills` (公式)
> プラグイン: `example-skills@anthropic-agent-skills`, `document-skills@anthropic-agent-skills`
> 両プラグインは**同一スキルセット**を別 namespace で提供。実用上は片方 (例えば `document-skills:*`) を主に使えば十分。

#### ドキュメント生成・操作

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `document-skills:pdf` | PDF読み取り/結合/分割/回転/透かし/フォーム入力/暗号化/画像抽出/OCR | PDF, .pdf, merge PDF, OCR, スキャンPDF, フォーム入力 |
| `document-skills:docx` | Word文書作成・編集 (TOC, 見出し, ページ番号, レターヘッド, find-replace, 画像挿入, トラックチェンジ) | Word doc, .docx, レポート, メモ, レター, テンプレート |
| `document-skills:xlsx` | Excel/CSV/TSV 読み書き、列追加、数式、書式、グラフ、データクレンジング | .xlsx, .csv, スプレッドシート, Excel, データ整形 |
| `document-skills:pptx` | PowerPoint スライド作成・編集 (デッキ, レイアウト, スピーカーノート, コメント) | .pptx, スライド, デッキ, プレゼン |

#### デザイン・ビジュアル

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `document-skills:canvas-design` | .png/.pdf 静的アート (ポスター、デザイン) — オリジナル作品のみ (著作権配慮) | ポスター, デザイン, 静的アート |
| `document-skills:algorithmic-art` | p5.js でジェネラティブアート (flow field, particle system, seeded random) | generative art, p5.js, flow field, パーティクル |
| `document-skills:frontend-design` | 高品質 frontend UI (AI っぽくない distinctive デザイン) | website, landing page, dashboard, React コンポーネント, beautify UI |
| `document-skills:web-artifacts-builder` | 複雑な claude.ai HTML artifact (React + Tailwind + shadcn/ui, state管理, routing) | multi-component artifact, claude.ai HTML, shadcn artifact |
| `document-skills:theme-factory` | 10種プリセットテーマ (色・フォント) を slides/docs/HTML に適用 | テーマ適用, color theme, ブランドテーマ |
| `document-skills:brand-guidelines` | Anthropic 公式ブランドカラー・タイポグラフィ適用 | Anthropic ブランド, brand color, 公式デザイン |
| `document-skills:slack-gif-creator` | Slack 最適化 アニメーション GIF 作成 | Slack GIF, アニメーション GIF |

#### 開発・運用

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `document-skills:mcp-builder` | MCP サーバー作成ガイド (Python FastMCP / Node TS SDK) | MCPサーバー作る, FastMCP, MCP SDK |
| `document-skills:webapp-testing` | Playwright でローカル webapp 操作・テスト (スクショ, ブラウザログ) | Playwright, webapp test, UI 動作確認 |
| `document-skills:claude-api` | (既存 [[claude-api]] と重複、グローバル版を優先使用) | Anthropic SDK, claude-* model |

#### ライティング・コミュニケーション

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `document-skills:internal-comms` | 社内コミュニケーション (ステータスレポート, リーダーシップ更新, 3P, ニュースレター, FAQ, インシデントレポート) | ステータスレポート, 社内連絡, FAQ, インシデントレポート |
| `document-skills:doc-coauthoring` | ドキュメント共同編集ワークフロー (proposal, spec, decision doc) | proposal書く, spec書く, decision doc, ドキュメント共同編集 |

#### メタ・スキル管理

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `document-skills:skill-creator` | スキルの新規作成、編集、最適化、eval/ベンチマーク | スキル作る, skill optimize, eval, ベンチマーク |

### Vercel プラグイン (vercel:* で始まる、Next.js / Vercel 系で活用)

| スキル名 | 用途 |
|---------|------|
| `vercel:nextjs` | Next.js App Router (RSC, Server Actions, Cache Components) |
| `vercel:shadcn` | shadcn/ui CLI、テーマ、コンポーネント、カスタムレジストリ |
| `vercel:ai-sdk` | AI SDK (chat, tool, structured, agent, MCP, embedding, image gen) |
| `vercel:deploy`, `vercel:deployments-cicd` | preview/production デプロイ、CI/CD |
| `vercel:env`, `vercel:env-vars` | 環境変数、OIDC token |
| `vercel:auth` | Clerk / Descope / Auth0 |
| `vercel:vercel-storage` | Blob, Edge Config, Neon, Upstash |
| `vercel:vercel-functions` | Serverless / Edge / Fluid Compute, Cron Jobs |
| `vercel:routing-middleware` | rewrite/redirect, personalization |
| `vercel:workflow` | 永続ワークフロー (pause/resume, retry) |
| `vercel:chat-sdk` | マルチプラットフォーム chat bot |
| `vercel:vercel-sandbox` | ephemeral microVM で untrusted コード実行 |
| `vercel:turbopack` | Next.js バンドラー設定 |
| `vercel:runtime-cache` | tag-based キャッシュ |
| `vercel:ai-gateway` | プロバイダ統合・failover |
| `vercel:next-cache-components` | PPR, use cache, cacheLife |
| `vercel:next-upgrade` | Next.js バージョンアップ |
| `vercel:next-forge` | Turborepo SaaS スターター |
| `vercel:vercel-cli` | vercel CLI 操作 |
| `vercel:vercel-firewall` | DDoS, WAF, rate limit |
| `vercel:vercel-agent` | AI コードレビュー、incident 調査 |
| `vercel:verification` | ブラウザ→API→DB の通し確認 |
| `vercel:marketplace` | Marketplace 統合 |
| `vercel:status` | プロジェクト状態確認 |
| `vercel:bootstrap` | プロジェクト初期セットアップ |
| `vercel:react-best-practices` | TSX レビュー |
| `vercel:knowledge-update` | LLM の Vercel 知識補正 |

---

## 🔌 MCPコネクタ (アプリ常時接続、claude.ai 経由)

| コネクタ | 用途 | トリガー語彙 |
|---------|------|------------|
| **Notion** | Arte Grow / Task Hub のページ管理、データベース更新、コメント | Notion, Arte Grow root, Task Hub ログ, タスクDB |
| **Google Calendar** | 予定確認・作成・更新・suggest_time | カレンダー, 予定, ミーティング, 空き時間 |
| **Gmail** | メール検索、ドラフト、ラベル管理、スレッド取得 | メール, Gmail, ドラフト, ラベル |
| **Google Drive** | ファイル検索・読み取り・作成・コピー・最近のファイル | Drive, ドキュメント検索, ファイル共有 |
| **Microsoft 365** | Office連携 (auth経由) | M365, Word, Excel, Outlook |
| **Canva** | デザイン生成、テンプレート、コメント、エクスポート、ブランドキット | Canva, デザイン作って, バナー, 販促物 |
| **Figma** | デザインから実装、Code Connect、FigJam、変数定義、スクショ | Figma, デザインから実装, design context, figma.com URL |
| **Miro** | ボード、コードウィジェット、図、ドキュメント、テーブル | Miro, ボード, ホワイトボード, 図解 |
| **Goodnotes** | Markdown ドキュメント、SVG、Mermaid 図 | Goodnotes, 手書きノート, マインドマップ |
| **Zoom for Claude** | 会議録画検索、要約、ファイル取得 | Zoom 録画, 会議, ミーティング録音 |
| **Vercel (plugin)** | 認証・デプロイ・統合管理 | Vercel auth |

---

## 🎭 ローカル MCP サーバー (自前 install、claude mcp 経由)

> claude.ai 側で常時接続される MCP コネクタとは別系統。ローカルマシンで動く MCP サーバーを Claude Code から `mcp__<server>__<tool>` として呼び出せる。`~/.claude/settings.local.json` の `permissions.allow` に `mcp__<server>` を追加することで毎回の許可プロンプトをスキップできる。
>
> 2026-05-23: Shin Coding Tutorial「今すぐにこれら9つのMCPを導入してください」を参考に 4 個追加 (Serena / Context7 / chrome-devtools / GitHub gh-mcp)。Notion / Figma は claude.ai 側のクラウド版で代替済 (二重稼働回避)、Supabase / Stripe は認証情報整い次第。

| サーバー | 用途 | トリガー語彙 | 詳細 |
|---------|------|------------|------|
| **Playwright** (`@playwright/mcp`) | ブラウザ自動操作 (Chromium / WebKit / Firefox)、DOM操作、スクリーンショット、フォーム入力、URL ナビゲーション、network intercept、JS 評価 | Playwright, ブラウザ自動化, E2Eテスト, スクレイピング, ブラウザでこのページを開いて, スクショ撮って, クリックして, フォーム入力 | [[playwright_mcp]] / [[mcp_local_servers]] |
| **Serena** (`oraios/serena`) | LSP連携の semantic コードベース探索・編集。`uvx --from git+...` で起動、`--project-from-cwd` で起動 cwd のプロジェクトを自動認識。Python/TS/Java/Vue/HTML 等多数のLSPに対応 | Serena, セマンティック探索, コードベース探索, シンボル検索, refactor, 巨大コードベース | [[mcp_local_servers]] |
| **Context7** (`@upstash/context7-mcp`) | 最新ドキュメント検索 (Next.js / Supabase / shadcn/ui / better-auth / Tailwind 等)。AI のフルめ情報問題を解決。GitHub URL から任意のリポを Add 登録可。API key なしでも使える (品質は key ありで向上) | Context7, 最新ドキュメント, use context7, ライブラリ最新仕様 | [[mcp_local_servers]] |
| **chrome-devtools** (`chrome-devtools-mcp`) | Chrome DevTools 経由でブラウザ操作 + コンソール/ネットワーク/パフォーマンス計測 (LCP/TTFB)。**Node.js v22+ 必須** (動画スピーカーが v20 で詰まった) | Chrome DevTools, LCP, TTFB, performance audit, console error, network inspect | [[mcp_local_servers]] |
| **GitHub** (`shuymn/gh-mcp` 拡張 → bundled `github-mcp-server`) | gh CLI 拡張経由で GitHub API 操作 (リポ作成 / PR / Issue / Actions)。`gh auth status` の認証情報をそのまま流用、PAT 別途管理不要 | GitHub MCP, gh コマンド, リポ作成, PR操作, Issue管理 | [[mcp_local_servers]] |
| **Supabase** (`@supabase/mcp-server-supabase`) | Supabase DB に対する read-only クエリ・スキーマ確認・pgvector 状態確認。`--read-only` + `--project-ref=lkrmziwygyyyijyabtzp` で **lecture-hub 専用**に固定。書き込みは再登録で解除 | Supabase, lecture-hub DB, SELECT, スキーマ確認, pgvector | [[mcp_local_servers]] |
| **Stripe** (`@stripe/mcp`) | Stripe API 操作 (商品 / 決済 / Customer / Subscription)。**Test mode** + `--tools=all` で全機能。本番モードは live key 切替が必要 | Stripe, 課金, 決済, サブスク, test mode, Customer 作成 | [[mcp_local_servers]] |

### Playwright MCP を使う典型シーン

- Salamat WBS / Lecture Hub / Task Hub の **本番 URL の動作確認** (HTTP コードだけでなく、レンダリング後の DOM を見る)
- DaVinci / Vercel ダッシュボードのような **GUI 操作の自動化** (computer-use との使い分け: Web系は Playwright が早い・正確)
- AI学習スプリント中の **ライブコーディング動画の取材** (Anthropic Academy のコースを自動巡回してスクショ + テキスト取得)
- ai-researcher の **scrape 対象拡張** (papers_with_code 不安定問題の代替経路)
- 平成たち祭の **応募フォームの動作確認** (Google Forms + GAS)

### 新規 6 MCP の典型シーン

- **Serena**: lecture-hub / salamat-website-v2 のような中規模 Next.js プロジェクトで「この関数どこから呼ばれてる?」「この型を使ってる箇所全部」のような semantic 探索。Grep より精密
- **Context7**: Next.js 16 / Tailwind v4 / shadcn/ui のように頻繁にアップデートされるライブラリの最新仕様確認。`CLAUDE.md` のベストプラクティスルール生成にも有用
- **chrome-devtools**: Salamat WBS / Lecture Hub のパフォーマンスチューニング (LCP 2.5秒以下クリアの維持)。Playwright が「動作確認」、chrome-devtools が「性能計測」
- **GitHub**: ai-researcher / vidkit / morning-briefing の Private repo の Issue 起票・PR 作成を自然言語で。gh CLI を Bash で叩くのと同等の機能だが、自然言語 ⇄ ツールパラメータの変換が gh-mcp 側で行われる
- **Supabase**: lecture-hub の `documents` / `tags` テーブルのスキーマ確認、サンプル SELECT、pgvector の状態確認 (read-only 固定)
- **Stripe**: 将来の課金実装 (Salamat Task Hub 商用化、Arte Grow 決済テスト) の動作確認、Customer / Subscription の test mode 操作

### 棲み分け

- **Playwright MCP** ↔ **chrome-devtools MCP**: Playwright = 操作・動作テスト・スクショ、chrome-devtools = 性能・コンソール・ネットワーク計測。動画スピーカーは「パフォーマンス系は chrome-devtools 使った方がいい」
- **Playwright MCP** ↔ **computer-use MCP**: Web 操作なら Playwright が DOM-aware で早い。computer-use はネイティブ desktop アプリ専用 (Finder / Photos / DaVinci 等)
- **Playwright MCP** ↔ **`vercel:verification` スキル**: スキルは「ブラウザ→API→DB の通し確認」フロー、Playwright MCP は素の操作プリミティブ。スキル内部から Playwright を呼ぶ形が筋
- **Serena** ↔ **Grep**: Grep は文字列マッチ、Serena は AST/LSP ベースで定義・参照を正確に追える。大きいリポでは Serena、小さい修正は Grep
- **Context7** ↔ **WebSearch / WebFetch**: Context7 は「ライブラリの最新ドキュメント特化」、Web 検索は汎用。ライブラリ仕様の確認は Context7 を先に
- **GitHub MCP (gh-mcp)** ↔ **Bash の `gh` 直叩き**: 同じ機能。自然言語で曖昧に指示したい時は MCP、明確なコマンドは Bash 直叩きが速い
- **Supabase MCP** ↔ **`vercel:vercel-storage` スキル**: MCP は実 DB クエリ、スキルは設計知識。スキーマ設計はスキル、実 DB 確認は MCP
- **Stripe MCP** ↔ **Stripe Dashboard 直操作**: MCP は test mode 限定のプログラマブル操作、本番設定変更はダッシュボードで YD 自身が行う

---

## 🗺 プロジェクト別の典型機能候補

### Salamat WBS (Next.js 16 + React 19 + Vercel)
- **コード**: vercel:nextjs, vercel:shadcn, vercel:deploy, vercel:vercel-storage, vercel:react-best-practices
- **デザイン**: Figma (デザイン参照), Canva (バナー), ui-ux-pro-max
- **下層ページ実装時**: vercel:next-cache-components, vercel:routing-middleware

### Lecture Hub (Next.js 15 + TipTap + Supabase)
- **コード**: vercel:nextjs, vercel:ai-sdk (要約/タスク抽出/embedding)
- **設計**: claude-api (LLM プロンプト設計)
- **将来**: Notion (連携用)

### Task Hub (Next.js + Firebase Hosting、Vercel ではない点に注意)
- **UI**: ui-ux-pro-max
- **コード**: vercel:env-vars (参考)、Firebase 本体は MCP なし

### vidkit (Python CLI)
- **実機テスト**: video-tutorial, fcp-autocut, fcp-tighten
- **lecture モード仕上げ後**: HF_TOKEN セット → FCP 実機検証

### morning-briefing (Python + claude -p + macOS say)
- **配信先**: Google Drive
- **スケジューリング**: schedule (cron 代替検討)

### ai-researcher (Python + claude -p)
- **launchd 確認**: schedule (補助参照)

### ai-simulator (Python REPL、対話型)
- ⚠️ Claude Code Bash 経由は不可、YD のターミナルで実行

### Arte Grow (社会起業)
- **ナレッジ**: Notion (Arte Grow root: 539af299-ce3c-8272-82b5-0176f676b926)
- **販促・デザイン**: Canva, Figma (ブランドガイド)

### AI学習スプリント (2026-05-19 開始)
- **Anthropic Academy 実装演習**: claude-api
- **Cowork コース**: vercel:ai-sdk, 動画化なら video-tutorial
- **教科書 PDF 配信**: Google Drive
- **学習スケジュール**: Google Calendar

### 平成たち祭 動画制作
- **素材整理**: fcp-autocut, fcp-tighten
- **構成案メモ**: Goodnotes

### 教科書システム (textbook-engine)
- **配信**: Google Drive (morning-briefing 基盤に相乗り想定)

### Salamat 運営 (260名、代表業務)
- **チーム連絡**: Gmail, Google Calendar
- **議事録・資料**: Notion or Goodnotes
- **公式文書 (報告書/レター)**: document-skills:docx, document-skills:pptx
- **団体内ステータスレポート/FAQ**: document-skills:internal-comms
- **販促バナー/ポスター**: document-skills:canvas-design (オリジナル制作)、Canva MCP (テンプレ系)

---

## 🎯 朝の機能マッピング演習 (毎セッション、Claude が自動実行)

新セッション開始時、Claude は以下を実行:

1. `current_state/active_projects.md` から「🟢 アクティブ・優先度高」を抽出
2. 当日 (`# currentDate` のメモリ) の予定アクションを確認
3. 上記カタログから、**当日タスクに使えそうな機能を 3〜5 個ピック**
4. YD への初手応答に「今日使えそうな候補」として簡潔に提示

### 提示フォーマット例

```
今日は AI学習スプリント Day 2 (Claude 101 + Cowork) + vidkit lecture モード仕上げ。
使えそうな機能候補:
- claude-api スキル — Anthropic Academy の SDK 実装演習に
- vercel:ai-sdk スキル — Cowork コースの実装パート
- Google Drive MCP — 教科書 PDF 配信先
- vidkit + fcp-autocut — lecture モード仕上げ後の実機テスト
```

---

## ⚠️ 「忘れる」問題への対策

- スキル/MCP は毎セッション system-reminder で**自動注入される** (= 技術的には「読み込まれている」)
- 問題は「**意識する → 提案する**」の習慣化が抜けること
- このファイルが、その習慣化の触媒
- 月1回、新しいスキル/MCP が増えたら本ファイルを更新

### 更新履歴
- **2026-05-23** — ローカル MCP サーバーを 4 個追加 (Serena / Context7 / chrome-devtools / GitHub gh-mcp)。Shin Coding Tutorial「今すぐにこれら9つのMCPを導入してください」(2025-09-29) を参考に、user scope (`claude mcp add --scope user`) で導入。Notion / Figma は claude.ai 側のクラウド版で代替、Supabase / Stripe は認証情報整い次第。詳細 [[2026-05-23_MCP_9個導入]]
- **2026-05-21 (夜)** — Claude Code エージェントチーム機能 (実験的、v2.1.32+) を有効化、「Claude Code ネイティブ機能」セクションを新規追加。詳細運用 [[claude_code_agent_teams]] / 意思決定 [[2026-05-21_エージェントチーム機能_有効化]]
- **2026-05-21** — Anthropic 公式 Agent Skills 群 (`example-skills:*` / `document-skills:*`) を追加。マーケットプレイス `anthropics/skills` から `example-skills` と `document-skills` プラグインを `/plugin install` で導入
- **2026-05-21** — 独立 skill 3件追加: `frontend-design` (Anthropic製、AI slop 回避哲学) / `web-design-guidelines` (Vercel製、UI レビュー) / `backend-patterns` (ECC = Everything Claude Code 由来、backend パターン集)。UI/UX 設計セクションを 3 skill 併用運用に拡張、Backend 開発セクションを新規追加。詳細運用ルールは [[knowledge/programming/skills/index]] に集約

---

## 📚 関連

- [[tools_available]] — 環境別ツール一覧 (旧ファイル、補完情報)
- [[active_projects]] — 当日タスクのソース
- [[2026-05-20_機能マッピング自動化]] — 本ファイル誕生の意思決定
- `~/.claude/CLAUDE.md` — グローバル起動シーケンス (本ファイルへのポインタあり)
- [[00_CLAUDE_BOOT]] — Vault起動シーケンス (Step 4.5 で本ファイルを読む)
- [[morning_briefing]] — 朝ブリーフィング (07 セクションで今日の機能候補を表示)
