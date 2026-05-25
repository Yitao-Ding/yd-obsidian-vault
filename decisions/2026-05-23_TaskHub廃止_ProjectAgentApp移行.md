---
type: decision
date: 2026-05-23
related: [task_hub, project_agent_application]
status: confirmed
---

# Task Hub 廃止 → Project Agent Application へ移行

## 決定

**Task Hub (`~/projects/salamat-task-hub/`) を完全に廃止し、代替として Project Agent Application (`~/projects/project-agent-application/`) を新規開発する。**

- Task Hub の利用は停止
- Salamat 内部運用も含めて、本アプリで完全に置き換える
- コードベースは引き継がない (思想・知見のみ引き継ぐ)

## 背景

### Task Hub の現状 (2026-05-23 廃止時点)
- 状況: 2026-05-21 から UI/UX 根本見直しフェーズに入り、4 自立型エージェント (planner / builder / qa-evaluator / design-evaluator) を構築済
- ただし planner への YD のビジョンインプットはまだ未完了 (Sprint 01 着手前)
- 5/19 に git 整理 + GitHub 連携完了 (5 commit、Firebase Hosting で稼働中: salamat-task-hub.web.app)
- 商用化検討 (大学サークル向けフリーミアム) は再設計完了後の予定だった

### 廃止に至った経緯
- 2026-05-23 13:00 過ぎ、`~/projects/project-agent-application/` 着手 (大学生・若年層向け Teams ライクなプロジェクト/タスク/チーム一元管理アプリ)
- 壁打ちで planner と Q1〜Q11 を詰める中、「Task Hub との関係性」(Q1) が出てきた
- YD「タスクハブは完全にもう使わなくなるから、もう捨てていいよ。あれもうマジで使わないです」と明言
- 本アプリのスコープ (複数団体所属モデル + 4 階層 + Z 世代向け Insta/BeReal デザイン + Finch 型キャラ) が Task Hub の上位互換であり、二重開発は不要と判断

## 採用された選択肢と却下された案

| 案 | 採用 | 理由 |
|---|---|---|
| Task Hub を Salamat 内部運用に留める (初期 Q1 推奨) | ❌ | YD が「使わない」と明言、思想だけ引き継ぐ |
| Task Hub をリブランド (本アプリに昇格) | ❌ | コードベース上の負債 (Firebase Hosting / next-pwa v5 等) を引きずる |
| **Task Hub 完全廃止 + 本アプリ新規開発** | ✅ | クリーンな状態で Q1〜Q11 の仕様を実装、思想・知見は本アプリの SPEC.md / DESIGN_DIRECTION.md に取り込む |

## 影響

### Vault への反映
- `current_state/active_projects.md` から Task Hub セクション (#6) を廃止マーカー化
- `archive/2026-05_TaskHub.md` に廃止時点のスナップショット保存
- `current_state/active_projects.md` に Project Agent Application セクションを追加 (planner フェーズ 1 中) — 別タスク

### 周辺資産
- Task Hub の GitHub Private repo (`Yitao-Ding/salamat-task-hub`) はそのまま放置 (削除しない、参考資料として)
- Firebase Hosting の `salamat-task-hub.web.app` も当面放置 (本アプリ本番リリース後に廃止判断)
- 4 自立型エージェント定義 (`.claude/agents/taskhub-*.md`) は本アプリの planner / builder / qa-evaluator / design-evaluator 構築時のテンプレ参考として活用 (CLAUDE.md に既に参照済)

### Salamat (260 名サークル) の運用
- 本アプリ MVP 完成までは LINE + Notion (現行) で凌ぐ
- 本アプリ完成後、Salamat も移行 (移行スコープは本アプリ SPEC.md で明記)

---

## ✅ うまく行ったこと

- YD の明確な意思表示 (「マジで使わない」) で判断が即決、迷いゼロ
- Task Hub の知見 (4 エージェントハーネス設計、Discord ロールモデル、寒色系、PWA) は本アプリに完全継承可能、捨て損ゼロ
- 5/21 の UI/UX 根本見直し方針 (planner / builder / qa / design 4 エージェント) は本アプリでそのまま再利用 (テンプレ元として活躍)
- 本アプリの Q1 (Task Hub との差別化) で「別物として再設計」が即決まったので、planner の壁打ちが加速

## ❌ 詰まったこと

- 5/21 に UI/UX 根本見直し方針を出して 4 エージェントハーネスまで構築した直後の廃止判断 → 2 日分の作業がアクティブから外れる
- 本来なら 5/21 時点で「これは Task Hub のまま続けるべきか、新規プロジェクトとして再起ちすべきか」を握っておくべきだった
- Task Hub の Firebase Hosting / next-pwa v5 等の技術負債を「直そう」と思っていたが、結局新規で作る方が早いという結論 → 既存資産への過度な執着は時間の無駄

## 📋 次回同様の判断をするときのチェックリスト

- 既存プロジェクトを「リブランド」「機能拡張」「根本見直し」しようとした時、次の問いを立てる:
  1. ユーザー (YD) はそのプロジェクトを継続使用したいか? (Yes でなければ新規一択)
  2. 既存コードベースの負債 (古い依存、移行困難な構造) はどれくらいか?
  3. 新規で作り直すコストと、既存を直すコストの比較
  4. 思想・知見だけ引き継いで、コードは捨てる、が最速ルートではないか?
- 新規開発を選んだ場合、即:
  - `decisions/` に廃止判断を記録 (必須3セクション付き)
  - `current_state/active_projects.md` から旧プロジェクト廃止マーカー化
  - `archive/` に廃止時点のスナップショット保存
  - GitHub repo は当面残す (参考資料 + ロールバック保険)
- 「根本見直し」フェーズに入ったタイミングで、「これは新規プロジェクトとして始めた方が良いのでは?」を即問い直す癖を付ける

## 関連

- [[../knowledge/programming/tools/task_hub]] (運用マニュアル、本決定後は archive 化候補)
- [[2026-05-19_TaskHub_git整理_GitHub連携]] (5/19 の git 整理)
- [[2026-05-21_TaskHub_UIUX根本見直し_4エージェントハーネス構築]] (5/21 の根本見直し方針、本決定で覆る)
- [[../current_state/active_projects]] (本決定で Task Hub セクション廃止マーカー化済)
- [[../archive/2026-05_TaskHub]] (廃止時点のスナップショット)
- 本アプリ: `/Users/ittou/projects/project-agent-application/` (CLAUDE.md + VISION.md)
