---
type: decision
date: 2026-06-05
project: パソコン1台の学校 (CC-business)
tags: [deploy, domain, 特商法, stripe, resend, go-live, 情報商材]
---

# パソコン1台の学校: 本番化着手 — 独自ドメイン取得・特商法方針・Go-live 6ステップ確定

> 2026-06-05 の意思決定記録。テストモード本公開（[[2026-06-04_CC-business_独自サイト継続_セキュリティ硬化_テスト公開]]）の翌日、YD「順番に全部やっていく」で本番化に着手。正本=`~/projects/CC-business/HANDOVER.md` §11。

## 状況
セキュリティ硬化＋テスト公開（購入→決済→解放→restore マジックリンク を Playwright/curl で実機検証済）まで完了済み。残は「YD作業のみ」の4項目（①特商法 operator 実情報 ②Stripe 本番キー ③Resend 独自ドメイン検証＋RESEND_FROM ④本公開）。本セッションでこれを順番に着手開始。**コード変更はなし＝決定のみ確定**。

## 決めたこと
1. **独自ドメイン = `pc1school.com`**（Vercel 新規取得）
   - 候補 `pasokon1dai.com`（当初推奨）/ `pasokon1.com` / `pc1school.com` / `pasokondai.com` を whois で空き確認（いずれも "No match"）→ YD が `pc1school.com` を選択。
   - 取得後、**サイトURL・Resend送信元・特商法メールを同一ドメインに統一**。Vercel 管理 DNS にすることで接続・Resend 検証レコード投入を Claude 側で完結させやすくする。
2. **特商法（特定商取引法）の表示方針 = 本名表示 ＋ 住所/電話は「請求があった場合に遅滞なく開示します」**
   - 個人のネット販売は氏名表示が原則必須のため本名は出す。住所・電話は開示請求対応の定型でリスク回避（個人ネット販売の一般運用）。
   - `web/src/lib/brand.ts` の `operator`：representative=本名（**YD入力待ち**）、email=`support@pc1school.com`、address/phone=現状の「請求があった場合に遅滞なく開示します」を維持。`/legal/tokushoho` に反映。
3. **メール = `support@pc1school.com`**：`operator.email`（特商法）と Resend `RESEND_FROM` の両方に使用 → 実顧客に復元マジックリンクが届く状態にする（現状は本人 yitao0907@gmail.com のみ配信）。
4. **販売は独自サイト継続**（[[2026-06-04_CC-business_独自サイト継続_セキュリティ硬化_テスト公開]] の方針を踏襲）。

## Go-live 6ステップ（依存順）
1. [YD] `pc1school.com` を Vercel で購入（約$20/年）
2. [Claude] `brand.ts` operator 実値化（**本名が来れば購入を待たず即実行可**）
3. [Claude] ドメインを cc-business に接続＋DNS＋`NEXT_PUBLIC_SITE_URL` 更新（購入後）
4. [Claude/YD] Resend でドメイン検証（SPF/DKIM/DMARC を Vercel DNS へ）→ `RESEND_FROM="パソコン1台の学校 <support@pc1school.com>"` 投入
5. [YD→Claude] Stripe 本番有効化 → `sk_live_` を **チャットに貼らず** Vercel env（`STRIPE_SECRET_KEY` production）へ
6. [Claude] `NEXT_PUBLIC_NOINDEX` 削除＋`ENABLE_DEV_UNLOCK` 空＋本番キー確認 → `cd web && vercel --prod --yes --scope yitao-dings-projects` → 本番ドメインで購入→決済→解放→restore を再検証

## いまYD待ち（3つ・並行可）
- ① **pc1school.com 購入**（Vercel Domains）→ step3・4 解禁
- ② **Stripe 本番有効化 申請**（審査ラグ＝今すぐ。`sk_live_` はチャット禁止・env直接）→ step5
- ③ **本名**（→ step2 `brand.ts` 即反映）

## 注意
- `sk_live_` をチャット/Vault に貼らない（セッションログ `~/.claude/projects/` に残る）。env投入は `vercel env add STRIPE_SECRET_KEY production`（貼付非表示）か Vercel ダッシュボードで。
- LIVE: BG agent / Workflow なし。dev サーバー（CC-business/web）ローカル稼働中だが動作確認は Vercel URL で。

## 関連
- [[2026-06-04_CC-business_独自サイト継続_セキュリティ硬化_テスト公開]]
- [[2026-06-03_パソコン1台の学校_GTM計画確定]]
- [[2026-06-03_パソコン1台の学校_情報商材システム構築]]
- 正本: `~/projects/CC-business/HANDOVER.md` §11 / `SECURITY/README.md` / `marketing/launch/README.md`
