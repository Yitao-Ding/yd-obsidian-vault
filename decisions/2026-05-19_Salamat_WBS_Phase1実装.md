---
type: decision
date: 2026-05-19
project: salamat-website-v2
phase: phase-1
---

# Salamat WBS v3 — Phase 1 実装完了 (2026-05-19)

## 概要

Salamat 公式HP v2 (`salamat-website-v2`) に対して、design-brief 収集 → 優先度付け → Phase 1 一気実装を1日で完了。
YD から「ここからの作業は全て Claude 判断で進めて、最後にレビュー」の委譲を受け、Claude が単独で実装。

## Phase 1 で実装した6項目

| # | プロンプト | 適用先 |
|---|----------|--------|
| #02 | Cobe 地球儀 | Action 各国の背景 |
| #03 | LocationTag | 各国 hero タイトル直下 (Cebu/Tokyo の現地時刻) |
| #04 | Glowing Shadow + MeshGradient (簡略版) | カード全部 |
| #05 | Radial Orbital Timeline | List ⇄ Orbital のビューモード切替 |
| #06 | Gallery4 構造 | Story/Action/Report の全カード統一 |
| #08 | Hero リデザイン | ZoomParallax × Shader Showcase 融合 |

Phase 2 持ち越し: #07 Magnetic+Fey ボタン / #01 Three.js パーティクル背景。

## 新規・書き換えファイル

**新規 10**: `src/components/effects/mesh-gradient-shader.tsx` / `ui/gallery-carousel.tsx` / `ui/cobe-globe.tsx` / `ui/location-tag.tsx` / `orbital/orbital-icons.tsx` / `orbital/orbital-data.ts` / `orbital/radial-orbital-timeline.tsx` / `orbital/view-mode-toggle.tsx` / `orbital/orbital-view.tsx`

**書き換え 7**: `sections/hero.tsx` / `sections/story-section.tsx` / `sections/report-section.tsx` / `sections/action-section.tsx` / `tweaks/types.ts` / `tweaks/tweaks-panel.tsx` / `homepage-client.tsx` / `app/globals.css`

**依存追加**: `three` 0.184.0 / `@types/three` / `cobe` 2.0.1 / `@paper-design/shaders-react` 0.0.76

## 検証結果

- `tsc --noEmit` 通過 (0 errors)
- `next build` (Turbopack) 通過 — 静的ページ4個生成
- `next dev` で HTTP 200 確認 (`http://localhost:3001/`)
- YD 目視レビュー: 「ちゃんと変わってます。大丈夫」

## ✅ うまく行ったこと

- **design-brief.md パターンが優秀**: 8個のプロンプトを構造化記録 → 優先度付け → Phase 計画 → 実装パラメータ確定 → 一気実装、の流れが破綻なく回った
- **収集と実装の二段フェーズ**: YDが即実装を期待せず溜める方針にしたことで、後半の実装で「全体を見た統合」(Gallery4 + Glowing Shadow の組合せなど) ができた
- **Claudeの率直な懸念表明 → YDの積極判断**: Claude案では Phase 2 持ち越し推奨だった #04 #05 を YD が Phase 1 に繰り上げる判断、議論が機能した
- **既存依存の活用**: framer-motion / embla-carousel-react / lucide-react / clsx / tailwind-merge / class-variance-authority が既存。追加は three / cobe / @paper-design/shaders-react の3つだけ
- **TaskCreate での進捗管理**: Phase 1 を10タスクに分解、in_progress/completed で状態管理、context 切れに耐える形にできた
- **画像 Read による直接確認**: 「写真が全部反対」の YD 報告に対して、Read ツールで画像を6枚並列確認し、ファイル名と中身の不一致を確証した上で修正

## ❌ 詰まったこと

