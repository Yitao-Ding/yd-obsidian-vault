---
type: decision
date: 2026-05-20
status: completed
tags: [vault, maintenance, task_hub, structure]
related: [vault_improvement_proposals, task_hub, CLAUDE]
---

# 2026-05-20 Vault メンテバッチ 1 + Task Hub 運用マニュアル整備

## 背景

`vault_improvement_proposals.md` に 2026-05-19 の ε モード監視ループが検出した 5 件の pending 提案が蓄積していた。加えて `current_focus.md` が AI学習スプリント開始以前の古い状態のままだった。また Task Hub の運用マニュアル (Phase 2) も未整備だった。

## 実施内容 (Phase 1: Vault メンテ)

### 1. current_focus.md 最新化
- 旧最優先: 「Obsidian Vault 構築」→ 新最優先: 「AI学習スプリント (2026-05-19 開始)」
- vidkit / Salamat WBS / Lecture Hub / Task Hub など完成済みプロジェクトのステータスを反映
- DaVinci 蛹件は前セッション既アーカイブ済みのため追記なし

### 2. knowledge/programming/tools/ → projects/ 構造分割
- `projects/` を新設、YD 自作プロジェクト 7 件を `git mv` で移動:
  - ai_researcher / ai_simulator / lecture_hub / morning_briefing / task_hub / textbook_engine / vidkit
- 残留 (tools/): obsidian_vault / vault_workflow / claude_code / claude_code_permissions / vercel
- WikiLink は basename 解決で自動追従 (変更不要)
- Markdown リンク形式の参照: grep 確認で 0 件
- backtick テキスト参照: active_projects.md + vault_workflow.md の 4 箇所を更新

### 3. CLAUDE.md に必須3セクション例外追記
- `knowledge/*/index.md` はハブ・目次ファイルのため適用外と明示

### 4. decisions/2026-05-19_教科書システム第2号企画.md に必須3セクション追加
- 企画段階のため「該当なし (実装着手前)」+ 想定リスク + 実装チェックリスト

### 5. vault_improvement_proposals.md の全 pending を更新
- index.md 例外: ✅ resolved
- tools/→projects/ 構造分割: ✅ resolved
- 教科書第2号 必須3セクション: ✅ resolved
- ai-researcher 監視対象: ✅ resolved (active_projects に既存)
- ε C grep ロジック改善: ⚠️ wontfix (cron prompt が独立ファイルとして存在しない)
- ε C raw/ 除外追加: ⚠️ wontfix (同上)

**ε C cron prompt 調査結果**: Vault 内全 .md を「cron prompt」「ε モード」「自己進化」で grep + CronList 確認 + .claude/ 配下検索 → いずれも該当なし。ε C 監視は `schedule` スキル経由の一過性セッションとして実行されており、prompt は独立ファイルに永続化されていない。次回 ε C を起動する際は新規 prompt 作成が必要。

## 実施内容 (Phase 2: Task Hub 運用マニュアル整備)

### task_hub.md 確認・更新
- `knowledge/programming/projects/task_hub.md` は前セッション (2026-05-20 02:49 の Task Hub git 整理 Vault 収容) で既作成済みと確認
- frontmatter の `subarea: tools` を `subarea: projects` に修正
- セットアップ手順セクションを新設 (git clone → npm install → .env.local → firebase rules deploy → admin init)
- 必須3セクション (✅/❌/📋) は前セッション作成済みで変更なし

### active_projects.md Task Hub 節の確認
- 「task_hub.md 新規作成」は次のアクションに存在しない (既完了扱い = 関連フィールドに `[[task_hub]] (運用マニュアル、2026-05-20 作成)` 記載済み)

## commit 履歴

| commit | 内容 |
|--------|------|
| `342e48e` | current_focus.md 最新化 |
| `8764c04` | tools/ → projects/ 構造分割 (git mv 7件) |
| `71d5f44` | CLAUDE.md index.md 例外追記 |
| `4bded5a` | 教科書第2号企画 必須3セクション追加 |
| `39cbc3f` | vault_improvement_proposals.md 一括 resolved |
| `e98933c` | task_hub.md frontmatter 修正 + セットアップ手順追加 |

---

## ✅ うまく行ったこと

- `git mv` で 7 ファイルを一括移動できた。履歴が断絶せず `git log --follow` で追跡可能
- Markdown リンク形式の参照が 0 件だったため、path 書き換え作業がほぼ不要だった (WikiLink のみで記述されていたのが功を奏した)
- task_hub.md が前セッション (03:28 頃) で既に作成済みだったため、Phase 2 は確認・微修正で完結できた
- vault_improvement_proposals.md の 5 件 pending を一気に処理できた

## ❌ 詰まったこと

- **ε C cron prompt が見つからなかった**: Vault 内全 .md / CronList / .claude/ 配下を調査しても「cron prompt」として独立ファイル化されたものが存在しなかった。ε C 監視は一過性セッションとして実行されており、提案にあった「cron prompt の grep パターン修正」は対象ファイルが存在しないため適用不可 → wontfix で処理
- **log.md が並行セッションで更新されており Edit が一度失敗**: 並行で動いていた別 CC セッションが log.md を書き込んでいたため、Read 後に内容が変わってしまい Edit エラー。再 Read して修正した

## 📋 次回同じことをするときのチェックリスト

Vault メンテバッチを実施する際の手順:

- [ ] `vault_improvement_proposals.md` の pending を全件読み込んで対応可否を判断
- [ ] ファイル移動は必ず `git mv` で履歴保全 (単純 mv は git 追跡が切れる)
- [ ] 移動前に Markdown リンク `[..](path)` 形式の参照を grep で確認
- [ ] WikiLink `[[name]]` は Obsidian の basename 解決で自動追従するため原則変更不要
- [ ] cron prompt を見つけられない場合、proposals の wontfix 化 + 「次回起動時は新規作成が必要」の注記を残す
- [ ] log.md は並行セッションが書き込む可能性があるため、Edit 直前に再 Read
- [ ] commit は作業単位で分割 (1 メンテ = 1 commit の粒度を守る)

## 関連

- [[vault_improvement_proposals]] — 解決した提案リスト
- [[CLAUDE]] — 例外追記した必須3セクションルール
- [[task_hub]] — 整備した運用マニュアル
- [[active_projects]] — current_focus を整合させたプロジェクト一覧
