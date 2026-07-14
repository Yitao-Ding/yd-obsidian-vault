---
type: knowledge
domain: programming/tools
last_updated: 2026-07-11
---

# Lecture Hub — 個人専用 Notion 風ワークスペース

## 概要

`/Users/ittou/AI projects/lecture-hub` にある **本人 (ittou) 専用**の Notion 風ワークスペース。講義ノート・タスク・AI チャットを1つに。マルチテナント・認可・課金は**意図的に無い**(シングルユーザー前提)。

### スタック
- Next.js 15 App Router + React 18.3 + TS strict
- Tailwind v4 CSS-first (`@theme` in `globals.css`)、ダークは `.dark` トグル
- Postgres = Supabase (DATABASE_URL のみ) + Drizzle ORM + pgvector
- **TipTap v3** (ProseMirror) + 自作カスタム Node + Toolbar
- Vercel AI SDK v6 (`ai` ^6) + Blob + Cron
- Dexie (IndexedDB) でオフライン編集キャッシュ

### 境界
無認証アプリなので、公開デプロイ時は Vercel Deployment Protection か合言葉ゲート (`GATE_PASSWORD`) で外側に境界を張る。ゲートはマルチテナント認証の復活ではない (ユーザーテーブルも owner_id も無い単一合言葉ロック)。

## 2026-07 全面改修 (Fable 司令塔 + Opus/Sonnet 実装)

YD が「Opus に作らせたが、フロント/バック/UI/UX の使い勝手が気に入らない。0→100 で全部見直したい」と依頼。体制 = **Fable が監査・計画・検収を担い、実装は Opus/Sonnet に委譲** ([[model-orchestration-policy]] 相当)。

- **監査**: Workflow で 12 次元並列レビュー + bug/security の敵対的検証 + 完全性クリティーク → 135 所見 (P1×9 / P2×63 / P3×63)
- **実装**: 4 Wave / 便ごとにファイル所有権を分離し二重編集を防止
  - Wave 1: データロスト2経路根治 (サーバー版数 `pages.rev` 導入) / 境界セキュリティ硬化 / AI チャットに**ノート本文の RAG 注入** + stop で課金停止 + 孤児メッセージ根絶 + モデル選択
  - Wave 2: 検索の**日本語ランキング根治** (word_similarity) + 全文スニペット + 複数語ハイライト + チャンク embedding + 統合検索 / エディタに**画像・テーブル・スラッシュコマンド・ドラッグ並べ替え・言語切替**を追加
  - Wave 3: シェル/ナビ/タスク UX (⌘K パレット・パンくず・dnd・タスク編集・loading) / 品質整理・エクスポート・CI / **デザイン全面刷新** (寒色スレート + navy ダーク、WCAG AA、LP デッドトークン除去)
- **成果**: ブランチ `overhaul/2026-07` に 5 コミット、113 files +9,111/−1,369、tsc/160 test/lint 全 green
- **migration 0005〜0008** は YD が Supabase SQL Editor で番号順手動適用 (0007 は `pages.embedding` drop → 適用後 `/admin/reindex` 必須)

## ✅ うまく行ったこと
- **監査を schema 付き Workflow で並列化**し、bug/security だけ敵対的検証 (複数の懐疑的検証官に「反証せよ」) にかけたことで、refuted 11 件をノイズとして事前に落とせた。実装対象が締まった
- **ファイル所有権を便ごとに固定**したので、6 便 + 代行を並列で回しても実質的な編集衝突はゼロ (media.ts の越境だけ司令塔裁定で W1-B に一本化)
- Fable が各便の完了報告を**鵜呑みにせず**、tsc/test/lint を毎回自分で再現 + 重要修正はコード直読 + WCAG は自分でコントラスト実測 ([[claude_mistakes]] E-5 の教訓を実践) → 報告と実体の乖離を防げた
- 状態を常に `OVERHAUL_2026-07.md` (リポジトリ直下) + git コミット + タスクリストに置き、いつ /clear しても新セッションで続行できる形を維持 ([[context-economy-policy]])

## ❌ 詰まったこと
- **構造化出力 Workflow の retry cap 超過** ([[claude_mistakes]] A-21): 監査官に所見を大量に出させると agent({schema}) が巨大配列を1回で返そうとして StructuredOutput の retry cap (5) を超え、便が全滅。当該次元を2分割 + 「上位N件・簡潔に」の出力規律追加 + 完走分のキャッシュ再利用で復旧
- **Max プランのセッション上限**で Wave 1 の3便が同時停止 (リセット 03:20 JST)。編集ゼロ・tree クリーンだったので、リセット後に各便へ SendMessage で再開して被害なし
- サンドボックスから Postgres (5432/6543) に TCP が抜けず、`pnpm dev`/build も禁止。**動的検証は tsc --noEmit + vitest のみ**。デザインの主観評価・migration 適用・実機フローは YD の家作業に委ねた

## 📋 次回このプロジェクトを触るときのチェックリスト
1. まず `CLAUDE.md` (リポジトリ直下) と `OVERHAUL_2026-07.md` を読む。git log を source of truth に
2. **絶対に復活させない**: Supabase 認証 / requireUser / owner_id / RLS / `drizzle-kit push`。境界ゲートは消さない
3. DB アクセスは Drizzle 経由のみ。スキーマ変更は `supabase/migrations/000X_*.sql` 新規 + README 追記 (適用は YD 手動)
4. 数式入力は `$$...$$` (単一 `$` は TipTap v3 で不可)。エディタ CSS は `.rich-text` 自前実装が正 (typography プラグイン不使用)
5. TipTap 拡張のバージョンは**コア (3.23.4) と一致**させる (浮くと ProseMirror 二重化で壊れる)
6. 保存系は `updatePage` 経由一本化。onChange は 150ms デバウンス + 離脱時同期 flush (データロスト非回帰を必ず確認)
7. サンドボックスでは tsc + vitest のみ。dev/build/migration 適用は YD に委譲。「動作確認する?」と毎回聞かない

## 関連
- リポジトリ: `/Users/ittou/AI projects/lecture-hub` (ブランチ `overhaul/2026-07`)
- 意思決定: [[2026-07-11_lecture-hub全面改修]]
- 過去: [[2026-05-19_tiptap_migration]] (BlockNote→TipTap 移行)
- ミス: [[claude_mistakes]] A-21 (構造化出力 retry cap)
