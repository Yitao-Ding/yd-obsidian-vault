---
type: knowledge
domain: programming/skills
tool: backend-patterns
last_updated: 2026-05-21
---

# backend-patterns skill

Backend アーキテクチャパターン / API 設計 / DB 最適化 / サーバーサイドベストプラクティス用の Claude Code skill。YD が 2026-05-21 に提供。設置場所: `~/.claude/skills/backend-patterns/SKILL.md`

## origin: ECC の正体 (2026-05-21 特定)

**ECC = "Everything Claude Code"** — Affaan Mustafa 氏 (`affaan-m`) が公開している harness-native operator system for agentic work。

- リポジトリ: https://github.com/affaan-m/everything-claude-code
- 該当 SKILL.md: https://github.com/affaan-m/everything-claude-code/blob/main/skills/backend-patterns/SKILL.md
- ライセンス: MIT (商用版 ECC Pro もあり)
- 規模: **60 agents + 232 skills + 75 legacy command shims + 12 言語エコシステム横断ルール + automation hooks**
- 出自: **Anthropic Hackathon Winner** (Cerebral Valley × Anthropic, 2026年2月)
- 対応 harness: Claude Code / Cursor / OpenCode / Codex / GitHub Copilot / Zed 他

つまり今回入れた backend-patterns は、ECC 内 232 skills の中の 1 つ。**ECC 全体を導入すれば残り 231 skills + 60 agents + その他資産も使えるようになる**。

## 概要

Node.js / Express / Next.js API routes 中心の **backend 制作時参照 skill**。コード例は **Supabase + Next.js App Router** を主軸にしたパターン集。

### 含まれるパターン群

| カテゴリ | 内容 |
|---------|-----|
| API Design | RESTful URL 設計、Repository、Service Layer、Middleware |
| Database | Query Optimization (select only needed)、N+1 防止、Transaction (Supabase RPC) |
| Caching | Redis Caching Layer、Cache-Aside Pattern (TTL 300s) |
| Error Handling | Centralized Error Handler (ApiError + Zod)、Retry with Exponential Backoff |
| Auth | JWT 検証、Role-Based Access Control (HOF パターン) |
| Rate Limiting | **必ず shared store (Redis / Gateway) を使う**、in-memory 禁止 (multi-instance/serverless で fail open) |
| Background Jobs | Simple in-process JobQueue (本格運用は別 skill / 別仕組みへ) |
| Logging | Structured JSON Logging (level + context + requestId) |

## トリガーフレーズ

YD が言いそうな:
- 「API 設計」「REST endpoint」「DB クエリ最適化」「N+1」「キャッシュ層追加」「認証ミドルウェア」「rate limit」「JWT」「Repository パターン」
- `api/`, `server/`, `lib/db/`, `route.ts` 配下を触る作業全般

## 既存 [[ui-ux-pro-max]] / [[frontend-design]] / [[web-design-guidelines]] との関係

これら **3 skill は frontend 専用**、backend-patterns は **backend 専用**。**併用関係ではなく独立**。

UI と API を両方触るフルスタックタスクでは:
- frontend-design + ui-ux-pro-max + web-design-guidelines (UI 側)
- backend-patterns (API 側)
- → それぞれの担当ファイルに対して別個に適用

## 重要な注意点

### 1. Rate Limiting の絶対ルール (SKILL.md 明記)

> Rate limiting must use a shared store such as Redis, a gateway, or the platform's native limiter. Do not use per-process in-memory counters for production APIs.

理由: deploy で reset、replicas 間で split、serverless で fail open。

### 2. 暗黙の前提スタック

- **Supabase** (`supabase.from(...)`、`supabase.rpc(...)` 等が頻出)
- **Next.js App Router** (`NextResponse.json`、`request: Request` 型)
- **zod** (バリデーション)
- **jsonwebtoken** (JWT)
- **redis** (キャッシュ層)

YD のプロジェクトで **Prisma を使う場合 / 他の DB を使う場合**は、Repository pattern の interface は使えるが具体実装は読み替え必要。

### 3. Background Jobs

SKILL.md 内の `JobQueue<T>` クラスは**プロセス内シンプル queue**。本格運用 (永続化、retry、多インスタンス) は別仕組み (BullMQ / pg-boss / Inngest / Vercel Workflow など) を別途検討。

## 設置情報

- 設置元: YD 直接提供 (SKILL.md 全文)
- 設置パス: `~/.claude/skills/backend-patterns/SKILL.md`
- frontmatter: `name: backend-patterns` / `description: Backend architecture patterns... Node.js, Express, and Next.js API routes` / `origin: ECC`

## ✅ うまく行ったこと

- 既存の UI/UX 系 3 skill とドメインが綺麗に分かれる (backend / frontend) ので運用ルールが衝突しない
- パターン集が**コード例ベース**で具体的 → 「これに沿って」が指示しやすい
- Rate limiting の落とし穴 (in-memory 禁止) など、**実害が出るアンチパターン**を明示的に書いてくれてる
- Repository + Service Layer + Middleware の 3 層分離パターンが提示されていて、Salamat / Lecture Hub などの今後の API 拡張に直接使える

## ❌ 詰まったこと

- 該当なし (2026-05-21 設置時点)。実プロジェクトで使ってみて、Supabase 以外の DB (Prisma / Drizzle) と組み合わせた時の摩擦などが出たらここに追記する
- ~~`origin: ECC` の出処が不明~~ → 2026-05-21 解決: ECC = "Everything Claude Code" (`affaan-m/everything-claude-code`)。SKILL.md 本文には ECC の説明がなく Vault 内にも手がかりゼロ、Web 検索 (`Rate limiting must use a shared store...` の一字一句マッチ) で `affaan-m/everything-claude-code/blob/main/skills/backend-patterns/SKILL.md` がヒットして判明
- 教訓: frontmatter の謎略号は **SKILL.md 本文の固有性が高いフレーズで Web 完全一致検索**すると配布元が一発で出る (パターン集系 skill は特に独特の表現が多くて検索が効きやすい)

## 📋 次回同じことをするときのチェックリスト

1. backend 作業の時は **必ずこの skill を 1 回呼ぶ** (frontend 系 skill と違って併用相手なし、単体で動く)
2. Rate limit を実装する時は **絶対 in-memory にしない** (SKILL.md 明記の禁止事項)
3. N+1 が疑われる loop を見たら、batch fetch + Map 化に書き換える (SKILL.md の Pattern どおり)
4. Repository → Service → Route handler の 3 層分離を**新規 API は最初から守る** (後でリファクタはコスト高)
5. プロジェクトのスタックが Supabase じゃない場合、Repository pattern の interface はそのまま、実装側だけ読み替え
6. **ECC 系の他 skill が必要になったら** `affaan-m/everything-claude-code` リポジトリを直接見る (232 skills 中から類似カテゴリの skill を抜き出す or ECC 全体導入を検討)
7. frontmatter に **未知の origin 略号**を見つけたら、SKILL.md 本文の独特なフレーズ (今回なら Rate limiting の英文 1 行) で **Web 完全一致検索**してリポジトリ特定

## 📚 関連

- [[index]] (knowledge/programming/skills/) — skills 領域のハブ、運用ルール集約
- (将来) [[api-design]] / [[security-review]] — SKILL.md 内で参照されている関連 skill (現状未確認)
