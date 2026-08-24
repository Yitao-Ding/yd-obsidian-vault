---
type: decision
date: 2026-08-23
project: Project Agent Application
---

# Project Agent: GOバッチ適用 (migration 023+024・Edge 12本 deploy・仕様判断2件)

YD 指示「3、4いがい GO やな」(旧判断待ち 1=migration/deploy、2=仕様判断2件、5=push を承認。3=ハル吹き出し語調、4=ハル体色対策は保留 → 同日発案のキャラ再設計に合流)。

実施内容は HANDOVER.md 冒頭「2026-08-23」が正本。要点: 023+024 を実DB `lkrmziwygyyyijyabtzp` に適用 (022 は意図的に据え置き)、Edge 12関数 deploy (旧語調13文言同期)、団体作成の学校情報選択 (トグル) とオンボ4コピー (授業・課題軸) を実装。ワークフロー8エージェント (実装3系統×builder+敵対的verifier + DB precheck) で並列実行、verifier の指摘3件 (オンボ4の「検索」未実装クレーム / 提案書の個人課題モデル矛盾 / treasure-box 旧文言3箇所) を親が修正してからコミット。

## ✅ うまく行ったこと

- 適用前 precheck を read-only で全項目実測 (digests 重複 / tms 名長 / 022 適用有無 / 関数存在) → 適用は一発成功、advisors ERROR 0
- 敵対的 verifier が「オンボ4 コピーの『検索に利用』は未実装機能クレーム」を摘出。builder 自身の却下基準との矛盾を突く検証が機能した
- 実DBの pause (INACTIVE) を precheck 段階で発見。適用がいきなり失敗する前に Management API `/restore` で復旧できた

## ❌ 詰まったこと

- **Supabase 無料枠の自動 pause**: 12日間の無活動で INACTIVE 化、ホスト名が NXDOMAIN になり MCP の execute_sql が全てタイムアウト。原因特定に precheck エージェントが約10分を費やした。restore は Management API POST `/v1/projects/<ref>/restore` で約90秒
- **gh トークン失効 + SSH 鍵なし**で push 不能。`gh auth login -h github.com` は対話式のため YD 待ち
- chrome-devtools MCP の Chrome プロファイルは claude.ai 未ログイン (過去セッションの claude-in-chrome とは別プロファイル)。Claude Design 自動化はログイン1回が前提

## 📋 次回同じことをするときのチェックリスト

1. 実DB 操作の前に project status を確認 (`GET /v1/projects/<ref>` → INACTIVE なら先に `/restore`)。無料枠は無活動 ~7日で再 pause する — 実モード公開前に有料化 or keep-alive を判断
2. migration 適用は「precheck (データ依存の失敗要因を実測) → 適用 → 適用後検証 (pg_policies/pg_trigger/information_schema)」の3段で。単一 txn 内の create unique index はデータ重複で全体 rollback する
3. RPC のシグネチャ変更は named-args 後方互換 (新引数 default null) + 旧シグネチャ drop をセットで。クライアント配信と DB 適用の順序を migration ヘッダに明記
4. push 前に `gh auth status` を先に見る (作業の最後に発覚すると手戻り)
