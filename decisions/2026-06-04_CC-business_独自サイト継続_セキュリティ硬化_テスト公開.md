---
type: decision
date: 2026-06-04
project: パソコン1台の学校 (CC-business)
tags: [platform, security, deploy, stripe, go-to-market, 情報商材]
---

# パソコン1台の学校: 独自サイト継続 → セキュリティ硬化 → テストモード本公開（全フロー検証済）

> 2026-06-04 の意思決定と実装の記録。正本=`~/projects/CC-business/HANDOVER.md` + `SECURITY/` + `marketing/`。

## 状況
GTM計画確定（[[2026-06-03_パソコン1台の学校_GTM計画確定]]）の翌日。Go-live を進める中で販売プラットフォームを再検討し、独自サイト継続＋セキュリティ硬化＋テスト公開（購入フロー実機検証）まで到達。

## 決めたこと
1. **販売プラットフォーム = 独自サイト継続**。note/Tips/Brain を比較した末（`marketing/PLATFORM-decision.md`）:
   - note は**情報商材を規約で禁止**（BAN+売上没収リスク）＝不可。
   - Tips（運営が特商法代行で本名非開示・14%）/ Brain（アフィリ拡散最強だが本名+住所開示必須）。
   - 結論: **特商法に本名を出すのがOKと判断** → 手数料最安(3.6%)・完全コントロール・先着100名の自動値上げがネイティブに効く**独自サイト**を採用。
2. **方針 = ship-first**: 「まず公開＋広告で一旦完成 → LINEファネル等は後で再考」。`FUNNEL-design.md`（SNS→LINE→ステップ配信→決済）は棚上げ（売り先が独自/Tipsでも上に乗る型）。
3. **セキュリティを固める**（課金ありのためYD指示）: project-agent の SECURITY宝箱 + IPA「安全なウェブサイトの作り方」+ IBM データ管理ガイドを学習 → CC-business版 `SECURITY/` ハブを構築・監査・硬化。
4. **テストモードで本公開し購入フローを実機検証**（本番Stripe前に）。

## 実装した主なこと
- **価格ロジック動的化（S-1）**: LP/料金/カードを `currentOffer()` 駆動に。表示=決済額一致・先着100名で自動定価切替・60秒キャッシュ。
- **セキュリティ（`SECURITY/` 6ファイル・監査 critical 0）**:
  - Phase A: JWT検証アルゴ HS256 固定 / `unlock` の `metadata.pc1==="main"` 検証 / セキュリティヘッダ5種(HSTS/nosniff/X-Frame-Options/Referrer/Permissions) / cookie属性統一
  - R-3: 依存なしレート制限（`lib/rate-limit.ts`、restore 5/10分・checkout 10/分）/ プライバシーにデータ最小化明記
  - 確定 high=1（**R-1 restore 本人確認なし**=メールだけで解放。v1はレート制限で許容・v2マジックリンク）
- **テスト本公開**: `cc-business.vercel.app`（noindex・Deployment Protection無効・Stripeテスト鍵）。**Playwright で 購入→決済(test card 4242)→/unlock→/learn 全6解放→レッスン本文配信まで end-to-end 検証成功**。

## ✅ うまく行ったこと
- 「DB無し・Stripeホスト決済・作者のみ静的教材」設計が IPA 9大脆弱性の大半を構造的に消していると裏取り（本丸は 1.9 認可=restore のみ）。
- 監査→反証検証→宝箱生成を Workflow で実施。背骨（ゲート・秘密・決済検証）が本物と `.next` 成果物まで確認して確証。
- Phase A 硬化後も全フローが壊れず、テスト購入が end-to-end で通った（metadata.pc1 検証含む）。

## ❌ 詰まったこと
- **Vercel デプロイで2回ハマった**: ①新規プロジェクトは framework 未設定で全ルート404（`framework:"nextjs"` をAPIで明示し解消）②Deployment Protection 既定ONで401（`ssoProtection:null` で無効化）。
- **Workflow の統合(synthesize)エージェントが socket エラーで複数回落ちた**（大きいファイルWrite＋構造化返却の兼務）。→ journal から findings を回収しメインが手で再構成。
- **自分のプロセスを並行CCと誤認して kill しかけた**（verify で回避、[[claude_mistakes]] A-17）。
- YD が一度「公開中止」→プラットフォーム再検討。oscillation を「本名公開の許容度」1軸で決着させ収束。

## 📋 次回同じことをするときのチェックリスト
- **Vercel 新規プロジェクトは作成直後に** `PATCH /v9/projects/<p>?slug=<scope>` で `{framework:"nextjs", ssoProtection:null}` → `vercel link` → `vercel env add`(×4) → `vercel --prod` の順。
- テスト公開=noindex(`NEXT_PUBLIC_NOINDEX=1`＋layout robots ゲート)＋Stripeテスト鍵。本番化=noindex env 削除＋`sk_live_`＋noindex外す。
- 課金サイトのセキュリティは「認可(エンタイトルメント/復元)」に集中。DB無しなら SQLi/XSS 等は構造的に小。restore は本人確認 or 最低レート制限。
- 大出力の Workflow synthesize は socket 落ちしうる→返却最小化・ファイルは個別Write・メインで手統合できる前提を持つ。

## 関連
- 正本=`~/projects/CC-business/HANDOVER.md` / `SECURITY/`(監査・宝箱) / `marketing/launch/`(GTM) / `marketing/PLATFORM-decision.md` / `marketing/FUNNEL-design.md`
- [[2026-06-03_パソコン1台の学校_GTM計画確定]] / [[claude_mistakes]] A-17
- **残（YD作業）**: 特商法 operator 実情報(本名+メール)→`brand.ts` / Stripe本番キー(`sk_live_`) / 本公開(noindex env削除) / R-1 マジックリンク(本番でメール基盤と)
