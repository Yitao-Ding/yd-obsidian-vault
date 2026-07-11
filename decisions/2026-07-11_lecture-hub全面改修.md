---
type: decision
date: 2026-07-11
project: lecture-hub
---

# lecture-hub 全面改修 (Fable 司令塔 + Opus/Sonnet 実装)

## 状況

YD 指示: 「このツールを今まで Opus に作らせてたけど、フロントエンド・バックエンドの仕組み、UI/UX の使い勝手に気に入らない部分が多すぎる。Fable で 0→100 まで全部見直して、改善点を全部修正した状態にしてほしい。計画・監視・分析は Fable、Fable じゃなくていい作業は Opus 以下でやって」。

追加指示: コンテキストが溜まったら適宜減らす ([[context-economy-policy]])。監視・企画・計画は Fable、実装は Opus 以下 ([[model-orchestration-policy]])。

## 決めたこと・やったこと

**体制**: Fable = 監査設計・改善計画・便の編成/監視・実体検収。実装 = Opus (難所) / Sonnet (機械的)。

**進め方 (Planner/Generator/Evaluator ハーネスの全面適用)**:
1. **監査**: Workflow で 12 次元並列レビュー (opus 8 + sonnet 4) → bug/security 所見を敵対的検証 (懐疑的検証官が「反証せよ」) → 完全性クリティーク。135 所見 (P1×9/P2×63/P3×63)、confirmed 28/refuted 11
2. **計画**: Fable が所見をファイル所有権で分離した 4 Wave / 6 便 + 代行に編成 (`OVERHAUL_2026-07.md` を作業正本に)
3. **実装**: 便ごと Opus/Sonnet 委譲。同一ファイルの書き込みは常に1便 ([[claude_mistakes]] A-19)
4. **検収**: Fable が完了報告を鵜呑みにせず tsc/test/lint 再現 + コード直読 + WCAG 実測 (E-5)

**成果** (ブランチ `overhaul/2026-07`、5 コミット、113 files +9,111/−1,369):
- データロスト2経路根治 (`pages.rev` サーバー版数で時計比較を廃止 / タブクローズを sendBeacon 化)
- 境界セキュリティ硬化 (ブルートフォース対策 / open-redirect / ゲート迂回 / cron 冪等化 / iframe SSRF / レート制限)
- AI チャットに RAG (ノート本文注入) / stop で課金停止 / 孤児メッセージ根絶 / モデル選択・再生成
- 検索の日本語ランキング根治 (word_similarity) / 全文スニペット / チャンク embedding / 統合検索
- エディタ Notion 級 (画像・テーブル・スラッシュコマンド・ドラッグ並べ替え・言語切替)
- デザイン全面刷新 (寒色スレート + navy ダーク、WCAG AA、LP デッドトークン除去、アニメーション)
- エクスポート機能 (バックアップ手段新設) / 最小 CI / migration 0005〜0008

**YD 残作業**: migration 番号順手動適用 → /admin/reindex → pnpm install → 実機確認 → push 判断。

## ✅ うまく行ったこと
- **Planner/Generator/Evaluator を全フェーズで機能**させ、監査 (敵対的検証) と実装 (Fable 検収) の両方で「別モデル/別視点の独立検証」を効かせた。片肺運用 ([[claude_mistakes]] D-6) を回避
- ファイル所有権の事前分離で 6 便並列でも衝突ゼロ。越境 (media.ts) は司令塔裁定で1便に固定
- 状態をディスク (OVERHAUL.md + git + タスク) に常時外部化 → コンテキスト肥大を抑えつつ、いつでも新セッション続行可能

## ❌ 詰まったこと
- 構造化出力 Workflow の retry cap 超過で監査便が2度失敗 → 次元分割 + 出力規律で解決 ([[claude_mistakes]] A-21)
- Max セッション上限で Wave 1 の3便が同時停止 → 編集ゼロ確認の上リセット後に SendMessage 再開
- サンドボックスは DB/dev 不可 → tsc+vitest のみで検証、実機・migration・主観デザイン評価は YD へ委譲

## 📋 次回同じことをするときのチェックリスト
1. 大規模改修は「監査 (敵対的検証付き) → 計画 (ファイル所有権分離) → 実装 (1ファイル1便) → Fable 実体検収 → Wave 単位コミット」の型で回す
2. schema 付き Workflow は「件数上限 + フィールド長上限 + 1回で確定」の出力規律を必ずプロンプトに入れる。広いスコープは分割
3. 実装便の完了報告は鵜呑みにせず、司令塔が tsc/test/lint 再現 + 重要修正はコード直読 + 数値指標 (WCAG 等) は自分で実測
4. 作業正本 (HANDOVER/OVERHAUL) を都度更新し、区切りで「/clear 安全」を YD に明示
5. push は YD 許可制。全面改修は feature ブランチに積んで main は汚さない

## 関連
- [[lecture_hub]] (knowledge/programming/tools)
- [[model-orchestration-policy]] / [[context-economy-policy]]
- [[claude_mistakes]] A-21 / E-5 / A-19 / D-6
