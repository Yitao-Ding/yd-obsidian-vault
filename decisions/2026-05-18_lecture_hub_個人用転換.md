---
date: 2026-05-18
type: decision
category: product/architecture
tags: [lecture-hub, auth, supabase, drizzle, scope-reduction]
---

# Lecture Hub を個人専用に転換 — 認証システム全削除

## 背景

2026-05-17 に Lecture Hub MVP を本番デプロイした時点では、将来「友達も使えるようにする」可能性を残してマルチテナント前提で設計していた:

- Supabase Auth (magic link) でログイン
- 各テーブルに `owner_id` カラム + RLS でテナント分離
- AI キーは Supabase Vault に暗号化保存 (ユーザーごとに `/settings/ai-keys` で登録)
- 公開 API `/api/v1/*` は PAT (Personal Access Token) で保護
- Upstash Redis で公開 API のレートリミット

ただし 2026-05-18 時点で「これは完全に自分専用ツール」とハッキリ決まり、上記の認証・認可レイヤが純粋にオーバーヘッドになっていた。

## 選択肢

- **A. 認証は残したまま、自分1人だけ使う**
  - メリット: 既存コード触らない、将来戻せる
  - デメリット: 毎回マジックリンク必要、`/settings/ai-keys` の Vault 経由が遅い、PAT 管理が面倒、Server Action ごとに `requireUser()` の往復、Vault SECURITY DEFINER 関数のメンテ

- **B. 認証だけ削除、DB は Supabase 継続**
  - Supabase は単なる Postgres プロバイダとして使う
  - RLS / Vault / Auth を全部剥がし、`owner_id` カラム削除
  - AI キーは env vars に移行
  - Drizzle ORM 一本化 (PostgREST 撤去)

- **C. Supabase ごと撤去して Neon / Vercel Postgres へ**
  - DB プロバイダ移行のコストが高い、利益薄い

## 決定

**B を採用**

理由:
1. 接続文字列だけ流用すれば DB 移行コストがゼロ
2. Auth / Vault のレイヤを剥がすことで Server Action がシンプルに (1 リクエスト ≒ 1 クエリ)
3. AI キーを env vars にすることで `/settings/ai-keys` の UI / Vault 関数も削除でき、コード行数が大きく減る
4. `owner_id` を全カラムから消すことで Drizzle の型がきれいに
5. 将来「やっぱり共有したい」となったら Clerk なり次世代 Auth を新規導入する方が、レガシー Supabase Auth を温存するより筋がいい

## 影響

### 削除したもの

- `src/middleware.ts` (Supabase 認証 middleware)
- `src/lib/supabase/*` (server / client / middleware)
- `src/lib/auth/*` (get-user / api-token / pat / scopes)
- `src/app/(auth)/login/` (マジックリンク UI)
- `src/app/auth/callback/` (Supabase コールバック)
- `src/app/(app)/settings/api-tokens/` (PAT 管理 UI)
- `src/app/(app)/settings/ai-keys/` (Vault 経由のキー管理 UI)
- `src/app/api/v1/` (PAT 保護の公開 API、journal/append, tasks/pending, tasks/completed)
- `src/lib/rate-limit.ts` (Upstash ラッパー)
- 依存: `@supabase/ssr`, `@supabase/supabase-js`, `@upstash/redis`, `@upstash/ratelimit`

### 置き換えたもの

- 全 PostgREST クエリ (`supabase.from().select()`) → Drizzle ORM
- AI キー取得: Supabase Vault RPC → `process.env.ANTHROPIC_API_KEY` 等

### 新しい migration

- `supabase/migrations/0002_drop_multitenancy.sql` を作成。RLS / Vault SECURITY DEFINER 関数 / `ai_keys` テーブル / `api_tokens` テーブル / `owner_id` カラム / FK to `auth.users` を全部 drop

## 副産物

この転換と同じ日に Phase 2 残項目を全部実装した:

- B1 ダークモード
- B2 ノート内 AI Slash メニュー
- B3 講義テンプレ Cron
- C1 全文検索 UI (tsvector)
- C2 AI チャット履歴 UI
- D1 数式 / コードハイライト / PDF
- D2 添付 + Whisper
- D3 pgvector セマンティック検索
- D4 オフライン編集 (Dexie + sync)

詳細は [[lecture_hub]] の進捗履歴。

## 学んだこと

- **「将来の柔軟性」のために認証を残すコストは、思ったより高い** — Server Action / RLS / Vault / PAT / env / migration の全レイヤに染み出す
- **個人用と宣言した瞬間、設計判断が一気にシンプルになる** — マルチテナントの呪縛から外れると、Drizzle のクエリも、env の管理も、エラーハンドリングも全部楽になる
- **Supabase は「Postgres + Vault + Auth」のセットで売られているが、Postgres だけ使うのも全然アリ** — DATABASE_URL があれば pgvector / tsvector も普通に使える

## 関連

- [[lecture_hub]] — プロジェクトの全体マニュアル
- [[active_projects]] — 進行中プロジェクト一覧
- [[2026-05-18_Obsidian_Vault構築完了]] — 同じ日に行った別の大きな決定
