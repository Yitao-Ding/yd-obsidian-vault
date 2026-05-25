---
type: knowledge
domain: programming/projects
last_updated: 2026-05-23
status: active
phase: Phase 0 (planner セットアップ完了、壁打ち未開始)
tags: [project, harness-design, claude-code-agents, task-management, gen-z]
---

# Project Agent Application

> 大学生・若年層向けに攻めた、Microsoft Teams ライクなプロジェクト/タスク/チーム一元管理アプリ。
> Anthropic ハーネス設計に基づく **5 エージェント自走体制** で構築中。

## 概要

- **着手**: 2026-05-23 ([[2026-05-23_Project_Agent_Application_着手]])
- **パス**: `~/projects/project-agent-application/`
- **目的**: タスク管理を **個人 → プロジェクト → チーム** の三層で一元管理。Teams より軽くて若い、Notion より統合された、Discord より仕事寄りの存在を目指す
- **想定ユーザー**: 大学生 / 若年層 (Gen Z 中心)、サークル・ゼミ・研究チーム・学生団体・インターン・起業準備チーム
- **技術スタック**: 未定 (planner と壁打ちで確定)
- **デプロイ先**: 未定

## エージェント体制 (5 つ)

| Agent | 役割 | 入力 | 出力 |
|---|---|---|---|
| planner | 仕様策定 (壁打ち + SPEC 生成) | VISION.md + YD 発話 | SPEC.md / DESIGN_DIRECTION.md / sprint-XX.md |
| designer | UI/UX 設計 | sprint-XX.md + DESIGN_DIRECTION.md | `design/<sprint>/mockup.html` + tokens.md |
| builder | コード実装 (Generator) | sprint-XX + designer 出力 | `code/` + IMPLEMENTATION_NOTES.md |
| qa-evaluator | 機能テスト (Evaluator A) | sprint-XX + dev サーバー | EVAL_QA.md |
| design-evaluator | デザイン検証 (Evaluator B) | sprint-XX + DESIGN_DIRECTION.md + dev サーバー | EVAL_DESIGN.md |

## 連携フロー

```
YD (1〜4 行のビジョン)
   ↓
planner (壁打ち) → designer → builder → [qa-evaluator ‖ design-evaluator]
                                              ↓ Fail
                                              builder へ差し戻し (最大 3 ループ)
```

### 並列実行ルール

- 直列必須: planner → designer → builder (依存あり)
- 並列 OK: qa-evaluator と design-evaluator (互いに独立、1 メッセージ内で 2 つの Agent 同時呼び出し)

### 自己改善ループ

- 同 sprint で 3 回連続 Fail → ループ停止、YD にエスカレーション
- ループ回数は `sprint-XX.md` の `loops:` フィールドに記録

## 起動方法

```bash
cd ~/projects/project-agent-application
claude
> planner エージェントで壁打ちして
```

自律実行 (Phase 2 以降):

```bash
cd ~/projects/project-agent-application
claude --dangerously-skip-permissions
> CLAUDE.md を読んで、Phase 3 から開始して Sprint 01 を完成させて
```

## 進捗

- [x] Phase 0: ディレクトリ + planner + VISION.md + CLAUDE.md (2026-05-23)
- [ ] Phase 1: planner で壁打ち → SPEC.md / DESIGN_DIRECTION.md / sprint-XX.md 確定
- [ ] Phase 2: designer / builder / qa-evaluator / design-evaluator を `.claude/agents/` に追加
- [ ] Phase 3: builder で Sprint 01 から自走実装 (1〜2 時間放置想定)

## ファイル構成

```
~/projects/project-agent-application/
├ VISION.md                     YD 発話 + 整理
├ CLAUDE.md                     5 エージェント運用ルール
├ .claude/
│  ├ agents/planner.md          壁打ち + 仕様書生成
│  └ sprints/                   planner の出力先 (空)
├ docs/                         将来用
└ code/                         builder の実装先 (Phase 3 以降)
```

