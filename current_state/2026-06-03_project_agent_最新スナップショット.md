---
type: current_state
created: 2026-06-03
author: Claude (チャットUI、Desktop Commander で project フォルダ実読込のうえ作成)
purpose: HANDOVER.md と実態のズレを次セッションが拾えるようにするスナップショット
sources_verified:
  - code/BLUE_FORWARD_NOTES.md (2026-06-03)
  - code/EVAL_AUTH.md (2026-06-02)
  - code/EVAL_DEVBUILD_AUTH.md (2026-06-02)
  - code/EVAL_QA.md (2026-06-03)
  - code/EVAL_DESIGN.md (2026-06-03)
  - current_state/active_projects.md (2026-06-02)
---

# Project Agent Application — 2026-06-03 最新スナップショット

> **作成経緯**: YD との会話中、`HANDOVER.md`(冒頭日付 2026-06-01) が実態より古いと判明。
> 以降の進捗(認証 Phase 12 完了 / BLUE-FORWARD リバランス / Apple 審査待ち)が HANDOVER に未反映で、
> 次の CC が古い前提で動くリスクがあるため、project フォルダの最新ファイルを実読込して本ノートに固定する。
> **正本は引き続き各 EVAL ファイル + tokens.ts。本ノートは「どこを見れば最新か」の索引。**

## ⚠️ HANDOVER.md との既知のズレ (次セッションは本ノートを優先)

| 項目 | HANDOVER.md の記述 | 2026-06-03 実態 |
|---|---|---|
| 認証 | 「実認証 #17 は未実装、Phase 12 で対応」 | ✅ **実認証 Phase 12 完了・独立検証 Pass** (`EVAL_AUTH.md` / `EVAL_DEVBUILD_AUTH.md`) |
| Notion | legal 文書を Notion 公開 URL で発行する旧手順あり | ❌ **Notion 不採用が確定済み**。Notion 前提の手順は無効 |
| デザイン | Voyager / getdesign 方針を YD 検討中で中断 | ✅ **3 色確定 + BLUE-FORWARD リバランス完了** (`BLUE_FORWARD_NOTES.md`) |
| 色 | パレット #21 未確定 | ✅ クリーム #FFF8DC / 青 #025291 / オレンジ #FF8B07 で確定、青主役 40% に再配分済 |

## ✅ 認証 (Phase 12 / BUG #17) — 完了・Pass

- **検証日 2026-06-02、独立検証エージェントが実測ゲートで Pass** (`code/EVAL_AUTH.md`)。
- **実配線済で点灯**: メール Magic Link (signInWithOtp + PKCE) / Apple Sign In (nonce hashed→Apple / raw→Supabase)。
- **枠だけ作って消灯 (comingSoon, enabled=false)**: Google (ネイティブ id_token、iOS ガード + lazy require) / LINE (Custom OIDC) / LinkedIn。
  → 点灯はコード数行 + **Supabase Dashboard と各 Console 設定 (コード範囲外)** 待ち。手順は `code/AUTH_PROVIDER_SETUP.md`。
- ゲート実測: tsc 0 / lint 0 (expo lint) / `expo export --platform web` 成功 / モック動線維持 / 招待 redirect 復帰維持。
- iOS 専用 dev build 整備済 (`eas.json` development プロファイル = simulator:true + developmentClient:true + distribution:internal、device 版も別途)。
- **follow-up (Pass を妨げない)**: 実 DB profiles 同期は未 (現状 profile は user.id 名前空間の secureStorage)。

## ✅ デザイン — BLUE-FORWARD リバランス完了 (2026-06-03)

- YD feedback「色はいい、でも白が多すぎ + 青が見えない + サブとアクセント同比率は微妙、もっと青青していい」→ 方針 A「青を主役に」。
- **3 色の hex は不変**、面積・位置・比率だけ青優位に再配分。目標比率 = 青 ≈40% / クリーム+白 ≈45% / オレンジ ≈10%。
- qa-evaluator (2026-06-03) Pass: tsc 0 / lint 0 / web export 成功 / 認証・ナビ・モック配線退行なし。
- design-evaluator (2026-06-03) Pass: blueRatio達成 / オレンジ差し色のみ / WCAG 0 fail (独立再計算) / slop 1.0 / appleParity 9.0 / NG 0。
- 実装の真実 = `code/src/lib/design/tokens.ts`、説明 = `code/BLUE_FORWARD_NOTES.md`、上位パレット決定 = `design/redesign-2026-06-02-Palette/palette-decision.md`。
- 共通部品 `src/components/BlueHeader.tsx` 新設 (青地 navbar を全画面に波及)。

