---
type: knowledge
domain: programming
created: 2026-09-05
source: ハタチたち公式サイト (hatachitachi.com) の INSTAGRAM セクション修正
---

# Instagram 公式埋め込みの幅の制約

Instagram の `blockquote.instagram-media` + `embed.js` を使った公式埋め込みには、
実測で分かった幅の制約が2つある。どちらも「iframe の高さは正しく確保されるので
一見動いているように見える」のが厄介。

## 制約1: min-width 326px を強制で付けてくる

embed.js は生成した iframe に `min-width: 326px` をインラインで書き込む。
CSS Grid のセルが 326px より狭いと、iframe がセルからはみ出して隣と重なる。
親に `overflow` の指定が無ければ、はみ出しはページ全体の横スクロールになる。

ハタチたちの実例: 右列に小4枚を 2 列で並べていたところ、1280px 幅でセルが 251px。
4枚とも 75px ずつはみ出し、うち1枚は幅 1265px × 高さ 2px に化けて、
`document.documentElement.scrollWidth` が 2206px (ビューポート 1280px) になっていた。
デスクトップで右に 900px 以上の空白が伸びる状態で本番に出ていた。

同じ原因でスマホ (セル 185px) が先に壊れており、そちらだけ直して
デスクトップを見ていなかった。**片方の幅で直したら必ずもう片方も測る。**

## 制約2: リール (reel) は幅が広いと中身を描画しない

同じ埋め込みでも、リールは幅によって表示が壊れる。実測:

| 埋め込み幅 | 結果 |
|---|---|
| 345px | 動画・いいね・コメント行まで完全に出る |
| 400px | 動画は出るが、いいね行が切れて下に約90pxの空白 |
| 477px | iframe 全面が白紙 (高さ 802px は確保される) |
| 570px | 動画の上3分の2だけ出て、下3分の1が白紙 |

埋め込みページ (`https://www.instagram.com/reel/<ID>/embed/`) 単体の
コンテンツ高は幅に比例して素直に伸びる (326→616px, 570→921px) ので、
高さ計算がずれているのではなく描画側の問題。通常の投稿 (`/p/<ID>/`) では起きない。

**リールを埋め込むなら幅 345px 前後に固定する。**

## 実装の型

トラック幅を `1fr` や `auto` にすると、Instagram 側の min-width 326px に
引っ張られてビューポートごとに幅がばらつく (768px で 326px、1280px で 400px、
のように安定しない)。列幅は px で明示する。

```jsx
<div className="mx-auto grid max-w-[345px] gap-6
                lg:max-w-none lg:grid-cols-[345px_345px] lg:justify-center lg:gap-8">
  <div className="min-h-[420px] overflow-hidden">{/* 1枚目 */}</div>
  <div className="hidden min-h-[420px] overflow-hidden lg:block">{/* 2枚目 */}</div>
</div>
```

`overflow-hidden` は保険。読み込みに失敗した iframe が幅を取り違えることが
実際にあるので、ページ全体に波及させず列の中で止める。

2枚目を `lg` (1024px) から出しているのは、345×2 + gap32 = 722px が
`container = min(1120px, 92vw)` に余裕で収まる最初のブレークポイントだから。
`md` (768px) では container が 706px しかなく足りない。

## ✅ うまく行ったこと

- 「見た目がおかしい」を Playwright で数値化したのが早かった。
  `document.documentElement.scrollWidth` とビューポート幅の差を見ると横溢れが一発で分かる
- iframe ごとに `getBoundingClientRect()` と親セルの幅を並べて出したら、
  「セル251px に対し iframe 326px」という原因がすぐ特定できた
- リールの幅の閾値は、埋め込みURLを単体で開いて `document.body.style.width` を
  変えながら測ることで切り分けられた (親ページの都合と分離できる)

## ❌ 詰まったこと

- 最初に幅を 400px にしたら「リールは描画されるがいいね行が切れる」中途半端な状態になった。
  345px まで落として初めて完全に出た。閾値は思ったよりシビア
- `justify-center` + `grid-cols-2` のような相対指定だと、Instagram の min-width に
  負けて幅が安定しない。px 固定にするまで、ビューポートを変えるたびに違う幅が出て混乱した
- 埋め込みが白紙でも iframe の高さは正しいので、高さだけ測っていると「出ている」と誤判定する。
  必ずスクリーンショットで中身を目視する

## 📋 次回同じことをするときのチェックリスト

1. 埋め込みを置く列の幅を先に計算する。`(container - gap) / 列数` が 326px 以上か
2. リールを含むなら列幅は 345px 固定にする。可変にしない
3. 375 / 768 / 1024 / 1280 / 1440 の5つで測る。片方の幅だけ直して終わりにしない
4. 測るのは3つ: `scrollWidth` (横溢れ)、iframe の幅と高さ、そしてスクリーンショット (中身が出ているか)
5. embed.js は非同期なので、計測前に 8 秒以上待つ

## 📚 関連

- [[active_projects]] ハタチたち公式ホームページ
- [[claude_mistakes]] 2026-09-05 Tailwind の `md:` でスマホ専用スタイルを上書きしようとした
