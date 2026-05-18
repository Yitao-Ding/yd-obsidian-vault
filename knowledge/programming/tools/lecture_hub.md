---
type: knowledge
domain: programming/tools
created: 2026-05-17
last_updated: 2026-05-18
tags: [lecture-hub, notion, blocknote, drizzle, supabase, pgvector, ai, whisper, offline]
priority: high
---

# Lecture Hub — 個人専用 Notion 風ワークスペース

> 大学講義ノート・タスク・AI チャットを一元管理する、YD 個人専用の Notion クローン。
> 2026-05-17 に MVP shipped、2026-05-18 に Phase 2 全項目 + 個人用転換まで完了。

## 🎯 何のツールか

YD が「講義ノート」「日々のタスク」「AI への質問」を 1 か所で扱うための個人 SaaS。Notion 風のブロックエディタに、自分の Anthropic / Google / OpenAI キーで動く AI を内蔵。

詳細な転換決定は [[2026-05-18_lecture_hub_個人用転換]] 参照。

## 📍 場所

- リポジトリ: `/Users/ittou/projects/lecture-hub`
- 本番 URL: `https://lecture-hub-yitao-ding-yitao-dings-projects.vercel.app/`
- GitHub: (Vercel と直結)

## 🧱 スタック

| レイヤ | 採用 |
|---|---|
| フレームワーク | Next.js 15 App Router + React 19 + TypeScript |
| スタイル | Tailwind CSS v4 (`@theme` CSS-first) |
| エディタ | BlockNote 0.51 (ariakit テーマ) + 自作カスタムブロック |
| DB | Supabase Postgres (DATABASE_URL のみ使用) + Drizzle ORM |
| 全文検索 | Postgres tsvector |
| セマンティック検索 | pgvector + Google `text-embedding-004` (768 dim) |
| AI | Vercel AI SDK + `@ai-sdk/{anthropic,google,react}` |
| 音声 | OpenAI Whisper |
| ストレージ | Vercel Blob (添付・PDF・音声) |
| Cron | Vercel Cron (`vercel.json`) |
| オフライン | Dexie (IndexedDB) + 自作 sync |
| 認証 | **なし** (個人専用) |

## 🚀 主な機能

| 場所 | できること |
|---|---|
| `/tasks` | タスク CRUD、優先度バッジ循環、期限フィルタ |
| `/p/[id]` | BlockNote ノート + AI Slash メニュー (要約 / タスク抽出 / 数式 / PDF / 音声) |
| `/search?q=...` | 全文 (tsvector) + セマンティック (pgvector) 切替 |
| `/chat` | AI チャット (Anthropic / Google)、スレッド履歴付き |
| `/admin/reindex` | pgvector の全ページ再 index |
| トップバー右 | 同期インジケータ + ダークモード切替 |

### BlockNote カスタムブロック (`/` Slash メニュー)

- **数式 (LaTeX)** — KaTeX、`⌘+Enter` で確定
- **コードブロック** — Shiki シンタックスハイライト (TS/JS/Py/Rust/Go/SQL 他)
- **PDF 埋め込み** — URL or Vercel Blob アップ
- **音声 + 文字起こし** — 音声ファイル → OpenAI Whisper で日本語文字起こし
- **AI: 要約を挿入** — 周囲のテキストを要約 → 引用ブロックで挿入
- **AI: タスク抽出** — 周囲のテキストから ToDo を抽出 → tasks テーブルへ自動投入

### Cron 自動生成 (vercel.json)

| path | 時刻 | 内容 |
|---|---|---|
| `/api/cron/daily-journal` | 06:00 JST | `日記/YYYY-MM-DD` ページを作成 |
| `/api/cron/weekly-lecture` | 07:00 JST 月曜 | `講義/Week of YYYY-MM-DD` + 科目ページ |

科目リストは `src/lib/cron/templates.ts` の `LECTURE_SUBJECTS` を編集。

