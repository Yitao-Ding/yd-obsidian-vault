---
type: knowledge
domain: programming/tools
created: 2026-06-03
tags: [claude-design, design, handoff, frontend, anthropic]
priority: medium
---

# Claude Design — デザインバンドルの受け取り→実装

> Claude Design(claude.ai/design)は、AIでHTML/CSS/JSのモックを作り、**コーディングエージェントに実装させるためのバンドル**を書き出せるツール。
> YD がデザインを作り、URL を渡してきたら Claude Code 側でこの手順で取り込む。初運用: 2026-06-03(パソコン1台の学校 LP)。

## 受け取りの実体

YD から `https://api.anthropic.com/v1/design/h/<id>` 形式の URL が来る(末尾 `?open_file=index.html` は Web ビューア用、無視可)。

```bash
# 1. GET でバンドル(gzip tarball)をダウンロード
curl -sL "https://api.anthropic.com/v1/design/h/<id>" -o design.bin
file design.bin            # → gzip compressed data
# 2. 展開
tar xzf design.bin
```

中身の構造(例):
```
<project>/README.md          ← まず読む(エージェント向け指示)
<project>/chats/chat1.md      ← ユーザーと設計AIの全やりとり(意図の源)
<project>/project/index.html  ← primary file(LP本体)
<project>/project/css/*.css   ← デザイントークン + スタイル
<project>/project/js/app.js   ← インタラクション
<project>/project/screenshots/*.png ← レンダリング結果
<project>/project/uploads/*   ← ユーザーがアップした素材
```

## README が指示する正しい進め方(重要)

1. **chats/ の会話ログを先に読む** — 最終的にユーザーがどこに着地したか、何を本当に欲しがっているか。
2. **primary file(index.html)を上から下まで読み、import を全部辿る**(CSS/JS)。
3. **ピクセル単位で「ターゲット技術」に再現**(React/Next 等)。プロトタイプの内部構造はコピーせず、視覚出力を再現。
4. 曖昧なら実装前にユーザー確認。
5. README いわく「スクショは撮るな(ソースに全部書いてある)」。ただしユーザー提供の screenshots/ を**見る**のは有効。

## ✅ うまく行ったこと

- **`.ed` スコープ隔離**: Claude Design の `site.css` を `app/editorial.css` に移植する際、全セレクタを `.ed ` 接頭辞でスコープ化し、LP の root を `<div className="ed">` にした。これで既存アプリページ(globals.css の .btn/.pill/.reading 等)と**衝突せず**、見た目だけ差し替え + 既存機能(Stripe/ゲート)を温存できた。
- フォント名(Shippori Mincho 等)は CSS 変数を next/font の変数にマップ(`--serif-jp: var(--font-mincho-jp)`)。Google Fonts の `<link>` は使わず next/font に寄せる。
- インタラクション(ターミナルのタイピング、reveal-up の IntersectionObserver、FAQ アコーディオン)は `"use client"` コンポーネントに移植。
- CTA は**プロトタイプのダミー `<a href="#">` を実機能に差し替え**(Stripe Checkout フォーム / 実ルートへの Link)。「見た目コピーで終わらせない」のが肝。

## ❌ 詰まったこと

- `:root` のグローバル変数 / `body` / `*` / `h1-h3` / `.btn` / `.pill` を素のグローバルで取り込むと、Tailwind や既存コンポーネントと**全面衝突**する。→ 全部 `.ed` 配下にスコープして解決。`body{}` は `.ed{}` のルートに移し替え、`*{}` は preflight に任せて省略。
- Claude Design が選んだフォント(Shippori Mincho)が、こちらの過去の批評で「ニセ太字」と言われた書体だった。→ 「ユーザーが選んだデザインを実装する」が優先なので**Claude Design に合わせる**(weight 400/500 で軽く使えば問題なし)。
- gzip バンドルの base URL は GET 専用(HEAD は 405)。`/README.md` 等のサブパスは 404 — **必ず base を tarball として落として展開**する。

## 📋 次回チェックリスト

- [ ] URL が来たら `curl -sL <base> -o design.bin && file design.bin`(gzip 確認)→ `tar xzf`
- [ ] README → chats/ → primary file → imports の順で読む
- [ ] CSS は `.ed` 等のスコープ class で隔離(既存 globals/Tailwind と衝突回避)。`:root`→スコープ root、`body`→スコープ root に変換
- [ ] フォントは next/font の CSS 変数にマップ
- [ ] JS 挙動は `"use client"` コンポーネントへ。reveal 系は撮影前に強制可視化
- [ ] **ダミー CTA を実機能(決済/ルート)に必ず差し替える**
- [ ] PII(団体/学校/勤務先名)が混ざっていれば実装時に一般化

## 関連

- [[claude_code]] / [[ui-ux-pro-max]] / [[frontend-design]]
- [[2026-06-03_パソコン1台の学校_情報商材システム構築]]
- `~/Downloads/パソコン1台の学校_デザイン依頼プロンプト.md`(逆方向: Claude Design に渡す依頼ブリーフの作り方)
