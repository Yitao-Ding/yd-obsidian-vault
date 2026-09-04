---
type: knowledge
domain: programming/tools
last_updated: 2026-05-20
status: active
related:
  - [[claude_code]]
  - [[lecture_hub]]
  - [[task_hub]]
---

# Vercel CLI 運用知見

> YDのプロジェクトのうち Vercel ホストは Lecture Hub / Salamat WBSサイト。Task Hub は Firebase Hosting。
> このノートは Vercel CLI のハマりやすい挙動と、初回デプロイのフローをまとめる
> 初回作成: 2026-05-18 (Salamat WBSサイト初回デプロイで得た知見)

---

## 🚀 基本コマンド早見表

| コマンド | 用途 | 備考 |
|---------|------|------|
| `vercel` | デプロイ (preview がデフォルト) | **初回実行はProduction扱いになる** ★ |
| `vercel --yes` | デフォルト承認で全自動 | 新規プロジェクト作成時に便利 |
| `vercel --prod` | 明示的に本番デプロイ | 既存プロジェクトの本番昇格 |
| `vercel deploy` | preview デプロイ | 既存プロジェクトの場合のみ確実に preview |
| `vercel link` | 既存プロジェクトに紐付け | `.vercel/project.json` 生成 |
| `vercel env pull` | env vars を `.env.local` に取得 | Lecture Hub の Blob 統合等で必須 |
| `vercel whoami` | ログインユーザー確認 | YD は `yitao-ding` |
| `vercel inspect <url>` | デプロイ詳細確認 | ビルドログ等 |

## 🔐 ログイン状態

- スコープ名: **`yitao-ding`** (個人)
- プロジェクトURL形式: `<name>-<hash>-yitao-dings-projects.vercel.app` (個別)
- エイリアスURL形式: `<name>.vercel.app` (本番のみ)

## 📂 初回デプロイの典型フロー (新規プロジェクト)

```bash
cd <project-dir>
vercel --yes      # 質問スキップでProduction自動作成
# ↓ 出力例
# Production: https://<name>-<hash>-yitao-dings-projects.vercel.app
# Aliased:    https://<name>.vercel.app
```

これで:
- `.vercel/project.json` が生成され、以降のデプロイは自動的に紐付け
- メインのエイリアスURL (`<name>.vercel.app`) が**公開アクセス可** (200)
- 個別デプロイURLは **Deployment Protection で401** (要Vercel認証)

## ⚠️ ハマりポイント

### 1. `vercel` 初回実行は Production になる

**事象**: Salamat WBSサイトで「Preview」のつもりで `vercel --yes` を叩いたら Production にデプロイされた。

**原因**: Vercel CLI は新規プロジェクト初回実行時、`target: production` で作成する仕様。`vercel deploy` でも初回は同じ挙動。

**回避**: 完全に Preview だけ作りたい場合は、先に Vercel ダッシュボードでプロジェクトを作っておくか、`vercel link` で紐付けしてから `vercel deploy` する。ただし「とにかく公開URLが欲しい」用途なら初回 `vercel --yes` で十分。

### 2. 個別デプロイURLは Deployment Protection で 401

**事象**: `<name>-<hash>-yitao-dings-projects.vercel.app` を共有しても、相手はVercelログインが要求される。

**原因**: Vercel の Deployment Protection (旧 Password Protection) がデフォルトで個別URLに有効。エイリアスURL (`<name>.vercel.app`) は例外で公開。

**対応**:
- チーム共有用には **必ずエイリアスURLを使う** (`https://<name>.vercel.app`)
- 個別URLを公開したい場合はダッシュボード `Settings > Deployment Protection` で OFF にする (要注意: 全デプロイが公開される)

### 3. Next.js 16 + Turbopack の build キャッシュ

**事象**: `next build` が `Turbopack: ✓ Compiled successfully in 3.8s` で爆速になる。

**正しい挙動**: 速いのが普通。逆に遅い場合 (`.next` 残骸など) は `.next/` を消すと改善する場合あり。

### 4. `package-lock.json` (npm) を使うと Turbopack が警告

**事象**: Turbopack が「workspace rootが特定できない」警告を出す。

**回避**: `next.config.ts` の `turbopack.root` で固定する (Salamat WBSサイトは対応済み)。

```ts
import path from "node:path";
const nextConfig: NextConfig = {
  turbopack: { root: path.resolve(".") },
};
```

## 🌐 公開URL運用パターン

