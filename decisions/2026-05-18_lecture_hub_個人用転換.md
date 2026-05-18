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

---

## ✅ うまく行ったこと

- **PostgREST → Drizzle 一本化が機械的に進んだ** — schema は元から定義済みだったので、`supabase.from(...).select()` を `db.select().from(...)` に置換するだけ。`owner_id` 列削除も型エラーが全箇所出てくれて漏れなく追跡できた
- **認証ディレクトリを `rm -rf` で一気に削除できた** — `(auth)/`, `(app)/settings/`, `lib/auth/`, `lib/supabase/`, `api/v1/`, `middleware.ts` がグラフ的に末端だったので参照漏れゼロ。事前に `grep -rn` で参照を 1 回確認したのが効いた
- **migration 0002 を「冪等な DDL」で書いたら何度叩いても壊れない** — `do $$ if exists ... end$$` パターンで policy 一括 drop、`alter table ... drop column if exists`、`create index if not exists`。再実行で副作用ゼロ
- **AI キーを env vars に集約 → コードが綺麗になり CLAUDE.md にも「Vault 復活禁止」と明記できた** — Server Action から非同期 RPC 1 本減ったのでレスポンス速度も改善
- **1 セッションで Phase A〜D4 を一気通貫** — 中間で大きな路線変更なし。フェーズごとに `pnpm exec tsc --noEmit` を回す習慣で型エラーを早期検出
- **vitest を入れた瞬間に 26 件パスした** — 純粋関数を切り出して個別にエクスポートしていたおかげで、テストを後付けでも書きやすい構造になっていた

## ❌ 詰まったこと

- **Phase A 完了直後の `pnpm build` で `.next/types/app/(auth)/login/page.ts などが見つからない` の TS エラー** — 削除済みのルートを `.next/types/validator.ts` がまだ参照していた。`rm -rf .next tsconfig.tsbuildinfo` で解消
- **`createReactBlockSpec` の返り値が「ファクトリ関数」だと知らずに `MathBlock` を直接 schema に渡してエラー** — TS が `(options?) => BlockSpec` を `BlockSpec` に代入できないと教えてくれて発覚。`MathBlock()` で呼び出す必要あり
- **BlockNote の `render` 内に `useState` を直書きしたら `react-hooks/rules-of-hooks` で ESLint 拒否** — `render` は React コンポーネント名でないため Hook を呼べない。`MathView` / `PdfView` / `AudioView` を内部コンポーネントとして切り出して `render: (props) => <MathView {...} />` の形に修正
- **Slash menu に項目を追加する API が分かりづらい** — `@blocknote/react` の `SuggestionMenuController` + `getDefaultReactSlashMenuItems` + `@blocknote/core` の `filterSuggestionItems` の 3 つを別 import で揃える必要がある
- **`useChat` (Vercel AI SDK v6) の API が v5 と全然違う** — `messages` を直接渡せず、`DefaultChatTransport` + `prepareSendMessagesRequest` を経由する必要がある。マイグレーションガイドを読みながら手探り
- **サンドボックスから Supabase Postgres 5432/6543 への TCP がブロックされ migration 自動適用不可** — HTTPS REST (Supabase API) は到達するのに Postgres プロトコルだけ抜けない。途中で気付き、ユーザーに「家で SQL Editor から適用」と引き渡した

## 📋 次回同じ判断をするときのチェックリスト

- [ ] 「個人用かマルチテナントか」をプロジェクト初期に明確に決める — 後から「個人用に転換」は今回みたいに 1 日で剥がせるが、その逆 (個人用 → マルチテナント) は遥かに重い
- [ ] マルチテナントを温存するコストを事前に見積もる: Server Action × N、RLS、Vault、PAT、env 管理、migration の order…
- [ ] 個人用と決まったら **全レイヤから `owner_id` を一括で消す覚悟** を最初に固める (半端は最悪)
- [ ] PostgREST → ORM 移行は「schema があれば機械的」なので、Drizzle / Prisma の schema を先に固める
- [ ] AI キーは個人用なら **絶対に env** (Vault に置く理由が無い)
- [ ] migration は必ず `drop ... if exists` の冪等 DDL で書く (再実行で副作用ゼロを保証)
- [ ] BlockNote の `createReactBlockSpec` は **ファクトリ関数を返す** → `XxxBlock()` で呼ぶ
- [ ] BlockNote `render` の中に React Hook を直書きしない → 内部コンポーネントに切り出す
- [ ] 削除後の build で `.next/types/` 由来の TS エラーが出たら `.next/` と `tsconfig.tsbuildinfo` を rm
- [ ] フェーズ完了ごとに `pnpm exec tsc --noEmit` を回す (build は最後の 1 回でいい)
- [ ] サンドボックスから Supabase Postgres ポートが抜けない前提で、migration は SQL Editor 適用に倒す (ユーザーに任せる、自動化を試みない)

## 関連

- [[lecture_hub]] — プロジェクトの全体マニュアル
- [[active_projects]] — 進行中プロジェクト一覧
- [[2026-05-18_Obsidian_Vault構築完了]] — 同じ日に行った別の大きな決定
