---
type: archive
archived_date: 2026-05-23
project: task_hub
status: discontinued
replaced_by: project_agent_application
---

# Task Hub (タスク管理アプリ) — 廃止スナップショット

> **2026-05-23 廃止決定。** 代替: [[../decisions/2026-05-23_TaskHub廃止_ProjectAgentApp移行]]
> 本ファイルは廃止時点の状態を記録する (active_projects.md からの転載)。

## 廃止時点の状態 (2026-05-23、active_projects.md #6 より)

- **状況**: 🟢 **UI/UX 根本見直しに着手 (2026-05-21 21:01)** — フロントエンドレベルから仕様を見直す方針 (YD 指示)。現状 UI 把握完了 (8 画面 + globals.css + spec + HANDOVER)、5 大課題特定 (ブランド分裂 #ff6a00 vs #2563EB / デザインシステム不在 / IA 浅い / SaaS テンプレ的 / ロゴ単文字)。**Anthropic Labs ハーネス設計を踏襲した 4 自立型エージェント** を `~/projects/salamat-task-hub/.claude/agents/` に構築完了 (planner / builder / qa-evaluator / design-evaluator★)。次は YD のビジョン (1〜4 行) → planner 起動。← **未着手のまま廃止**
- **過去履歴**: 2026-05-19 21:17 git 整理 + GitHub 連携完了 (untracked 32 + dirty 7 → 5 commit)。**デプロイ実態は Firebase Hosting** (`salamat-task-hub.web.app`) — Vercel ではない
- **パス**: `/Users/ittou/projects/salamat-task-hub`
- **GitHub**: https://github.com/Yitao-Ding/salamat-task-hub (Private、main → origin/main 追跡済、当面放置)
- **本番 URL**: https://salamat-task-hub.web.app (Firebase Hosting、Spark プラン、当面放置)
- **スタック**: Next.js 16.2.1 (App Router、静的 export) + React 19.2.4 + TypeScript + Tailwind CSS v4 + Firebase (Auth/Firestore/Storage) + next-pwa
- **コミット履歴 (2026-05-19 push)**:
  - `f876634` chore: prepare Next.js + Firebase build configuration
  - `6d0e341` feat: add Firebase backend configuration
  - `7fa1b65` feat: implement core app (auth, Firestore CRUD, routes, UI)
  - `47b6be5` feat: PWA setup (manifest, service worker, icons)
  - `9dfcd77` docs: add handover document, spec, and admin setup script
- **4 エージェント構成**: taskhub-planner / taskhub-builder / taskhub-qa-evaluator / taskhub-design-evaluator (Anthropic Labs ハーネス設計準拠、Design Evaluator は frontend-design / web-design-guidelines / ui-ux-pro-max / vercel:react-best-practices の 4 スキル参照必須 + AIスロップ即不合格ルール内蔵)
- **関連**: [[../knowledge/programming/tools/task_hub]] (運用マニュアル、2026-05-20 作成)、[[../decisions/2026-05-19_TaskHub_git整理_GitHub連携]] (git 整理の意思決定)、[[../decisions/2026-05-21_TaskHub_UIUX根本見直し_4エージェントハーネス構築]] (本フェーズの意思決定、本決定で覆る)

## 廃止の経緯

詳細: [[../decisions/2026-05-23_TaskHub廃止_ProjectAgentApp移行]]

- YD「タスクハブは完全にもう使わなくなるから、もう捨てていいよ。あれもうマジで使わないです」(2026-05-23 13:30 頃)
- 本アプリ Project Agent Application のスコープ (複数団体所属モデル + 4 階層 + Z 世代向け Insta/BeReal デザイン + Finch 型キャラ) が Task Hub の上位互換
- 二重開発は不要、Task Hub の思想・知見だけ引き継ぐ

## 引き継いだ知見 (本アプリ Project Agent Application へ)

- 4 自立型エージェントのハーネス設計 (planner / builder / qa-evaluator / design-evaluator)
- Discord ロールモデルの IA
- 寒色系ベース (ただし本アプリでは Q11 で再検討中)
- PWA 重視

## 復活させる場合の手順 (万一)

- `git clone https://github.com/Yitao-Ding/salamat-task-hub` で復元可能
- Firebase Hosting (`salamat-task-hub.web.app`) はまだ生きてる (Spark プラン無料)
- 5/19 push 時点 (5 commit) から再開可能
