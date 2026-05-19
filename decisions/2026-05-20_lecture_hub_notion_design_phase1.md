---
date: 2026-05-20
type: decision
category: design
tags: [lecture-hub, ui, notion, design-system]
related:
  - "[[2026-05-19_tiptap_migration]]"
---

# Lecture Hub: Notion 風デザイン刷新 (Phase 1)

## 背景

2026-05-19 21:50 に TipTap 移行で機能面の本番デプロイが完了 ([[2026-05-19_tiptap_migration]])。
しかし YD から **「デザインがとにかく終わってる」「初期アプリ感」** の指摘。
特定の何かが悪いのではなく全体的な要素 (ボタンの形・大きさ・配色・余白・タイポ) が
全部洗練されていない。

YD の希望: **「Notion のデザイン・文字配置・ボタンをそのまま取り入れてみてほしい」**。

## 選択肢

- A. token (色 + 余白 + タイポ + radius) から刷新してコンポーネントに反映
- B. shadcn 系を base にカスタム
- C. tailwindcss/typography 公式 prose をベースに

## 決定

**A (Notion 風 token + 主要コンポーネント順次刷新)** を採用。
既存の token (globals.css) は元から Notion 風 (canvas #FFFFFF, surface #F7F6F3, dark #191919)
で体系化されており、配色 primary を Purple → Blue に振るだけで大枠が整う。

## 実装 (Phase 1)

### 配色
- `--color-primary` を **Notion Purple #5C46E5 → Notion Blue #2383E2** に変更
- pressed / deep も追従 (`#1A6BC0` / `#0F4A8C`)
- 既存の Navy / Brand / Pastel tint はそのまま (将来使い分け)

### ボタン (`src/components/ui/button.tsx`)
- 高さ: `h-10` → **`h-8`** (32px)、`sm` は h-7 (28px)
- 角丸: `rounded-md` (8px) → **`rounded-[4px]`** (4px)
- gap: 2 → 1.5
- フォント: `text-button-md` → `text-body-sm-medium`
- アイコン: 16px → 15px
- 影なし、hover で BG だけ動く
- focus ring: `ring-offset-2` 撤去、Notion 風シンプルに
- ghost variant: text-ink → text-slate (普段は薄く、hover で濃く)

### サイドバー (`src/components/sidebar/Sidebar.tsx`)
- 幅: 260 → **240px**
- ワークスペースヘッダ: 28px → 18px monogram、flat
- ナビアイテム: `py-1.5` → `h-7 px-2`、間隔 `gap-1` → `gap-px`
- 検索リンクに `⌘K` ヒント (右端、小さい灰色)
- ページセクション: uppercase ラベル → 小さい灰色テキスト (controlled、tracking-wide)
- 新規ページボタン: `Button ghost` → `<button>` 直書きで他ナビと統一

### PageTree (`src/components/sidebar/PageTree.tsx`)
- ファイル名行: `py-1` → `h-7`
- アイコン 14px、左 padding 8px + depth × 14px
- gap: 1.5 (タイトとアイコンが近く、Notion ぽい)

### Topbar (`src/components/topbar/Topbar.tsx`)
- 高さ: `h-16` (64px) → **`h-11`** (44px)
- 背景: `bg-canvas` → `bg-canvas/80 backdrop-blur` (Notion の浮遊感)
- gap / padding 微調整

### エディタページ (`src/app/(app)/p/[id]/PageEditor.tsx`)
- レイアウト: `max-w-[820px]` → `max-w-[760px]`、`px-8 py-10` → `px-12 pt-24 pb-32` (Notion 余白)
- タイトル input: heading-1 + leading-tight + tracking-tight + 40px + 600 weight
- placeholder: 「無題のページ」→「無題」、text-stone → text-muted (薄め)
- 削除ボタン: 大きな ghost icon button → 右上の subtle icon (size-7、hover で text-error)
- 保存状態: 本文上の段落 → 上部ミニメタ行 (text-caption text-stone)、idle 時非表示

### タスクページ (`src/app/(app)/tasks/TasksClient.tsx`)
- ヘッダ: heading-2 + 説明文 → **40px H1 のみ** (Notion 風)、説明文は削除
- 作成フォーム: `Card` でラップ → border-b 1 本だけの inline フォーム
- input / select: コンパクト (h-8 / h-7、rounded-[4px])
- Tabs: `pill` variant (丸ボタン) → **`segmented`** (underline) — Notion 風
- リスト: `Card` + 各タスクが border-rounded-lg → **`<ul>` の中で `h-9 hover:bg-surface`** の database 行
- 削除ボタン: 常時表示 → hover で `opacity-100` に
- 期限表示: text-body-sm → text-caption-bold、color は overdue / today / future で 3 段階

## ❌ やらなかった (Phase 3 行き)

- /search /chat /admin/reindex ページの Notion 風刷新
- BubbleMenu (選択時の浮遊メニュー) + SlashMenu (`/` 入力) でエディタの Toolbar を置き換え
- ページごとの絵文字アイコン (Notion 特有)
- サイドバー上部の大きな検索ボタン化 (Topbar から移行)
- 旧 BlockNote 関連 `*.bak` 削除 + `@blocknote/*` パッケージ remove
- 本番で 音声 / AI 要約 / タスク抽出 / 数式 の動作確認

## 本番デプロイ

- commit: `8929e5f Notion-style design refresh (phase 1)` push 済 (`f764346..8929e5f`)
- 本番: `https://lecture-hub-sable.vercel.app/` (新エイリアス、TipTap 移行から継続)
- `next build` 4 秒、tsc 通過、エラーゼロ

## ✅ うまく行ったこと

- **token が既に Notion 風に体系化されていた**: globals.css は #FFFFFF + #F7F6F3 + #191919 ベースで揃っていたので、コンポーネントの使い方を直すだけで Notion 寄せが効いた。token から再設計する必要がなかった
- **primary 色だけで体感がだいぶ変わる**: Purple → Blue で「Notion ぽさ」が一気に増した。focus ring の色も同期して整った
- **段階的アプローチ**: token / ボタン / サイドバー / Topbar → エディタ / タスク の 2 ステージで進めて、各段階で動作確認できた
- **既存の `segmented` variant を発見**: Tabs の variant に `pill` 以外に `segmented` (underline) が既にあった。Tabs コンポーネントを書き換えずに variant 切り替えだけで Notion 風に
- **コンパクトサイズへの寄せ**: 高さ・余白・アイコンをすべて少しずつ縮めるだけで「ぎっしり」「業務アプリ」感が消えた

## ❌ 詰まったこと

- **Phase 1 を見ても「まだ初期アプリ感」と言われた**: ボタン + サイドバー + Topbar だけだとメインコンテンツ (タスク / エディタ) の「業務アプリ感」が残り、印象変化が弱かった。レイアウトもまとめて変える必要があった
- **Edit ツールが他 CC からの書き込みでブロックされた**: 並行で別 CC が active_projects.md を編集していて、Edit が「File has been modified since read」で失敗。Vault は append-only パターンが安全 (`cat >> file`)
- **Write ツールが「Read していない」エラー**: Bash の cat で読んだだけだと Read ツールの状態が更新されない。Edit/Write 前は必ず Read ツールを通す ([[claude_mistakes]] A-4)
- **絵文字をテキストに置換し忘れ**: PageEditor の削除ボタンに最初絵文字風アイコン (📄 🎙) を入れかけたが、CLAUDE.md ルールに反するので lucide-react アイコンに置換 (今回は最初から lucide で正解)

## 📋 次回 Notion 風デザインを当てるときのチェックリスト

### token 設計
- [ ] canvas / surface / surface-soft で Light は #FFFFFF / #F7F6F3 / #FAF9F7、Dark は #191919 / #202020 / #1B1B1B
- [ ] ink グラデーション (#1F1F1F → muted) を Notion の `Color tokens` に合わせる
- [ ] primary は Notion Blue (#2383E2) or Black (#000)。Purple は使わない (Notion ぽくない)
- [ ] hairline は極めて薄く (rgba(55,53,47,0.09) 相当)
- [ ] radius は 3-4px、md 以上は使わない (Notion はほぼ全て 3-4px)
- [ ] shadow は subtle (`0 1px 2px rgba(0,0,0,0.04)`) 程度。card-shadow は基本使わない

### コンポーネント
- [ ] ボタン: `h-8 max`、`rounded-[4px]`、影なし、`gap-1.5`、アイコン 15px、フォントは body-sm-medium
- [ ] Input / Select: `h-8`、`rounded-[4px]`、border-hairline-strong
- [ ] Tabs: `segmented` (underline) のみ。`pill` は使わない
- [ ] Card: ほぼ使わない。区切りは `border-b border-hairline` で十分
- [ ] Badge: 小さく (`h-5 px-2 text-caption`)、丸めは radius-sm

### レイアウト
- [ ] サイドバー: 240px、surface-soft 背景、padding 12px
- [ ] エディタ: max-width 760px、py-24-32、px-12
- [ ] タスク等のページ: max-width 960px、py-24-32、px-12
- [ ] 行 (database row): h-9、hover で bg-surface、削除ボタンは hover で出現

### タイポグラフィ
- [ ] H1: 40px / 600 / tracking -0.018em / leading 1.2
- [ ] H2: 30px / 600 / tracking -0.014em
- [ ] H3: 24px / 600
- [ ] body-md: 16px / 400 / leading 1.55
- [ ] body-sm: 14px / 400 / leading 1.5
- [ ] caption: 13px / 600 (label) or 400 (text)

### よくある落とし穴
- [ ] `text-on-dark` (dark mode の白テキスト) は Notion だと `rgba(255,255,255,0.81)` 風に若干透過しがち
- [ ] サイドバーのナビアイテムを ghost Button にすると padding が合わない → `<button>` 直書きの方が integer 高さに収まる
- [ ] EditorToolbar が常時表示の Word 風だと Notion 感ゼロ → BubbleMenu + SlashMenu に移行が筋 (Phase 3)
- [ ] アイコンサイズはコンポーネント全体で統一 (14-15px が Notion 標準)
- [ ] 余白を緩めにとる: Notion の本文ページは画面横幅の 40-50% を余白に使う

## 関連

- 親 (Lecture Hub TipTap 移行): [[2026-05-19_tiptap_migration]]
- 実装: `~/projects/lecture-hub/src/components/{sidebar,topbar,ui,editor}/` + `app/(app)/p/[id]/PageEditor.tsx` + `app/(app)/tasks/TasksClient.tsx`
- 本番: https://lecture-hub-sable.vercel.app/
- commit: `8929e5f Notion-style design refresh (phase 1)`