| URL種別 | 公開アクセス | 用途 |
|--------|------------|------|
| `<name>.vercel.app` (本番エイリアス) | ✅ 200 | チーム共有・公開・SNS掲載 |
| `<name>-<hash>-yitao-dings-projects.vercel.app` (個別) | ❌ 401 | 自分の確認・差分比較 |
| 独自ドメイン (例: `salamat-toyo.com`) | ✅ 200 (DNS設定後) | 最終的な公開URL |

## ⚠️ Next.js / next/font の落とし穴 (2026-08-28 ハタチたちHP実機検証で発見)

### next/font の CSS 変数は `<html>` に渡さないと全ページで無効になる

`next/font` で定義したフォント変数 (`variable: '--ff-mincho'` など) は、`layout.tsx` の `<html>` タグに `className={mincho.variable}` を渡さないと CSS に展開されない。`:root` 変数として書いても解決できない。

```tsx
// ❌ ダメ: layout.tsx の <body> だけに渡す
<body className={mincho.variable}>

// ✅ 正しい: <html> に渡す
<html lang="ja" className={`${mincho.variable} ${sans.variable}`}>
```

症状: `getComputedStyle(document.documentElement).getPropertyValue('--ff-mincho')` が空文字列になる。日本語フォントのpreloadリクエストが全ページ分積み上がる (今回は123本 → 修正後1本)。

---

## 📋 YDのプロジェクト別 Vercel 状況

| プロジェクト | スコープ | 本番URL | 状態 |
|------------|---------|---------|------|
| Lecture Hub | yitao-dings-projects | `lecture-hub-sable.vercel.app` | TipTap v3 移行 + 本番デプロイ完了 (2026-05-19) |
| Task Hub | - (Vercel 管理外) | `salamat-task-hub.web.app` | Firebase Hosting 確定、Vercel 管理外 |
| Salamat WBSサイト | yitao-dings-projects | `salamat-website-v2.vercel.app` | Phase 1+2 演出強化完成、本番デプロイ済み (2026-05-19 夜) |

## 🔧 環境変数管理

```bash
vercel env pull             # 全環境を .env.local に取得
vercel env pull .env.development --environment=development
vercel env add KEY          # 対話的に追加
vercel env ls               # 一覧
```

Lecture Hub 等のプロジェクトでは `ANTHROPIC_API_KEY` / `GOOGLE_GENERATIVE_AI_API_KEY` / `OPENAI_API_KEY` / `BLOB_READ_WRITE_TOKEN` / `CRON_SECRET` を Vercel 側で設定 → `vercel env pull` でローカル取得が定型。

## ✅ うまく行ったこと

- `vercel --yes` で「git連携なし・対話なし」の最短デプロイが成立。salamat-website-v2 では未コミット変更がある状態でもデプロイ成功 (CLIはローカルファイルをそのままアップ)。
- Next.js 16 + Turbopack + React 19 + Tailwind v4 構成が Vercel側で**ゼロ設定で認識**された (Build Completed in 33s, onBuildComplete from Vercel)。
- エイリアスURL (`<name>.vercel.app`) が個別URLの 401 問題を回避するベストプラクティスだと確認できた。
- `vercel whoami` で事前にログイン状態を確認しておけば、デプロイ中に詰まらない。

## ❌ 詰まったこと

- **「Preview デプロイ」と思って実行したら Production になった** — Vercel CLI は新規プロジェクト初回実行時のみ `target: production` 固定。YD には事後説明で対応したが、知っていれば事前にダッシュボードでプロジェクト作成→`vercel link`→`vercel deploy` の順で完全Previewに分けられた。
- **個別デプロイURLを共有しようとして401を踏みかけた** — Vercel Deployment Protection のデフォルトON仕様を忘れがち。チーム共有時は必ずエイリアスURLを使う。
- **Turbopack の workspace root 警告** — `~/package-lock.json` (ホーム直下のゴミ) が原因で root 推測が誤る。`next.config.ts` で `turbopack.root` を pin して解決済み (salamat-website-v2)。

## 📋 次回同じことをするときのチェックリスト

新規プロジェクトを Vercel に初回デプロイするときの手順:

1. **事前確認**
   - [ ] `vercel whoami` でログイン確認 (期待: `yitao-ding`)
   - [ ] `npx next build` (or `pnpm build`) でローカルビルド通過確認
   - [ ] `.gitignore` に `.vercel/` が入っていること (project.json は秘匿対象)
   - [ ] env vars が必要なら **先に Vercel ダッシュボードで設定** してから `vercel env pull`