## エスカレーション条件 (YD に聞くべきタイミング)

| Agent | エスカレートする状況 |
|---|---|
| planner | ビジョンが曖昧で複数解釈できる時 (Q1〜Q5 形式で質問) |
| designer | DESIGN_DIRECTION.md だけでは判断できない美的選択肢 |
| builder | 技術スタックを変更する必要が出た時 |
| qa-evaluator | スプリント契約自体が実装不可能だと判明した時 |
| design-evaluator | NG リスト基準そのものに矛盾がある時 |
| (loop) | 同 sprint で 3 回連続 Fail |

## やってはいけないこと

- planner が builder 領域 (具体的ライブラリ、Tailwind クラス) に踏み込む
- designer が SPEC.md の機能スコープを変更する
- builder が designer の tokens.md を勝手に変える
- evaluator が「美しい/使いにくい」と主観で書く (スプリント契約の客観基準で判定)
- 自己改善ループを 3 回超えて回す
- YD に聞かずに新機能を追加する
- 全エージェント並列起動 (依存関係を破壊する)

## ✅ うまく行ったこと

- Task Hub の `.claude/agents/taskhub-planner.md` (200 行) を流用 → セットアップ時間を約 20 分に圧縮
- VISION.md に「未確定論点」リストを明示 → 壁打ちの目的が一発で明確化
- 5 エージェントの責任マトリクスを CLAUDE.md にテーブル化 → 入出力ファイル名が一意に決まり、引き継ぎミスを防止
- 動画 (Shin Coding Tutorial) を vidkit で前処理 → 同日に実戦投入できた (動画→着手まで 24 時間以内)

## ❌ 詰まったこと

- 該当なし (Phase 0 のみ完了)
- 想定リスク (Phase 1 以降で発生し得るもの):
  - planner の Q1〜Q5 が抽象的すぎて YD が答えづらい → 推奨案を必ず付ける運用で予防
  - designer と builder の境界が曖昧で重複作業発生 → tokens.md を designer の専管出力にして分離
  - evaluator が NG リスト遵守の判定で揺れる → 機械テスト可能な基準のみに限定 (色 hex / フォント名 / 角丸 px)

## 📋 次回同じことをするときのチェックリスト

新規プロジェクトでハーネス設計 5 エージェントを立ち上げる時:

1. `mkdir -p <project>/.claude/agents <project>/.claude/sprints <project>/docs`
2. **VISION.md** を最初 — YD 発話原文 + 整理 + 「未確定論点」リスト
3. **CLAUDE.md** — 5 エージェント運用ルール (フロー図 + 責任マトリクス + 並列ルール + エスカレーション条件 + やってはいけないこと)
4. **planner.md** だけ先に作る — Task Hub の `taskhub-planner.md` を流用、`name:` 書き換え、必読パスを新プロジェクト用に書き換え
5. **壁打ち優先** — planner 起動 → Q1〜Q5 → YD 回答 → SPEC.md / DESIGN_DIRECTION.md / sprint-XX.md 生成
6. **designer / builder / 2 evaluator は仕様確定後** に追加 (Task Hub の対応エージェントから流用)
7. **Sprint 01 から自走実装** — `--dangerously-skip-permissions` で 1〜2 時間放置

## 関連

- [[task_hub]] — 姉妹プロジェクト、4 エージェントテンプレ元
- [[2026-05-23_Project_Agent_Application_着手]] — 着手時の意思決定
- [[2026-05-21_TaskHub_UIUX根本見直し_4エージェントハーネス構築]] — テンプレ作成元
- 動画: Shin Coding Tutorial「Claude Code ハーネスエンジニアリング」(2026-04-04、25:15)
- 解説記事: claude-code-academy.dev/articles/claude-code-harness-design
- サンプルリポ: github.com/Shin-sibainu/harness-sample-app
- Anthropic 公式: ハーネス設計 (Planner/Generator/Evaluator)
