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

## 🔗 関連

- [[2026-05-17_lecture_hub_MVP_shipped]] (もし作るなら)
- [[2026-05-18_lecture_hub_個人用転換]]
- [[task_hub]] — 同じスタック・別目的の前作プロジェクト
- [[obsidian_vault]] — このノート自体の運用基盤
