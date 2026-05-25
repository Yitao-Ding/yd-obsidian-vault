---
type: decision
date: 2026-05-25
project: salamat_wbs
status: 完了
related:
  - "[[active_projects]]"
  - "[[2026-05-19_Salamat_WBS_Phase1実装]]"
  - "[[2026-05-19_Salamat_WBS_Phase2_演出強化]]"
---

# 2026-05-25 Salamat WBS 独自ドメイン化 (toyo-salamat.com)

## 背景

Salamat WBS の Vercel 本番デプロイ済 (`salamat-website-v2.vercel.app`) を独自ドメインに切り替え。`active_projects.md` の Phase 3 持ち越しタスク「独自ドメイン取得 → Vercel に紐付け」を消化。

## 決定事項

| 項目 | 内容 |
|------|------|
| ドメイン | **`toyo-salamat.com`** |
| Apex メイン / www リダイレクト | Apex を主、`www` は 308 で Apex へ |
| レジストラ | Wix (DNS も Wix で運用継続、NS 移管せず) |
| メール | Google Workspace (MX `aspmx.l.google.com` 系、無変更) |
| 旧 Firebase サイト | **そのまま放置** (`salamat-toyo.web.app` も稼働継続、リダイレクト設定なし) |

## DNS 設定 (Wix)

| Type | Host | Value | 備考 |
|------|------|-------|------|
| A | `@` (toyo-salamat.com) | `76.76.21.21` | Wix 初期の 185.230.63.x 3 件を 1 件に集約 |
| CNAME | `www` | `cname.vercel-dns.com` | Wix デフォルトの `www.wixdns.net` から書換 |
| MX | (Google Workspace) | aspmx.l.google.com 系 | **無変更** |

## Vercel 設定