2. **デプロイ実行**
   - [ ] 「とにかく公開URL欲しい」ケース → `vercel --yes` (Production即時)
   - [ ] 「Previewだけにしたい」ケース → ダッシュボードで先にプロジェクト作成 → `vercel link --yes` → `vercel deploy`
   - [ ] CLIの出力から **Aliased: https://<name>.vercel.app** を控える (これがチーム共有用URL)

3. **デプロイ後確認**
   - [ ] `curl -s -o /dev/null -w "%{http_code}" https://<name>.vercel.app/` で 200 確認
   - [ ] エイリアスURLをブラウザで開いて、表示崩れ・font未ロード等を目視確認
   - [ ] チーム共有時は**個別URLではなくエイリアスURL**を渡す

4. **記録**
   - [ ] `current_state/active_projects.md` のプロジェクト状況を更新 (公開URL明記)
   - [ ] 必要なら `decisions/YYYY-MM-DD_*.md` に意思決定 (本番扱いになった理由など)
   - [ ] `log.md` に1行追記

5. **避けるべきこと**
   - ❌ 個別デプロイURL (`<name>-<hash>-yitao-dings-projects.vercel.app`) を共有する → 401 で相手が困る
   - ❌ `vercel --prod` を新規プロジェクトの初回に打つ → どうせ Production になるので不要だが、混乱を生む
   - ❌ ローカルで `next build` を試さずに `vercel` を叩く → Vercel側でビルドエラー = デプロイ枠の浪費

## 📚 関連

- [[claude_code]] — Claude Code から Vercel CLI を呼ぶ際の注意
- [[lecture_hub]] — Lecture Hub の Vercel デプロイ事例
- `current_state/active_projects.md` — Vercel 上にあるプロジェクト一覧

---

## 独自ドメイン接続: CLI の値を信じない (2026-09-05 追記、hatachitachi.com で判明)

`vercel domains inspect <domain>` は apex にも www にも `A 76.76.21.21` を勧めてくる。**これは legacy 値**。
プロジェクト固有の正しい値はダッシュボードの Project Settings → Domains → 対象ドメインの
「View DNS configuration」カードにしか出ない。hatachitachi.com の実例:

| | CLI が言う値 | カードの実値 |
|---|---|---|
| apex | A 76.76.21.21 | **A 216.198.79.1** |
| www | A 76.76.21.21 | **CNAME 05944606fa304010.vercel-dns-017.com** |

カード下の注記: "We're expanding our IP range. We recommend the records above. The legacy records
cname.vercel-dns.com and 76.76.21.21 will continue to work."

Vercel 公式も「domain card が single source of truth。検証はプロジェクトが期待する厳密な値に対して
行われるので、他所で見た IP を貼ると Invalid Configuration のままになる」と書いている
(https://vercel.com/kb/guide/a-record-and-caa-with-vercel)。新しいプロジェクトほど anycast プールから
別の IP が割り当たるので、過去のプロジェクトの値を使い回さない。

**AAAA は入れない。** Vercel はサードパーティ DNS 経由のカスタムドメインで IPv6 非対応。AAAA があると
トラフィックが分裂して SSL 発行が止まる。

**www → apex のリダイレクトは CLI では設定できない。** `vercel domains` にサブコマンドもフラグも無い。
ダッシュボードの Edit →「Redirect to Another Domain」→ 308、または REST API
`PATCH /v9/projects/{id}/domains/{domain}` の `redirect` / `redirectStatusCode`。

**Domain Connect (レジストラ側 DNS の自動書き込み) は Cloudflare 管理ドメインのみ。** 2025-02 の告知から
「more providers coming soon」のまま。お名前.com は対象外なので手入力しかない。

**transfer-in は ICANN の60日ロックがある。** 登録直後のドメインは Vercel に移管できない。

お名前.com 側の手順と落とし穴は [[onamae_ドメイン未反映_2026-09-03]] と
`~/AI projects/hatachi-tachi-website/docs/domain-setup.md`。

### apex の SSL 証明書が自動発行されない (2回発生、どちらも CLI 50.44.0)

- 2026-05-25 toyo-salamat.com: 15分待っても `vercel certs ls` が空 → `vercel certs issue` で11秒
- 2026-09-05 hatachitachi.com: www の証明書だけ発行され apex が無い → `vercel certs issue hatachitachi.com` で12秒

DNS が正しく引けて HTTP も通っているのに HTTPS だけ `no alternative certificate subject name matches`
で落ちる場合はこれ。`vercel certs ls` で apex の行が無いことを確認して、待たずに手動発行してよい。
2回とも CLI 50.44.0 なので、CLI を上げると直る可能性がある (2026-09-05 時点の最新は 59.11.2)。