## ✅ 実 DB (Supabase) — 整備済

- project `lkrmziwygyyyijyabtzp` (既存 Notion クローンを wipe→上書き、backup は `~/Documents/notion_clone_backup_lkrmziwygyyyijyabtzp_20260601.md`)。
- 26 テーブル全 RLS 有効 / schools 854 / storage 3 バケット。
- `client.ts` FORCE_MOCK は **env 化済・既定=実DB** (`EXPO_PUBLIC_FORCE_MOCK==='true'` のときだけ MOCK)。
  → ⚠️ **データありで触る/デモは `EXPO_PUBLIC_FORCE_MOCK=true` で起動** (実認証セッションがないと RLS で全テーブル 0 件 = 空画面になるため)。
- **DB 残作業**: Edge Functions 12 個を新 project へ再 deploy + Gemini secret 再設定 / storage object RLS ポリシー / WARN advisory ハードニング。

## 🚦 今の唯一の外部ボトルネック = Apple Developer Program enrollment

- **2026-06-03 時点: ステータスは Pending のまま変わっていない** (支払いはクレカ反映済、当日メール返信は来たがステータス未変化)。
- ★ これは YD 固有の問題ではなく、**2026 年に入ってからの Apple enrollment 審査の広範な機能不全** (個人 enrollment で支払い済なのに 12 日〜55 日 Pending、サポート無反応の報告が developer.apple.com / discussions.apple.com に多数)。
- **対処 (YD 側でできること)**: ①二重払いしない (重複課金は解消困難で活性化も早まらない) ②enrollment 時に VPN を使っていたら切る (失敗主因の一つ) ③developer.apple.com/account → Contact Us から催促だけ出す ④数日に一度ステータス確認。あとは待ち。
- 活性化したら即: EAS Build (production) → TestFlight Internal Testing (最大 100 名)。正式 App Store 審査を待たず先行配布可。

## 📦 配布前の残タスク (Apple 待ちの間に潰せるもの)

- [ ] legal 文書 (プライバシーポリシー / 利用規約) の最終化 — ★ **Notion 不採用なので発行先を別途決める** (アプリ内 LEGAL_LINKS は配線済 = login.tsx L595/L605)。
- [ ] App Store 用 説明文 + キーワード + スクショ 5 枚 + アイコン 1024。
- [ ] Google / LINE Console 設定 → 認証 3〜5 種目の点灯 (任意、MVP 必須ではない)。
- UI ブラッシュアップ継続中 (機能追加はしていない = リリース範囲を広げず完成度だけ上げる方針、Apple 待ち時間の使い方として妥当)。

## 🎯 需要サイド (会話で更新)

- YD が Salamat 外含め **口頭で大人数にヒアリング済**。特に **ダンサー界隈の繋がり**で反応良好、**「早速使いたい」チームが複数**到来。
- アプリの構造的特性 = **大人数で使ってこそ真価** (お宝箱 / 振り返り / 寄せ書きは 10〜20 人規模で効く)。→ 「機能を削って 2 機能 MVP」は不適、機能群が揃ってこそ最初のチームが価値を体験できる。
- beachhead 候補として「公演・イベントを打つクリエイティブチーム (ダンス等)」が学生団体一般より刺さる可能性 (要検証、YD 自身が当該界隈のインサイト保持)。

## 🔁 運用メモ (会話で判明)

- YD の現運用は「**新しい pj フォルダを自分で作り、そこで CC を立てて指示出し**」が主。中心が `~/ObsidianVault/` から各 pj フォルダ (project-agent-application 等) に移動。
- そのため **userPreferences の「朝の挨拶トリガー → Vault 読込」フローは形骸化している** (YD 談「おはようコマンドマジで使ってない」)。起動フローを現運用に合わせて作り直す提案が保留中。
- 現在 CC を 5 ペルソナ自律で走らせ中 (UI ブラッシュアップ系)。1 セッション 200〜400 万トークン規模の運用。
