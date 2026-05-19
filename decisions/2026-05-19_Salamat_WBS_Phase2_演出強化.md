---
type: decision
date: 2026-05-19
project: salamat-wbs
phase: 2
related: [[2026-05-19_Salamat_WBS_Phase1実装]]
---

# Salamat WBS Phase 2 演出強化 完了 + 本番デプロイ (2026-05-19 夜)

## 背景

[[2026-05-19_Salamat_WBS_Phase1実装]] で Phase 1 (6項目) のローカル実装を完成させ、tsc/build/dev HTTP 200 まで通したが、本番 Vercel への反映は Phase 2 に持ち越していた。

Phase 2 の残タスクは 10 項目あり、性質がバラバラ (cleanup / 演出 / コンテンツ / インフラ)。本セッションでは、優先順位を「Cleanup → デプロイ → 演出強化 (#07 + #01)」に絞り、4 項目をまとめて完遂した。

## 今日完了した 6 項目

| # | 内容 | コミット | デプロイ |
|---|------|---------|---------|
| Phase 2-1 | 旧コード cleanup (country-hero/cards-band/scroller/card 関連 CSS 削除) | `46e6839` | dpl_G4DhSUZ6uHL41h3753vzAwdk76nB |
| Phase 2-2 | 写真ファイル名 cleanup (ph-/jp- の逆転を `ph-1〜3 / jp-1〜3` にナンバリング) | `46e6839` | (同上) |
| Phase 2-3 | Phase 1 を Vercel 本番に初デプロイ (`https://salamat-website-v2.vercel.app` HTTP 200) | — | (同上) |
| Phase 2-4 | #07 MagneticFeyButton 実装 + 主要 CTA に適用 (Hero × 2 + Vision Value) | `20ae3ee` | dpl_3cZb35DTmMDYeZyLDA4zXdzwDJCh |
| Phase 2-5 | #01 ParticleBg 実装 + Vision/Report/Story/News に per-section マウント | `20ae3ee` | (同上) |
| Phase 2-6 | Phase 2 演出を Vercel 再デプロイ (`https://salamat-website-v2.vercel.app` HTTP 200) | — | (同上) |

## 確定した実装パラメータ

### #07 Magnetic + Fey ボタン

| 項目 | 値 |
|------|-----|
| Magnetic 物理 | `useMotionValue + useSpring` (stiffness 180, damping 14, mass 0.18) |
| 吸着強度 (distance) | 0.35 |
| Fey 演出 | `radial-gradient` (50%/50% from rgba(186,226,255,0.7) → transparent) を `::after` で hover opacity 0/1 切替 |
| Variant | `primary` (白基調) / `ghost` (透明枠線) |
| 適用箇所 | Hero CTA × 2 (直接 MagneticFeyButton) + Vision Value CTA (CtaButton 経由) |
| Tweaks Panel | `magnetic-fey` を 4 番目のオプションとして追加、`DEFAULT_TWEAKS.buttonStyle = "magnetic-fey"` に |

### #01 Three.js パーティクル背景

| 項目 | 値 |
|------|-----|
| 適用範囲 | Vision / Report / Story (scroller + circular 両方) / News の **4 セクション** (Hero/Action は除外、Footer は今回スキップ) |
| 配置方式 | 各セクションに `<ParticleBg />` を per-section マウント (`position: absolute; inset: 0; z-index: 0; pointer-events: none`) |
| Density | Vision 1400 / Report 1200 / Story 1300 / News 900 (mobile は 1/3) |
| Opacity | Vision 0.65 / Report 0.55 / Story 0.55 / News 0.5 |
| Particle size | PC 0.07 / mobile 0.09 (sizeAttenuation: true) |
| Palette | `#1f5bd7 / #38b6ff / #0e2e70 / #6fb3ff / #7fd0ff` (寒色5色) |
| Blending | `NormalBlending` (Additive は文字が読みづらくなり却下) |
| Interaction | カーソル追従反発 (REPULSE_R 2.2, REPULSE_S 0.09) + 原点復帰 (RETURN_S 0.014) + 減衰 (DAMP 0.91) + 緩い rotation.z (0.00015/frame) |
| Three.js | top-level `import * as THREE from "three"` (dynamic import は dev で安定しなかったため断念) |
| アクセシビリティ | `prefers-reduced-motion: reduce` で全停止 |

### 旧コード cleanup

- **CSS 削除**: `.country-hero` / `.country-cards-band` / `.country-scroller` / `.country-card` (および `.num/.thumb/.label/.sub`) 系統 約 120 行、`.dot-map` / `.dark-mode .dot-map` / `.section-action .action-intro .dot-map` 約 15 行
- **コンポーネント**: `PhilippinesMap` / `JapanMap` / `PinIcon` は元々存在せず (Phase 1 で削除済み)
- **`.gitignore`**: `/.claude/` 追加 (Claude Code ローカル設定の除外)

### 写真ファイル名 cleanup

| 旧 (実態と逆) | 新 (実態) | 中身 |
|------------|----------|-----|
| ph-feeding.jpg (実は日本) | jp-1.jpg | 古民家、合宿、集合写真 |
| ph-lumbani.jpg (実は日本) | jp-2.jpg | 校門前集合、緑の山 |
| ph-selma.jpg (実は日本) | jp-3.jpg | 山道の木の階段、苔 |
| jp-kurodaira.jpg (実はフィリピン) | ph-1.jpg | 海辺のスモッグ、ゴミ山 |
| jp-terakoya.jpg (実はフィリピン) | ph-2.jpg | ゴミ山で遊ぶ子ども |
| jp-abk.jpg (実はマニラ着の古着) | ph-3.jpg | 段ボール箱の中の衣類、大理石床 |

シンプル番号方式を採用 (活動名と紐付けない)、コード側で「活動 → 画像」のマッピングを意図的に決める。
コード上の逆転マッピングを素直な形に戻し、関連 NOTE コメントも削除。

## 残 Phase 3 タスク

- **GitHub Private repo 作成 + push** (本セッションで remote 未設定が判明、commit は local のみ。`gh repo create` で初期化要)
- モバイル fallback の本格実装 (ZoomParallax 簡略 / 地球儀静止画 / ParticleBg 更に密度削減)
- circular Story レイアウトの Gallery 化判断
- 下層ページ実装 (About / Activities / Reports / News / Members / Support / Transparency)
- お問い合わせフォーム + CMS 連携 (microCMS/Notion/Sanity)
- 独自ドメイン取得 → Vercel に紐付け

---

## ✅ うまく行ったこと

- **段階デプロイ**: Phase 1+cleanup → デプロイ → Phase 2 演出 → デプロイ の 2 段階に分けたことで、各段階で「本番が壊れていないか」を切り分けて検証できた
- **写真ファイル名 cleanup の判断**: 「シンプル番号 (ph-1〜3 / jp-1〜3)」を採用したことで、将来の写真追加でも崩れない命名規則になった。活動とのマッピングはコード側に閉じ込めた
- **MagneticFeyButton の variant 分け**: primary (白基調) / ghost (透明枠線) の 2 variant に分けたことで、Hero CTA の左右ペアが視覚的に意味分け (Salamatを知る = ghost、Save Smile を見る = primary) できた
- **ParticleBg の per-section マウント**: 全画面 fixed 1 個ではなく、各セクションに独立 Canvas を持たせる方式にしたことで、各セクションの背景色・レイアウトを変更せずに済んだ
- **3つの blending を順番に試行**: AdditiveBlending → NormalBlending に切り替えで「文字が読みづらい」問題を解決。さらに opacity/size を 3 段階調整して、最終的に「見える + 読める」のバランス点に着地
- **AskUserQuestion でこまめに確認**: 「写真ファイル名の命名規則」「jp-abk の中身判定」「パーティクルの見え方」など、判断の分岐ごとに YD の意向を確認、後戻りを最小化
- **TaskCreate で 10 タスク分解**: 1 セッションで Cleanup 〜 Phase 2 デプロイまで貫通、context 長くなっても進捗を見失わなかった
- **Read で 6 枚並列画像確認**: 写真ファイルの中身を一気に確認、ファイル名と中身の対応を正確に把握 ([[claude_mistakes]] A-6 の再発防止が機能)

## ❌ 詰まったこと

- **ParticleBg が「立ち上がってない」問題**: 最初の実装 (dynamic `await import("three")` + NormalBlending + size 0.032 + opacity 0.42) で、YD のブラウザでパーティクルが視認できなかった。3 段階の修正 (a) top-level `import * as THREE from "three"` に変更、(b) size を 0.07 (PC) / 0.09 (mobile) まで上げる、(c) opacity を 0.5〜0.65 に戻す で解決
  - 原因の仮説: dynamic import + React StrictMode の組合せで useEffect が 2 回走り、WebGL コンテキスト確保で衝突した可能性 (確証はないが top-level import で解消したので、結果オーライ)
  - 教訓: WebGL 系コンポーネントは dynamic import より top-level import の方が dev で安定する
- **写真ファイル名と中身の逆転**: Phase 1 で `ph-*.jpg` が日本、`jp-*.jpg` がフィリピンの中身、と完全に逆転していた ([[claude_mistakes]] A-6 既出)。コード側の `img: "/assets/actions/jp-abk.jpg"` のような逆転参照を素直な形に戻すのに sed で一括置換した
- **GitHub remote が未設定**: commit したものの push できなかった。`git remote -v` が空。Phase 1 セッションでも GitHub repo を作っていなかったらしい。Vercel CLI 直接デプロイで切り抜けた
- **Edit ツールが file modified エラー**: sed で一括書き換えた直後に Edit を呼ぶと「File has been modified since read」で失敗。Read → Edit の順を守る必要があった ([[claude_mistakes]] A-4 既出)
- **Tweaks Panel の `buttonStyle` 型拡張**: TypeScript の型 union 拡張で、`types.ts` の `buttonStyle: "liquid" | "metal" | "cool"` を `"magnetic-fey" | "liquid" | "metal" | "cool"` に変更する必要があり、修正箇所が 4 ファイル (types.ts / cta-button.tsx / tweaks-panel.tsx / DEFAULT_TWEAKS) に散らばっていた

## 📋 次回同じことをするときのチェックリスト

### Vercel デプロイ (CLI 直接)

1. `cat .vercel/project.json` で link 状態確認 (projectId / orgId / projectName)
2. `git status` でステージング状況確認
3. `./node_modules/.bin/tsc --noEmit` で型エラー 0 を確認
4. `./node_modules/.bin/next build` で本番ビルド通過を確認
5. `vercel --prod --yes` でデプロイ実行 (`--yes` で対話スキップ)
6. 完了後の JSON 出力から `deployment.url` を取得
7. `curl -s -o /dev/null -w "%{http_code}\n" https://salamat-website-v2.vercel.app/` で alias の HTTP 200 確認 (deployment-unique URL は 401 になることがあるが、alias が 200 なら本番反映 OK)

### Three.js Particle 背景 (新規セクション追加時)

1. ParticleBg を import: `import { ParticleBg } from "@/components/effects/particle-bg"`
2. セクションの **最初の子要素** として配置 (CSS の z-index 構造を維持)
3. 親セクションが `position: relative` であることを確認 (今回 Vision/Report/Story/News はすべて relative だった)
4. density は 800〜1500 程度 (セクションの大きさに応じて)
5. opacity は 0.4〜0.65 (背景色とのコントラスト調整)
6. 開いた瞬間に見えない場合のチェックリスト:
   - DevTools で `<canvas>` 要素が DOM に存在するか
   - canvas の `width/height` が 0 でないか
   - Console に Three.js / WebGL 関連エラーが出ていないか
   - size を一時的に 0.15 まで上げてデバッグ表示

### Tweaks Panel に新スタイルを追加するときの 4 箇所修正

1. `src/components/tweaks/types.ts` の `TweakState.buttonStyle` 型 union を拡張
2. `src/components/tweaks/types.ts` の `DEFAULT_TWEAKS.buttonStyle` を新規値に (または既存値のまま)
3. `src/components/buttons/cta-button.tsx` の `CtaStyle` 型 union を拡張 + 分岐追加
4. `src/components/tweaks/tweaks-panel.tsx` の `options=[...]` に新規エントリ追加

### 写真ファイル名と中身が逆転していたとき

1. `Read` で 6 枚並列で中身を確認 (画像が読めるのでファイル名に頼らない)
2. 命名規則を YD に確認 (今回はシンプル番号方式 ph-1〜3 / jp-1〜3 を採用)
3. リネームは衝突しないように一括 mv (新名がすべて未使用なら直接可)
4. コード参照は sed で一括置換 (拡張子付きで部分一致を避ける、`'s|jp-abk\\.jpg|ph-3.jpg|g'`)
5. NOTE コメント (「中身が逆」など) を削除

### GitHub remote の事前確認

- セッション冒頭で `git remote -v` を確認、空なら最初に `gh repo create <name> --private --source . --remote origin` で作っておく
- そうしないと commit はできても push でハマる (今回経験)

---

## 関連

- [[2026-05-19_Salamat_WBS_Phase1実装]] - 前提となる Phase 1 の意思決定
- `knowledge/salamat/wbs_team.md` - WBS チーム情報 + 技術スタック
- `~/Downloads/07_開発・アプリ制作/salamat-website-v2/design-brief.md` - #01〜#08 の収集 + Phase 計画
- `~/Downloads/07_開発・アプリ制作/salamat-website-v2/AGENTS.md` - Next.js 16 の breaking change 注意書き
- [[claude_mistakes]] A-4 (Edit 前 Read 必須) / A-6 (画像ファイル名を信用しない) - 今回も再発防止が機能した

## 出典

- 本セッション 2026-05-19 21:30 〜 22:05 の YD ↔ Claude Code 対話
- commit `46e6839` / `20ae3ee`
- Vercel deployment `dpl_G4DhSUZ6uHL41h3753vzAwdk76nB` / `dpl_3cZb35DTmMDYeZyLDA4zXdzwDJCh`
