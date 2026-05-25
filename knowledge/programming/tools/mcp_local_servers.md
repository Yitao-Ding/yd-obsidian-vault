---
type: knowledge
domain: programming/tools
last_updated: 2026-05-23
status: active
related_decision: [[2026-05-23_MCP_9個導入]]
---

# ローカル MCP サーバー 7個 — 運用マニュアル

> 2026-05-23 に user scope で揃えた 7 個のローカル MCP サーバーの運用ノート。
> 個別ファイル化せずこのファイルにまとめる (各サーバーの運用差分は小さい)。
> Playwright だけは詳細を [[playwright_mcp]] に分けている (Web 自動化の中核なので)。
>
> 起動先: `~/.claude.json` の `mcpServers` (user scope)、`claude mcp list` で全件 `✓ Connected` を確認可

---

## 一覧

| サーバー | 起動コマンド (user scope) | 認証 | 主用途 |
|---------|--------------------------|------|--------|
| `playwright` | `npx @playwright/mcp@latest` | 不要 | Web 自動化 (DOM-aware) |
| `serena` | `uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context claude-code --project-from-cwd` | 不要 | LSP semantic 探索・編集 |
| `context7` | `npx -y @upstash/context7-mcp --api-key ctx7sk-...` | API key | 最新ライブラリ docs |
| `chrome-devtools` | `npx chrome-devtools-mcp@latest` | 不要 | Chrome perf / console / network |
| `github` | `gh mcp` (= `shuymn/gh-mcp` 拡張) | `gh auth` 流用 | GitHub API (リポ/PR/Issue) |
| `supabase` | `npx -y @supabase/mcp-server-supabase@latest --read-only --project-ref=lkrmziwygyyyijyabtzp` | env `SUPABASE_ACCESS_TOKEN` | Supabase DB (lecture-hub、read-only) |
| `stripe` | `npx -y @stripe/mcp --tools=all --api-key=rk_test_...` | Test mode key | Stripe API (test mode、Write 含む) |

許可設定: `~/.claude/settings.local.json` の `permissions.allow` に `mcp__<server>` をサーバーレベルで追加すれば全 tool 許可。

---

## Serena

LSP ベースの semantic code search & edit。`Grep` (文字列) と違って AST/定義/参照を正確に追える。

- **典型シーン**: lecture-hub / salamat-website-v2 のような中規模 Next.js プロジェクトで「この関数どこから呼ばれてる?」「この型を使ってる箇所全部」
- **`--project-from-cwd`**: Claude Code 起動 cwd のプロジェクトを自動認識。プロジェクト切り替えのたびに設定し直し不要
- **対応 LSP**: Python / TypeScript / Java / Vue / HTML 等多数
- **小さい修正**: Grep のほうが速い。Serena は中〜大規模の semantic 探索で本領発揮

## Context7

最新ライブラリドキュメントの取得。LLM の「フルめ情報」問題を直接解決する。

- **典型シーン**: Next.js 16 / Tailwind v4 / shadcn/ui のように頻繁に更新されるライブラリの最新仕様確認
- **使い方**: プロンプトに `use context7` を入れる、または `mcp__context7__resolve_library_id` → `mcp__context7__get_library_docs`
- **API key**: あり (`ctx7sk-19b5aed8-...`)。key なしでも公開クエリは可能だが品質は key ありで向上
- **GitHub URL からの追加**: 任意のリポを Add 登録可
- **WebSearch / WebFetch との棲み分け**: ライブラリ仕様 = Context7、汎用検索 = Web

## chrome-devtools

Chrome DevTools 経由でブラウザ操作 + パフォーマンス計測。Playwright が「動作確認」なら chrome-devtools は「性能計測」。

- **典型シーン**: Salamat WBS / Lecture Hub のパフォーマンスチューニング (LCP 2.5秒以下クリアの維持)
- **計測**: LCP / TTFB / CLS / INP、console error、network request 詳細
- **Node 要件**: v22+ 必須 (YD: v24.15 で動作)
- **Playwright との同時利用注意**: 同じブラウザターゲットに両方接続すると競合の可能性。perf 計測専用に切り替える

## GitHub (gh-mcp)

`gh` CLI 拡張版。`gh auth status` の認証を流用するので PAT 別途管理不要。

- **典型シーン**: ai-researcher / vidkit / morning-briefing の Private repo の Issue 起票・PR 作成を自然言語で
- **本体**: `shuymn/gh-mcp` v3.0.3 → bundled `github-mcp-server` v1.0.5
- **Docker 版を選ばなかった理由**: Mac で Docker 動かすコスト > gh CLI 拡張のメリット
- **Bash の `gh` 直叩きとの棲み分け**: 曖昧な自然言語指示は MCP、明確なコマンドは Bash 直叩きが速い

