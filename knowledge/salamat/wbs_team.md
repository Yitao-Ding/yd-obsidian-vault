---
type: knowledge
domain: salamat
created: 2026-05-18
last_updated: 2026-05-18
---

# WBS チーム / Salamat 公式HPリニューアル

## 概要

東洋大学公認国際ボランティアサークル Salamat の公式サイトリニューアル案件。
現行サイト `salamat-toyo.web.app` (Firebase) を新スタックで作り直す。
チーム4人体制、YD が実装担当。

## チーム構成

| 役割 | 担当 |
|------|------|
| 実装 | YD |
| 内容 | Riko |
| 構成 | Haruka |
| デザイン | Rena |

## 技術スタック

- **Framework**: Next.js 16.2.6 (App Router, Turbopack)
- **UI**: React 19.2.4 + TypeScript + Tailwind CSS v4
- **フォント**: Sora + Zen Maru Gothic + Noto Sans JP (next/font/google でセルフホスト)
- **モーション**: 自前のscroll-driven SVG path (Framer Motion は入れていない)
- **デプロイ**: 未定 (Vercel 想定)
- **プロジェクトパス**: `~/Downloads/07_開発・アプリ制作/salamat-website-v2`

## デザインプロセス

Claude Design (claude.ai/design) でカンプを詰めてから、Claude Code に渡して
Next.js プロジェクトに移植する流れ。
カンプは HTML/CSS/JS prototype として export され、Claude Code 側で React Server
Component + Client Component に変換する。

### 経緯

1. **初版** (2026-05-18 早朝): JPEG カンプ4枚から Next.js プロジェクトを初期構築。
   Embla Carousel + 通常のセクション構成。
2. **Claude Design でカンプを詰める** (2026-05-18 午後): モーション・グラデーション・
   タイプライターなど演出要素を追加した HTML プロトタイプを生成。
3. **2版** (2026-05-18 夕方): プロトタイプを Next.js プロジェクトに上書き反映。
   既存セクションを全て書き換え。

## 実装済みセクション (2026-05-18 時点)

1. **Header** — sticky + backdrop-blur、HOME ピル
2. **Hero** — split / centered / fullbleed の3レイアウト切替 (split がデフォルト)。
   写真 + 縦書き stroke "Salamat" オーバーレイ
3. **Vision Value** — GradientText (虹色アニメ) + Typewriter ("Smile / Hope /
   Future / Tomorrow" がローテーション) + Liquid Glass CTA
4. **Action** — Philippines + Japan 国別ブロック。地図シルエットSVG + 写真背景 +
   濃青バンドのカード横スクロール
5. **Report** — レポートカードグリッド + **スクロール連動の飛行機アニメ** (Report →
   Story を貫く SVG パスを進行+回転、生の requestAnimationFrame + getPointAtLength)
6. **Story** — scroller / circular の2レイアウト切替。3:4 写真カードの横スクロール
7. **News** — ピル装飾 + 点線区切りリスト + 「お知らせ一覧」ボタン
8. **Footer** — 3カラム (Logo / Sitemap / Contact)、SNS (Instagram / X / YouTube)

### Tweaks Panel

右下フローティング。本番環境でも表示する仕様。localStorage 永続化。
切替可能項目:
- カラーパレット (Ocean / Sky / Indigo / Teal / Marine の5種)
- ダークモード
- Hero レイアウト
- Story レイアウト
- CTA ボタン (Liquid / Metal / Cool)
- 飛行機アニメ ON/OFF

## ステータス (2026-05-18 時点)

- `pnpm/npm run build` 通過 ✅
- TypeScript `tsc --noEmit` 通過 ✅
- dev server HTTP 200 確認済み (curl で .next 生成は確認、目視は未)
- Impact Stats セクションは新デザインに合わせて削除済み
- モーション/カーソル演出は土台のみ。本番演出は次フェーズ

## 残タスク

- [ ] Claude Design でさらにカンプを詰める (YD 側作業)
- [ ] About / Vision / Action 詳細ページなど下層ページ
- [ ] お問い合わせフォーム
- [ ] CMS 連携 (microCMS / Notion / Sanity いずれか)
- [ ] 多言語対応 (i18n)
- [ ] 本番モーション/カーソル演出の作り込み
- [ ] ドメイン取得 + Vercel デプロイ
- [ ] 実際の活動写真・人名画像への差し替え (現状一部プレースホルダー)
- [ ] Impact Stats を Trust 要素として復活させるか再検討

## 関連

- [[index]] - Salamat 領域ハブ
- `~/Downloads/07_開発・アプリ制作/salamat-website-v2` - プロジェクトパス
- 現行サイト: `salamat-toyo.web.app`

## 出典

- 2026-05-18 セッションでの YD ↔ Claude Code 実装ログ
- Claude Design handoff bundle (`~/.claude/projects/.../tool-results/webfetch-*.bin`)
