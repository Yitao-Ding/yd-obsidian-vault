---
type: knowledge
domain: programming/tools
last_updated: 2026-05-23
status: active
---

# Playwright MCP — ローカル MCP サーバー

> Microsoft 公式の `@playwright/mcp`。Claude Code から `mcp__playwright__*` ツール経由で
> Chromium / WebKit / Firefox を駆動できる。
>
> 導入日: 2026-05-23 (YDがインストール)
> 関連: [[available_capabilities]] / [[claude_code]]

---

## 概要

| 項目 | 内容 |
|------|------|
| **パッケージ** | `@playwright/mcp` (npx で起動) |
| **提供元** | Microsoft (Playwright チーム公式) |
| **位置づけ** | ローカル MCP サーバー (claude.ai のリモートコネクタとは別系統) |
| **呼び出し名** | `mcp__playwright` (Claude Code のツール namespace) |
| **許可設定** | `~/.claude/settings.local.json` の `permissions.allow` に `mcp__playwright` を追加済 (2026-05-23) → 毎回の許可プロンプトなし |

## ✅ うまく行ったこと

- **`@playwright/mcp` は npx で直接起動可能** — `npx -y @playwright/mcp --help` で疎通確認OK
- **`mcp__<server>` を allow 配列に書くだけで全 tool が許可される** — `mcp__playwright__browser_navigate` `mcp__playwright__browser_click` などを個別に許可する必要なし
- **DOM-aware なので computer-use より早く・正確** — Web 操作なら座標ピクセルではなく CSS セレクタ / ARIA role でターゲット指定
- **Vercel デプロイ済の Salamat WBS / Lecture Hub の動作確認に最適** — curl で HTTP コード見るだけより、レンダリング後の DOM を見られるので「画面が真っ白」「ボタンが押せない」系のバグを発見できる

## ❌ 詰まったこと

- **`claude mcp list` には出てこない可能性あり** — ユーザースコープ (`~/.claude/mcp.json` 相当) ではなく、Claude Code Desktop / IDE 側の設定で入れた場合、CLI 側からは見えないことがある。YD インストール経路は要確認 (`claude mcp list -s user` / `-s local` / `-s project` のどれか)
- **`permissions.allow` の表記** — Claude Code は MCP ツールを `mcp__<server>__<tool>` 形式で expose する。`mcp__playwright` のように server レベルで指定すると全 tool が許可される (推奨)、`mcp__playwright__browser_navigate` のように tool レベルで指定すると個別許可
- **`computer-use` との衝突に注意** — 同じ「画面操作」系。Web は Playwright、ネイティブアプリは computer-use と棲み分ける。両方使うと意図しない介入が起きる
- **ブラウザを開いたままにすると state 持ち越し** — セッション間で Cookie / localStorage が残る場合あり。動作確認後は close する習慣を

## 📋 次回同じことをするときのチェックリスト

### 新規にローカル MCP サーバーを追加するとき

1. **インストール経路を確定する**
   - npx で直接起動 (`npx -y <package>`) → ユーザースコープに自動追加されない
   - `claude mcp add <name> -s user -- <command>` → ユーザースコープに永続化
   - Claude Code Desktop の Settings UI → アプリ側の管理

2. **疎通確認**
   - `claude mcp list` で表示される? (スコープに依存)
   - `npx -y <package> --help` で実体が動くか確認

3. **許可設定** — `~/.claude/settings.local.json` の `permissions.allow` に追加
   ```json
   {
     "permissions": {
       "allow": ["mcp__<server_name>"]
     }
   }
   ```
   server レベル指定で全 tool 許可。個別 tool は `mcp__<server>__<tool>` で書く。

4. **Vault 反映**
   - `current_state/available_capabilities.md` の「ローカル MCP サーバー」表に追加
   - `knowledge/programming/tools/<server_name>_mcp.md` に詳細
   - `log.md` に1行追記

5. **memory にも reference type で保存** — 「常に存在を覚えておく」必要があるなら必須

### Playwright MCP を使うとき

1. **対象が Web か?** — ネイティブアプリなら computer-use を選ぶ
2. **既存のブラウザセッションが残っていないか?** — Cookie/localStorage が残ると本番チェックが歪む
3. **本番ドメインを操作する前に確認** — 特に Vercel ダッシュボード等、誤クリックで設定変更が起きるサイトは事前確認
4. **金融操作は禁止** — クレジットカード入力、送金、取引などは Playwright でも computer-use と同じく user に委譲する
5. **スクショは `~/Downloads/` または Vault `raw/` に保存** — Vercel deploy ログとセットで残すなど

---

## 🛠 typical な呼び出しパターン

(参考、実装時に確認)

```
mcp__playwright__browser_navigate(url="https://salamat-website-v2.vercel.app")
mcp__playwright__browser_take_screenshot(path="~/Downloads/wbs_check.png")
mcp__playwright__browser_click(selector="button:has-text('Activities')")
mcp__playwright__browser_evaluate(expression="document.title")
```

## 📚 関連

- [[available_capabilities]] — 全機能カタログ (Playwright を「ローカル MCP サーバー」セクションに登録済)
- [[claude_code]] — Claude Code 本体の運用
- [[claude_code_permissions]] — settings.json / settings.local.json の運用ルール
- [[vercel]] — Vercel デプロイチェックでの併用想定