### オフライン編集 (Phase D4)

- ページを開いた時点で IndexedDB (`lecture-hub-offline`) にキャッシュ
- 編集 → Dexie に即書込 + debounce 後にサーバー保存
- 保存失敗 (オフライン含む) は `dirty=1` で残し、`online` イベント / 60秒間隔で自動 flush
- Topbar に同期ピル: **オンライン / オフライン / 同期中 / 同期失敗** + 保留件数
- 制限: フルオフライン (Next.js ルートそのもの) は未対応。Service Worker は Phase E 候補

## 🛠 開発の前提・規約

詳細は `/Users/ittou/projects/lecture-hub/CLAUDE.md` を参照。要点だけ:

- **認証を復活させない** — `@supabase/ssr` は依存から外してある。`requireUser()` / `ownerId` も全部消した
- **AI キーは env vars** — Vault / DB / Cookie に保存しない
- **DB は Drizzle 経由のみ** — `supabase.from(...)` パターンは復活させない
- **migration は手動適用** — Supabase SQL Editor で順に走らせる
- **`pnpm dev` は勝手に立ち上げない** — Claude のサンドボックスから Postgres ポートが抜けない事故あり

### env vars (.env.local)

| key | 用途 | 必須 |
|---|---|---|
| `DATABASE_URL` | Postgres | ✅ |
| `ANTHROPIC_API_KEY` | Claude (チャット / 要約 / タスク抽出) | AI 使うなら |
| `GOOGLE_GENERATIVE_AI_API_KEY` | Gemini + Embedding | セマンティック検索使うなら |
| `OPENAI_API_KEY` | Whisper | 音声ブロック使うなら |
| `BLOB_READ_WRITE_TOKEN` | Vercel Blob | PDF/音声使うなら |
| `CRON_SECRET` | Vercel Cron auth | Cron 使うなら |

### Migrations (順に手動適用)

```
0001_init.sql                  (2026-05-17 適用済み)
0002_drop_multitenancy.sql     ← 2026-05-18 作成、未適用
0003_pgvector.sql              ← 2026-05-18 作成、未適用
```

## 📚 ディレクトリ

```
src/app/
  layout.tsx                    ルート (テーマ初期化 inline script)
  page.tsx                      → /tasks へリダイレクト
  (app)/                        サイドバー付きレイアウト
    tasks/                      タスクダッシュボード
    p/[id]/                     ページ編集 (BlockNote)
    search/                     全文 + セマンティック検索
    chat/                       AI チャット
    admin/reindex/              pgvector 一括再 index
  api/
    ai/{chat,summarize,extract-tasks,threads}/
    cron/{daily-journal,weekly-lecture}/
    search/, search/semantic/
    transcribe/, upload/

src/components/
  editor/
    Editor.tsx                  BlockNote ラッパー
    schema.ts                   カスタムスキーマ (Shiki + math/pdf/audio)
    blocks/{Math,Pdf,Audio}Block.tsx
    ai-slash-items.tsx          Slash メニュー追加項目
  sidebar/, topbar/
  ui/                           shadcn 風 (Notion トークンで再皮膚化)

src/lib/
  db/{client,schema,queries}.ts Drizzle
  ai/{providers,embeddings,messages}.ts
  blocknote/text.ts             plainTextFromDocument / plainTextAround
  cron/templates.ts             テンプレ定義
  offline/{db,sync}.ts          Dexie + 同期

supabase/migrations/            0001/0002/0003
```

## 🧪 テスト

`pnpm test` で vitest が走る (26 件、DB 非依存の純粋関数のみ):

- `plainTextFromDocument` / `plainTextAround`
- `toVectorLiteral`
- `uiMessageText` (旧/新 SDK 両形式)
- `renderDailyJournal` / `renderWeeklyLecture` (JST 境界・構造)
- `isAuthorizedCron` (Bearer / query secret)

## 📅 進捗履歴

