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

## ✅ うまく行ったこと

- **Claude Design → Claude Code のリレー**が想定以上にスムーズ。HTML/CSS/JS prototype を
  そのまま Next.js + Tailwind v4 + Server/Client component に再構築するパターンが確立
- **`tar -xzf` で handoff bundle 展開** → README + chat transcript + 全ソースを順に読む方式が
  低オーバーヘッド。「ブラウザでレンダリング禁止」の README 指示通り、HTML/CSS を直読みで充分
- **scroll-driven 飛行機アニメ**を Framer Motion なしで実装できた (生の rAF + SVG
  `getPointAtLength` + `viewBox` スケーリング)。依存最小化に成功
- **Tweaks Panel + localStorage** で、デザイン詰めをコード再ビルドなしで回せる仕組みに
- **AskUserQuestion で先に方針確認** (上書きvs全書き直し、Tweaks有無、Impact Stats削除、
  飛行機今期実装か) → 後戻りゼロで2時間程度で全セクション完了
- **CSS変数 + Tailwind v4 `@theme inline`** の併用で、Tweaks のランタイム書き換えと
  Tailwind ユーティリティの両方が同居できた

## ❌ 詰まったこと

- **lucide-react v1.16.0 が Instagram/Facebook/X アイコンを削除している** ことを初版で
  踏み抜いた → 今回はインライン SVG に切替済み
- **Next.js 16 の next/font 型定義で Sora の `style: ["normal", "italic"]` が拒否**
  された (italic は normal の variant 扱い) → `style` プロパティ自体を外して解決
- **`Write` ツールは事前 Read 必須**。既存ファイルの上書きで何度かハマった (vision-value.tsx
  など)。`Edit` でもこの制約は同じで、log.md でも踏んだ
- **巨大コンテキスト引き継ぎ後の context loss**：別セッションで「lecture-hub の Dexie 作業」と
  混線しかけた。プロジェクトパスを明示する確認質問で軌道修正
- **vault の log.md / active_projects.md が他セッションと同時編集**で linter 反映に
  時間差 → 都度 Read してから Edit すれば安全
- **Vault の auto-sync が走るタイミングが読めない**。stage しても次の bash 実行前に
  commit されていることがある (今回もそう) → 上書きを避けるため、push 前に必ず git status

## 📋 次回同じことをするときのチェックリスト

### Claude Design handoff の取り込み

1. URL を WebFetch → 「Binary content」エラーが返るので、`tool-results/webfetch-*.bin` の
   実体パスを Bash で `file` して gzip 確認
2. `mkdir /tmp/<name> && tar -xzf <bin> -C /tmp/<name>` で展開
3. `wbs/README.md` を最初に Read (handoff の前提を確認)
4. `wbs/chats/chat1.md` を Read (ユーザーが何を欲しがっているか、どこに落ち着いたか)
5. `wbs/project/<開いていたファイル>.html` を Read (エントリーポイント)
6. import している styles.css / 各 component.jsx を順に Read
7. ここまで読んで初めて AskUserQuestion で実装スコープを確認 (上書き / 全書き直し / Tweaks 扱い / etc.)

### Next.js 16 + Tailwind v4 への移植

- `next/font/google` で Sora は `style: italic` を指定しない (variant扱い)
- `:root` レベルの CSS 変数は `<html>` に乗せる (フォント変数は body だと :root から見えない)
- 設計トークンは `globals.css` の `@theme inline { --color-foo: var(--foo) }` でTailwindに繋ぐ
- `useTweaks` のようなクライアント状態は `homepage-client.tsx` に集約、`page.tsx` は薄く保つ
- `lucide-react v1.16+` ではブランドアイコン (SNS系) が無い前提で、インラインSVG用意
- `Write` する前に必ず `Read` (既存ファイル) — 触ってないファイルでも同様

### 検証 (ネット不要範囲)

- `npx tsc --noEmit` → 型エラーゼロが必須
- `npm run build` → Turbopack ビルド通過を確認
- dev server は curl で HTTP 200 確認まで。目視は家でやる
- Lighthouse / 実機目視・DB接続を伴う動作確認は後回し

## 関連

- [[index]] - Salamat 領域ハブ
- `~/Downloads/07_開発・アプリ制作/salamat-website-v2` - プロジェクトパス
- 現行サイト: `salamat-toyo.web.app`

## 出典

- 2026-05-18 セッションでの YD ↔ Claude Code 実装ログ
- Claude Design handoff bundle (`~/.claude/projects/.../tool-results/webfetch-*.bin`)
