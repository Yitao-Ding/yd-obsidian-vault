---
name: ハタチたちHP microCMS不使用・site.ts管理に決定
description: hatachi-tachi-website で microCMS を使わず src/content/site.ts 直書き管理にする旨YDが決定
type: project
---

# ハタチたちHP: microCMS不使用・site.ts 管理に決定 (2026-09-05)

## 状況

Vercel デプロイ後、YD が「CMSやらずにできる方法ないの、もうそういうのめんどいからお手軽のがいい」と発言。

## 選択肢と判断

もともとの設計は microCMS 無料枠で news / archive / site の3 API を管理し、Webhook で Vercel 再ビルドする想定だった。ただし、コードは最初から「環境変数がなければ site.ts のフォールバックを返す」設計になっており、microCMS 未接続でも全ページ成立する。

YD の「めんどい」という明示的な意思を受けて、microCMS は導入しない。更新は `src/content/site.ts` を直書きして push するだけ。YD が「上映日これね」と言えば Claude が直す運用。

スタジオメタリは PD 個人の屋号で代替わりしないため「学生団体が管理画面を使える必要がある」という前提がそもそも成立しない。

## ✅ うまく行ったこと

site.ts フォールバック設計が最初から入っていたため、この決定でコードの変更ゼロ。

## ❌ 詰まったこと

該当なし。

## 📋 次回同じことをするときのチェックリスト

- microCMS の接続コード (lib/microcms.ts) はそのまま残っていい。接続しなければ動かないだけで、削除しなくていい
- 更新したいときは `src/content/site.ts` を編集して `git push` → Vercel が自動でリビルドする
- もし将来 CMS が欲しくなったときの候補: Notion + notion-to-md、Contentful 無料枠
