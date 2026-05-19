---
type: current_state
last_updated: 2026-05-20
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

### UI/UX 設計

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `ui-ux-pro-max` | 67スタイル/96パレット/57フォントペア/13スタック (React/Next/Vue/Svelte/SwiftUI/RN/Flutter/Tailwind/shadcn) | UI/UX, ダッシュボード, ランディング, glassmorphism, brutalism, bento, shadcn, color palette |

### Claude / API

| スキル名 | 用途 | トリガー語彙 |
|---------|------|------------|
| `claude-api` | Anthropic SDK 利用 (プロンプトキャッシュ込み)、モデル間マイグレーション (4.5→4.7) | anthropic SDK, claude-* model, prompt caching, batch API, Anthropic Academy 実装 |

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

---

## 📚 関連

- [[tools_available]] — 環境別ツール一覧 (旧ファイル、補完情報)
- [[active_projects]] — 当日タスクのソース
- [[2026-05-20_機能マッピング自動化]] — 本ファイル誕生の意思決定
- `~/.claude/CLAUDE.md` — グローバル起動シーケンス (本ファイルへのポインタあり)
- [[00_CLAUDE_BOOT]] — Vault起動シーケンス (Step 4.5 で本ファイルを読む)
- [[morning_briefing]] — 朝ブリーフィング (07 セクションで今日の機能候補を表示)