### 2026-05-17 — MVP shipped
- Supabase Auth (magic link) + Vault (AI キー暗号化) + PAT 発行で公開 API
- BlockNote エディタ + タスク管理 + AI 要約
- 本番 Vercel デプロイ
- 詳細: `git log --oneline` の `70c86fe` `3386865` `ca2f0bc`

### 2026-05-18 — 個人用転換 + Phase 2 全完了
- **Phase A**: 認証システム全削除 — Supabase Auth / Vault / RLS / api_tokens / ai_keys / `(auth)` / `/api/v1` / Upstash / middleware を一掃。全クエリを PostgREST → Drizzle に移行。AI キーは env vars 直読み
- **Phase B1**: ダークモード — `.dark` クラストグル + globals.css 上書き + FOUC 防止 inline script + BlockNote 連動
- **Phase B2**: ノート内 AI Slash メニュー (要約 / タスク抽出)
- **Phase B3**: 講義テンプレ Cron (daily-journal / weekly-lecture)
- **Phase C1**: 全文検索 UI (tsvector + Topbar ライブドロップダウン + `⌘K`)
- **Phase C2**: AI チャット履歴 UI (`ai_threads` / `ai_messages` 永続化 + `/chat` ページ + `@ai-sdk/react` の `useChat`)
- **Phase D1**: 数式 (KaTeX) / コードハイライト (Shiki) / PDF 埋め込み
- **Phase D2**: 添付ファイル + Whisper 文字起こし
- **Phase D3**: pgvector セマンティック検索 (Google text-embedding-004 / 768 dim) + 自動 reindex + `/admin/reindex`
- **Phase D4**: オフライン編集 (Dexie + sync) + Topbar 同期インジケータ
- リファクタ + CLAUDE.md 整備 + vitest 26 件

## ⚠ 家でやる残作業 (家のネット環境で)

1. Supabase SQL Editor で `0002_drop_multitenancy.sql` と `0003_pgvector.sql` を順に適用
2. `.env.local` の `ANTHROPIC_API_KEY` / `GOOGLE_GENERATIVE_AI_API_KEY` / `OPENAI_API_KEY` / `BLOB_READ_WRITE_TOKEN` / `CRON_SECRET` を埋める
3. Vercel ダッシュボードで Blob 統合を有効化 → `vercel env pull` で `BLOB_READ_WRITE_TOKEN` 取得
4. `pnpm dev` で動作確認:
   - Topbar 同期インジケータが「オンライン ✓」になるか
   - 機内モードで編集 → ネット復帰で flush されるか
   - 各 Slash メニュー項目が動くか (要 AI キー)
5. `vercel --prod` で本番デプロイ更新

## 🔮 Phase E 候補 (未着手)

- Service Worker — ナビゲーションごとオフラインで動かす
- 「キャッシュ済みページ一覧」UI
- 衝突解決 UI (現状 last-write-wins)
- AI メッセージへの引用 / コードブロックレンダリング (現状 plain text)
- モバイル PWA としての最適化 (touch / viewport)

---

## ✅ うまく行ったこと

- **BlockNote 0.51 (ariakit) + Drizzle + Vercel Blob の組み合わせが認証なしで完動** — 個人用 Notion クローンのスタックとして今のところベスト
- **`after()` (`next/server`) で保存後の embedding 再生成を非同期に逃せる** — 保存自体は速いまま pgvector が更新される。Server Action でも使えるのが効く
- **Topbar の `SyncIndicator` を `MutationObserver` で `<html>` の class を監視する設計** — ダークモード切替に追従しやすく、状態の重複管理が消える
- **`plainTextFromDocument` / `plainTextAround` の共通化** — AI Slash / embedding / 全文検索の 3 ヶ所の重複が消えて保守性が上がった
- **vitest 26 件、全部 DB 非依存** — ネット無くても回せる。CI 不要、`pnpm test` 一発
- **`drizzle-kit push` ではなく SQL migration 手書き** — pgvector / SECURITY DEFINER / RLS を意識した DDL を温存できる。Supabase との相性も良い
- **`@ai-sdk/react` の `useChat` + `DefaultChatTransport` の `prepareSendMessagesRequest` で `threadId` を毎リクエスト付与** — 既存スレッド再開もスムーズ

