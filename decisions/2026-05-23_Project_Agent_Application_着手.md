---
type: decision
date: 2026-05-23
tags: [project, agent, harness-design, project_agent_application, claude-code]
---

# 2026-05-23 Project Agent Application 着手 — ハーネス設計 5 エージェント体制

## 状況

YD が「全く新しいアプリケーションを作りたい」と発話。アプリ名は **Project Agent Application** (仮称、最終ブランド名は別途決定)。Microsoft Teams ライクなプロジェクト/タスク/チーム一元管理アプリだが、大学生・若年層向けに専門特化。デザイン性 / UI を最重視する方針。

直前に Shin Coding Tutorial の「Claude Code ハーネスエンジニアリング」動画 (25:15、2026-04-04 公開) を vidkit の tutorial モードで処理 + 内容理解済 (`~/Downloads/vidkit_2026-05-23_tutorial_Wfz-gdWcItM/`)。動画は **Planner / Generator / Evaluator** の 3 エージェント自走パターン。本意思決定はそれを実戦投入する流れ。

## 採用した方針

### 1. ディレクトリ構造

`~/projects/project-agent-application/` を新規作成:

- `VISION.md` — YD 発話原文 + 整理 (Planner の入力)
- `CLAUDE.md` — 起動時必読リスト + 5 エージェント運用ルール (フロー図 / 入出力表 / エスカレーション条件)
- `.claude/agents/planner.md` — Task Hub の planner をベースに汎用化
- `.claude/sprints/` — Planner の出力先 (現状空)
- `docs/` — 将来用

### 2. エージェント体制: 5 エージェント (動画 3 + designer + design-evaluator)

| Agent | 役割 | 動画準拠? |
|---|---|---|
| **planner** (今回先に立ち上げ) | VISION → SPEC + DESIGN_DIRECTION + sprint-XX | ✅ 動画 Planner |
| **designer** (後で追加) | UI/UX 専門、モック + tokens | ❌ 拡張 (若年層向け UI 最重要のため分離) |
| **builder** (後で追加) | Generator、コード実装 | ✅ 動画 Generator |
| **qa-evaluator** (後で追加) | 機能テスト + Lighthouse | ✅ 動画 Evaluator |
| **design-evaluator** (後で追加) | デザイン要件チェック (NG リスト遵守) | ❌ 拡張 (Task Hub 知見流用) |

動画は 3 エージェントだが、Task Hub で既に「design-evaluator 分離」の知見あり ([[2026-05-21_TaskHub_UIUX根本見直し_4エージェントハーネス構築]])。本プロジェクトはさらに **designer** を分離して 5 エージェント = **動画より 2 歩進んだ構成**。

### 3. オーケストレーション (CLAUDE.md に記述)

- 上位エージェントは作らず、CLAUDE.md でフロー管理 (動画準拠)
- **直列**: planner → designer → builder (依存あり)
- **並列**: qa-evaluator ‖ design-evaluator (builder 完了後、互いに独立)
- **自己改善ループ**: 評価 Fail → builder へ差し戻し → 同 sprint 3 回連続 Fail で YD にエスカレーション

### 4. 進行方針: planner だけ先に立ち上げる

5 エージェント全部一気に作るのは早すぎる。仕様未確定で builder/evaluator を作っても評価対象が定義されていない。
**まず planner で壁打ち → SPEC.md / DESIGN_DIRECTION.md / sprint-XX.md を確定 → 残り 4 エージェントを後追いで構築**。動画の Shin 氏も「いきなり全部作れ」とは言っていない (記事から markdown コピペして 1 個ずつ立ち上げる流れ)。

## 選択肢

| 案 | 内容 | 採用判定 |
|---|---|---|
| A | Task Hub に新機能として組み込む | 棄却。「全く新しいアプリ」要望と合わない |
| B | 5 エージェント一気に構築 | 棄却。仕様未確定で builder/evaluator 作っても無駄 |
| **C (採用)** | planner だけ先に立ち上げて壁打ち → 段階的に拡張 | YD が「壁打ちで仕様を煮詰めたい」と明言 |
| D | 動画準拠 3 エージェント (designer なし) | 棄却。Task Hub の design-evaluator 分離知見あり、若年層向けで UI 最重要 |

## ✅ うまく行ったこと

- 動画の内容を vidkit で前処理 → 完全理解 → 同日に実戦投入の流れがスムーズ (vidkit tutorial モードが想定通りに機能した)
- Task Hub の `.claude/agents/taskhub-planner.md` (200 行) をテンプレ流用 → 実装作業を約 20 分に短縮
- VISION.md に YD 発話原文を残したことで、後続エージェントが原意を読み取れる構造に
- 「未確定 / Planner で詰める論点」を VISION.md に列挙したことで、壁打ちの目的が明確化
- 5 エージェントの責任マトリクスを CLAUDE.md にテーブル化 → エージェント定義作成時の重複/抜けを防止可能

## ❌ 詰まったこと

- 該当なし (Phase 0 のみ完了、planner.md の Write まで一発で完走)
- ただし planner エージェントを実際に呼び出して壁打ち品質を検証する前に停止したため、現実に Q1〜Q5 が良い形で出るかは未検証

## 📋 次回同じことをするときのチェックリスト

新規アプリにハーネス設計を導入する場合の手順:

1. **VISION.md を最初に書く** — YD 発話原文 + 整理 + 「未確定論点」リスト
2. **CLAUDE.md にオーケストレーションルール** — 5 エージェントの責任マトリクス + フロー図 + 並列実行ルール + エスカレーション条件 + やってはいけないこと
3. **planner だけ先に作る** — Task Hub から流用、`name:` を taskhub-planner から汎用名に書き換え、必読パスを新プロジェクト用に書き換え、参考プロジェクトとして Task Hub を残す
4. **壁打ち優先** — planner 起動 → Q1〜Q5 → YD 回答 → SPEC.md 生成 (ここまで 1 セッション)
5. **designer / builder / 2 evaluator は仕様確定後** — 順序間違えると build 対象が定義されてない

参考: [[task_hub]] のハーネス構築 ([[2026-05-21_TaskHub_UIUX根本見直し_4エージェントハーネス構築]]) の流用ノウハウ

## 関連

- [[project_agent_application]] (knowledge/programming/projects/) — 運用マニュアル
- [[task_hub]] — 姉妹プロジェクト、4 エージェントテンプレ元
- [[2026-05-21_TaskHub_UIUX根本見直し_4エージェントハーネス構築]] — テンプレ作成時の意思決定
- 動画: Shin Coding Tutorial「Claude Code ハーネスエンジニアリング」(2026-04-04、25:15、Wfz-gdWcItM)
- 動画解説記事: claude-code-academy.dev/articles/claude-code-harness-design
- 動画サンプルリポ: github.com/Shin-sibainu/harness-sample-app
- Anthropic 公式: ハーネス設計 (Planner/Generator/Evaluator 3 エージェント)

## 次のアクション

- [ ] planner エージェントを起動 → Q1〜Q5 壁打ち開始
- [ ] YD 回答 → planner が SPEC.md / DESIGN_DIRECTION.md / sprint-XX.md を生成
- [ ] designer / builder / qa-evaluator / design-evaluator を `.claude/agents/` に追加 (テンプレ: Task Hub の対応エージェント)
- [ ] Sprint 01 から自走実装 (`--dangerously-skip-permissions` で 1〜2 時間放置)