- プロジェクト: `salamat-website-v2` (scope: `yitao-dings-projects`)
- 両ドメイン (Apex + www) を `vercel domains add` で追加
- SSL 証明書: **自動発行がスタックしたため `vercel certs issue` で手動発行** (Let's Encrypt 11 秒で発行完了)
- www → Apex の 308 リダイレクト: **Vercel REST API** (`PATCH /v10/projects/:id/domains/:domain` で `redirect: "toyo-salamat.com"` + `redirectStatusCode: 308`) で設定。CLI には直接コマンドなし

### 認証

Vercel REST API 認証は `~/Library/Application Support/com.vercel.cli/auth.json` の `.token` フィールドを Bearer Token で利用 (CLI ログイン状態を流用、別途 PAT 不要)。

## 動作確認

```
https://toyo-salamat.com         → HTTP/2 200 (Next.js prerender)
https://www.toyo-salamat.com     → HTTP/2 308 → https://toyo-salamat.com/
https://salamat-toyo.web.app     → HTTP/2 200 (旧 Firebase、放置)
MX                                → aspmx.l.google.com (Gmail 健在)
```

---

## ✅ うまく行ったこと

- **MX レコードを触らず Gmail を守れた** — 事前に `dig MX` で Google Workspace 指定を確認、Wix DNS 編集中も MX は対象外にする旨を YD に明示。メール影響ゼロ
- **Wix の DNS 編集 UI が分かりやすかった** — A レコード編集と CNAME 編集が同じ画面、保存も即時反映、TTL 1時間でも Google DNS への伝播は数分以内
- **Vercel CLI auth token を直接読んで API を叩けた** — `~/Library/Application Support/com.vercel.cli/auth.json` から token を `jq -r '.token'` で取得 → REST API で `redirect` 設定を一発で完了。Vercel CLI に該当コマンドがない隙間を埋められた
- **NS 移管せず Wix DNS のままにした判断** — Google Workspace の MX も含めて Wix で一元管理されてるので、無理に Vercel 側の NS に統一する必要がなかった。リスク最小化

## ❌ 詰まったこと

- **Wix で www に A レコード追加しようとしたら CNAME 衝突エラー** — Wix デフォルトで `www` が `www.wixdns.net` 系の CNAME に向いており、A レコードを追加できない。一度キャンセル → CNAME セクションを編集して `cname.vercel-dns.com` に書換。Vercel CLI が「A レコード推奨」と提示してきた指示にそのまま従うと詰まる
- **Wix の Apex に A レコードが 3 件 (185.230.63.107/186/171)** — Wix 初期のラウンドロビン設定。1 件編集 + 2 件削除で集約。3 件全部に同じ IP 入れる手もあるが、冗長になるので避けた
- **Vercel SSL 自動発行がスタック (15 分待っても出ず)** — DNS は伝播済 (`dig` で確認)、HTTP 200 は返ってる、`vercel domains inspect` も Edge Network: yes、なのに `vercel certs ls` は空。原因不明だが `vercel certs issue toyo-salamat.com www.toyo-salamat.com` で手動発行したら 11 秒で完了。CLI v50 が古い (v54 が最新) ことも一因かも
- **Vercel CLI に「ドメインのリダイレクト設定」コマンドがない** — `vercel alias` は別物、`vercel.json` の redirects は host ベースで書けるがビルド再デプロイが必要、結局 REST API 直叩きが最短

## 📋 次回同じことをするときのチェックリスト

ゼロから Wix ドメインを Vercel に紐付ける場合:

1. **事前確認 (DNS 触る前に必ず)**:
   - `dig MX <domain> +short` で現在の MX を確認、メール運用先を把握
   - `dig A <domain> +short` / `dig CNAME www.<domain> +short` で現状把握
   - `dig NS <domain> +short` でレジストラの NS を確認
2. **Vercel 側にドメイン追加**:
   - `vercel domains add <domain> <project> --scope <scope>`
   - `vercel domains add www.<domain> <project> --scope <scope>`
3. **Wix DNS 編集**: ドメイン > 高度な設定 > DNSレコードを編集
   - **A レコード (Apex)**: 既存の 3 件 (Wix デフォルト 185.230.63.x) を 1 件 `76.76.21.21` に集約
   - **CNAME (www)**: 既存の `www.wixdns.net` 系を `cname.vercel-dns.com` に書換 (A レコードで追加しようとすると詰まる)
   - **MX**: 絶対触らない (Google Workspace 利用なら Gmail が死ぬ)
4. **DNS 伝播確認**: `dig A <domain> +short @8.8.8.8` で `76.76.21.21` が返るか
5. **SSL 発行**: Vercel が自動発行するはずだが、10-15 分待っても `vercel certs ls` が空なら `vercel certs issue <apex> <www> --scope <scope>` で手動発行
6. **www → Apex の 308 リダイレクト** (Vercel REST API、CLI には無い):
   ```bash
   TOKEN=$(jq -r '.token' ~/Library/Application\ Support/com.vercel.cli/auth.json)
   curl -X PATCH "https://api.vercel.com/v10/projects/<project>/domains/www.<domain>?slug=<scope>" \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"redirect":"<domain>","redirectStatusCode":308}'
   ```
7. **最終動作確認**:
   - `curl -sI https://<domain>` → 200
   - `curl -sI https://www.<domain>` → 308 + Location ヘッダ
   - `curl -sI https://<domain>` の `server: Vercel` 確認
   - `dig MX <domain>` で Google Workspace 健在確認

落とし穴:
- **Wix の www は最初から CNAME**: 「A レコードで追加」の指示に従うと詰まる、CNAME 編集ルートで進める
- **Vercel CLI v50 以下は SSL 自動発行が不安定** な可能性: 出ないときは `vercel certs issue` で手動 (即発行される)
- **MX は絶対触らない**: Google Workspace 利用時の事故防止
- **NS 移管しない選択肢**: メール (MX) を含めて DNS を Wix で一元管理してるなら、無理に Vercel NS に統一しない方がリスク低い

## 関連

- [[active_projects]] (Salamat WBS Phase 3)
- [[2026-05-19_Salamat_WBS_Phase1実装]]
- [[2026-05-19_Salamat_WBS_Phase2_演出強化]]
- [[claude_mistakes]] A-13 / A-14 (本作業で追加)