- **`@paper-design/shaders-react` のバージョン差異**: 21st.dev のサンプルコードは古い API (`backgroundColor` / `spotsPerColor` / `frame`) を使用。0.0.76 ではこれらが廃止または改名 (`spots`, `colorBack`, parent background)、tsc エラーで発覚。`node_modules/.pnpm/@paper-design+shaders@0.0.76/.../dist/shaders/*.d.ts` を直読みして対応
- **lucide-react v1.16 のアイコン互換性**: 旧 v0.x のアイコン名 (Heart/Globe2 など) が削除/改名されている可能性が高い。予防的に自前 SVG (`makeIcon`) で6種実装した
- **写真ファイル名 vs 中身の不一致**: `ph-*.jpg` の中身が実は日本、`jp-*.jpg` の中身が実はフィリピン。YDの指摘で発覚。Read で6枚確認 → コード上のマッピングを入れ替え (ファイル名はそのまま、Phase 2 で cleanup予定)
- **pnpm の build scripts 警告**: `sharp@0.34.5 / unrs-resolver@1.11.1` の build scripts が ignored。`pnpm tsc` 経由だと install チェックでエラーになる。`./node_modules/.bin/tsc` `./node_modules/.bin/next` を直接呼ぶことで回避
- **dev server の二重起動エラー**: 2回目起動で 3001 ポート使用中。`lsof -ti:3001 | xargs kill` で確実に止める必要

## 📋 次回同じことをするときのチェックリスト

### 「収集 → 優先度付け → 一気実装」を回すとき

1. プロンプトを投げてもらう度に design-brief.md に #NN として構造化記録 (参考URL / YDのコメント原文 / Claude抽出 / 詰めポイント / 判定)
2. 各プロンプトに対して Claude 解釈を明示し、AskUserQuestion で要点だけ確認 (3問以内)
3. 収集が一巡したら、Claude の率直な懸念表明 (技術スタック肥大化 / 演出過密 / パフォーマンス / メンテコスト / 情報設計と装飾のバランス)
4. ABC評価 + Phase 1/2/3 分割案を提示 → YDが判断 (積極/慎重は YD に委ねる)
5. 実装パラメータ全部 Claude 仮置きして design-brief に書き込み (Hero高さ、写真選定、緯度経度、カード幅、色、ノード構成など)
6. TaskCreate で実装ステップを分解 (10前後)、in_progress/completed で進捗管理
7. 依存追加 → コア実装 → tsc 通過 → build 通過 → dev起動確認 → レビュー資料作成
8. 写真などのアセットは Read で中身確認してから配置 (ファイル名を信用しない)

### Salamat WBS 固有の注意

- `salamat-website-v2/AGENTS.md` の「This is NOT the Next.js you know」警告に注意 (Next.js 16)
- ファイル名と中身の不一致がある (`ph-*.jpg` が日本、`jp-*.jpg` がフィリピン)、Phase 2 で cleanup
- `next-themes` は導入しない (現プロジェクトは独自 `.dark-mode` クラス)
- shadcn は導入しない (既存 CSS 変数で代替)
- `@paper-design/shaders-react` の API はバージョンごとに違うので `node_modules/.../dist/*.d.ts` を直読みして確認

### 検証フロー (コピペ可能)

```bash
cd "/Users/ittou/Downloads/07_開発・アプリ制作/salamat-website-v2"
./node_modules/.bin/tsc --noEmit
./node_modules/.bin/next build
./node_modules/.bin/next dev -p 3001 &
sleep 6 && curl -s -o /dev/null -w "HTTP %{http_code}\n" http://localhost:3001/
open http://localhost:3001/
# 確認後
lsof -ti:3001 | xargs kill
```

## YDの意思決定の特徴 (この日に観察された)

- 「ここからの作業は全て Claude 判断で進めて、僕の承認は不要、最後にレビュー」と委譲する判断ができる
- Claude の懸念表明 (Phase 2 持ち越し推奨) に対して、自分の哲学 (動くものより本物のクオリティ、最初から完成形を目指す) で逆判断できる
- 「変なリクエスト」「うまく作れない」と Claude が思っているかを率直に聞く
- 細かい確認 (Q&A) より、まとめて意思決定したいタイプ

## 関連

- [[wbs_team]] - WBS チーム / プロジェクト概要
- `~/Downloads/07_開発・アプリ制作/salamat-website-v2/design-brief.md` - 詳細な収集ログ・実装パラメータ・レビュー資料
- `~/Downloads/07_開発・アプリ制作/salamat-website-v2/CLAUDE.md` & `AGENTS.md` - プロジェクト固有の注意