## Supabase

`@supabase/mcp-server-supabase` のローカル版。**Read-only モード**、`lecture-hub` プロジェクトに固定。

- **project_ref**: `lkrmziwygyyyijyabtzp` (lecture-hub)
- **モード**: `--read-only` (DB 変更不可)。書き込みが必要になったら再登録 (`claude mcp remove supabase` → 再 `add` で `--read-only` を外す)
- **典型シーン**: Lecture Hub の `documents` / `tags` テーブルのスキーマ確認、サンプル SELECT、pgvector の状態確認
- **環境変数**: `SUPABASE_ACCESS_TOKEN`
- **プロジェクト切り替え**: 別プロジェクトを参照したい場合は再登録が必要 (現状 lecture-hub 専用)

## Stripe

`@stripe/mcp`、**Test mode**、`--tools=all` で全機能。

- **API key**: `rk_test_...` (test mode)
- **典型シーン**: 将来の課金実装 (Salamat Task Hub の商用化、Arte Grow の決済テスト) で動作確認
- **本番モード移行**: 実運用前に test key → live key に切り替え。本番モードの誤操作を避けるため、当面は test key 固定が安全
- **YD 現状の用途**: まだ active な決済プロジェクトなし、将来のための準備

---

## ✅ うまく行ったこと

- **user scope で 7個揃えたら、cwd 関係なく `claude mcp list` で全部見える** — project scope だと cwd 依存で見落とすので、複数プロジェクト並行運用では user scope が正解
- **gh-mcp は認証を流用**するので、PAT 発行・保管・rotate のコストゼロ
- **Context7 / Serena は認証なしでも動く** → 試しやすい
- **Supabase の `--read-only` 固定**で誤操作リスクを排除しつつ、スキーマ確認や SELECT は自由にできる

## ❌ 詰まったこと

- **動画の `.mcp.json` (project scope) 方式は YD には不向き** → user scope に統一
- **Notion / Figma を二重に入れない判断**: claude.ai 経由のクラウド版が既にあるので、ローカル版を入れるとツール名衝突 + メンテナンス2重 (動画スピーカーは全部入れていたが、YD 環境では除外)
- **Playwright が最初 project scope に入っていた** → 他プロジェクト cwd で見えなかった → user scope に移行
- **Context7 を API key なしで一旦登録 → key 取得後に再登録** で再起動コストあり (最初から key 揃えてから登録するほうが手戻りなし)

## 📋 次回同じことをするときのチェックリスト

### 新規 MCP サーバー追加時

1. **認証要否を最初に分類** (不要なものから先行で動作確認)
2. **user scope を default**、project scope は本当に必要な時だけ (`claude mcp add --scope user <name> -- <command>`)
3. **claude.ai 側のコネクタと重複しないか確認** — Notion / Figma / Google系は claude.ai 側で十分
4. **`claude mcp list` で `✓ Connected` を全部見届ける**
5. **`~/.claude.json` の `mcpServers` 直編集はしない** — 必ず `claude mcp add` 経由
6. **Vault 反映**: [[available_capabilities]] の表に追加 / 本ファイルにセクション追加 / `log.md` に1行
7. **memory 更新**: `~/.claude/projects/-Users-ittou/memory/mcp_playwright.md` (slug: mcp-local-servers) のテーブルに行追加

### MCP を使う前に毎回確認

1. **Web 系操作** → Playwright (DOM-aware で速い) または chrome-devtools (perf 計測)
2. **コードベース探索** → Serena (semantic) または Grep (文字列)
3. **ライブラリ仕様** → Context7 (`use context7` プロンプト or 直接 tool 呼び)
4. **GitHub 操作** → gh-mcp (自然言語) または Bash `gh` (明確なコマンド)
5. **Supabase 書き込みが必要になったら**: `--read-only` を外して再登録、作業終わったら戻す
6. **Stripe**: live key に絶対に勝手に切り替えない、test mode 固定

### 衝突回避

- **Playwright + chrome-devtools + computer-use** を同じブラウザターゲットに同時接続しない
- **Native desktop app** は Playwright ではなく computer-use
- **Notion / Figma のローカル版を入れない** (claude.ai 版で代替)

---

## 関連

- [[available_capabilities]] — 機能カタログ (本ファイルへの主リンク元)
- [[playwright_mcp]] — Playwright の詳細運用 (Web 自動化の中核)
- [[2026-05-23_MCP_9個導入]] — 本ノート誕生の意思決定
- [[claude_code_permissions]] — `~/.claude/settings.local.json` の運用
- [[claude_code]] — Claude Code 本体
- 動画素材: `~/Downloads/vidkit_2026-05-23_tutorial_7fxguUgpIPs/` (Shin Coding Tutorial、39分動画)