## ❌ 詰まったこと

- **`createReactBlockSpec` の返り値はファクトリ関数** — `MathBlock` を schema にそのまま渡すと TS エラー。`MathBlock()` で呼ぶ必要あり
- **BlockNote の `render` 内に React Hook を書くと `react-hooks/rules-of-hooks` 違反** — `render: (props) => <InnerView {...props} />` の形に分離する必要がある
- **Slash menu のカスタム化に 3 つの import が必要** — `SuggestionMenuController` + `getDefaultReactSlashMenuItems` (どちらも `@blocknote/react`) + `filterSuggestionItems` (`@blocknote/core`)。1 ヶ所からは出てこない
- **pgvector の演算子: cosine `<=>` / 内積 `<#>` / L2 `<->`** — クエリ書く時に混同しやすい。今回は `<=>` (cosine distance)、score は `1 - (a <=> b)` で類似度に変換
- **embedding 次元 (768) と ivfflat `lists`** — `text-embedding-004` は 768、OpenAI `text-embedding-3-small` は 1536。次元を変えると schema と index ごと作り直し必要
- **BlockNote document JSON の `block.content`** — `undefined` だったり配列だったりするので、必ず `Array.isArray()` で防御。共通ヘルパーに集約済み
- **Next.js 15 + React 19 + Tailwind v4 + BlockNote の peer dep がうるさい** — `pnpm install` で警告は出るが動く。深追いせず無視

## 📋 次回同じことをするときのチェックリスト

- [ ] `pnpm install` 直後に `pnpm exec tsc --noEmit` で型が通るか確認
- [ ] migration は順序厳守 (`0001` → `0002` → `0003`)、Supabase SQL Editor で貼り付け実行
- [ ] `.env.local` の必須 keys を埋めてから `pnpm dev` (`DATABASE_URL` だけは絶対必要)
- [ ] 新しいカスタムブロックを追加するときは:
  - [ ] `createReactBlockSpec(config, impl)` の返り値はファクトリ → `schema.ts` で `MyBlock()` を呼ぶ
  - [ ] `render` 内に Hook を書かず、内部コンポーネントに切り出す
  - [ ] `propSchema` の default 値は **その型のリテラル** で書く (default: "" / default: 0 / default: true)
- [ ] Slash menu に項目を増やすときは `src/components/editor/ai-slash-items.tsx` の `customSlashItems` を編集
- [ ] BlockNote document を扱うコードを書く前に `src/lib/blocknote/text.ts` のヘルパーで足りるか確認 (新たに `block.content.map(...)` を書かない)
- [ ] AI モデルを切り替えるなら `src/lib/ai/providers.ts` の `DEFAULT_MODELS` を編集
- [ ] embedding 次元を変える場合は **migration を必ず更新** + 既存 `embedding` を NULL に → `/admin/reindex` で再生成
- [ ] Cron を追加するときは `vercel.json` に schedule + `/api/cron/<name>/route.ts` + `isAuthorizedCron(req)` の 3 点セット
- [ ] 削除後の build で `.next/types/` の幻影エラーが出たら `rm -rf .next tsconfig.tsbuildinfo`
- [ ] サンドボックスから Postgres ポートが抜けない事故への備え: migration は SQL Editor 適用、自動化を試みない

## 🔗 関連

- [[2026-05-17_lecture_hub_MVP_shipped]] (もし作るなら)
- [[2026-05-18_lecture_hub_個人用転換]]
- [[task_hub]] — 同じスタック・別目的の前作プロジェクト
- [[obsidian_vault]] — このノート自体の運用基盤
