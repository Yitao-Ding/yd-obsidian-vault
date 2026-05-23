---
type: decision
date: 2026-05-21
category: architecture / harness / ui-ux
project: Task Hub
status: in_progress
related:
  - "[[active_projects]] #6 Task Hub"
  - "[[task_hub]]"
  - "[[2026-05-19_TaskHub_git整理_GitHub連携]]"
  - "[[available_capabilities]]"
---

# Task Hub UI/UX 根本見直し + GAN着想 4 自立型エージェント・ハーネス構築

## 背景

Task Hub は 2026-05-19 に git 整理 + GitHub Private push 完了し、🟡 完成済み・運用フェーズに入っていた。本日 (2026-05-21) YD から「フロントエンドレベルから仕様を見直したい。主要な機能、ツールの UI/UX を根本から変えていく」と方針転換指示。

現状 UI 把握の結果、5 大課題を特定:
1. **ブランド分裂** — spec は寒色系 #2563EB だが globals.css と login は orange グラデ (`#ff6a00 → white`)、Project 全体は青、入口だけオレンジで矛盾
2. **デザインシステム不在** — Tailwind 直書き、コンポーネント未抽出、各画面で同じ UI を再実装
3. **IA が浅い** — タスク中心の世界観なのに「自分のタスク」だけ。期限俯瞰 (カレンダー/ガント) / チーム活動 / 検索 全部なし
4. **インタラクション SaaS テンプレ的** — DnD なし / @メンション なし / OGP なし / フィルター未実装 (spec にはある) / リアクション なし / 招待リンク なし
5. **ロゴが「S」一文字** — 260 名 + 18 年の組織的個性ゼロ

## 選択肢 (アプローチ)

- A. 単一 Claude Code セッションで一気に書き換え
- B. **Anthropic Labs ハーネス設計に倣い、GAN 着想の生成×評価マルチエージェント構成**
- C. 外部デザイナーに依頼

## 決定

**B** (GAN 着想ハーネス) を採用。さらに **Design UIUX Evaluator を独立エージェントとして追加** し、合計 4 自立型エージェント構成にする。

## 理由

### A 単一セッションを避けた理由

Anthropic Labs の研究で実証されている 2 つの構造的問題:

1. **コンテキスト不安 (Context Anxiety)** — コンテキスト窓が埋まると焦って機能省略 / エラーハンドル雑化 / テスト省略 / CSS インライン化 / 「後で追加してください」を頻発
2. **自己評価の甘さ** — 自分の出力に「全体的に洗練されたデザインです、特に改善必要な点はありません」と返す。デザインのような主観タスクで特に深刻

Task Hub の UIUX 根本見直しは「数時間 〜 数日」スパンの大型タスクで、単一セッションでは品質の天井を突破できない。

### B GAN 着想ハーネスを採用した理由

- 生成器と評価器を分離 → 評価器を懐疑的にチューニング可能
- 外部からの具体的フィードバック → 生成器は具体修正に集中
- 生成 → 評価 → 修正 → 再評価のループが自動化される
- Anthropic Labs の実証: ソロ ($9 / 20分) vs フルハーネス ($200 / 6時間) で品質差歴然
- DAW 実験 ($124.7 / 3時間50分) でも最終 1 マイルを評価器が埋めた

### Design UIUX Evaluator を独立エージェントにした理由 (★ 本決定の独自追加)

Anthropic Labs の原典は「Planner / Generator / Evaluator (QA)」の 3 エージェント構成。今回は **デザイン UI/UX の重要度を最上位に置きたい** という YD 方針から、Evaluator を機能 QA とデザイン UI/UX で**意図的に分離**し 4 エージェントに拡張。

理由:
- Anthropic Labs 自身が「デザインで AI 評価が最も甘くなる」と言及
- QA Evaluator が機能とデザインを両方見ると、機能が動けばデザインを甘く採点する傾向に流れやすい
- スキル参照 (frontend-design / web-design-guidelines / ui-ux-pro-max / vercel:react-best-practices の 4 種) を強制するためにも分離が必要
- Anthropic 4 基準のうち「デザインの質 ★★★ / オリジナリティ ★★★」を最高重みで採点する役割を独立させる

## 4 エージェント構成

配置: `~/projects/salamat-task-hub/.claude/agents/` (プロジェクト直下)

| エージェント | 役割 | 主要ツール | 出力先 |
|---|---|---|---|
| **taskhub-planner** | 1〜4 行ビジョン → SPEC.md + DESIGN_DIRECTION.md + sprint-XX-*.md (スプリント契約付き) | Read, Glob, Grep, WebFetch, WebSearch, Write | `.claude/sprints/` |
| **taskhub-builder** | 1 起動 = 1 スプリント実装、完了時に自己レビュー必須 | Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, **Skill** | `src/` + `.claude/reports/sprint-XX-self-review.md` |
| **taskhub-qa-evaluator** | スプリント契約の機能要件を Playwright + Lighthouse で実機検証、スタブ検出 | Bash, Read, Grep, Glob, WebFetch | `.claude/reports/sprint-XX-qa.md` |
| **taskhub-design-evaluator** ★ | Anthropic 4 基準 (D/O ★★★, C/F ★) でデザイン採点、4 スキル参照必須、AIスロップ即不合格 | Bash, Read, Grep, Glob, WebFetch, **Skill** | `.claude/reports/sprint-XX-design.md` |

全エージェント `model: opus` (Anthropic Labs 実証通り、評価で甘くならないため)。

## Design Evaluator が必ず読み込む 4 スキル

1. **`frontend-design`** (Anthropic 製) — AI slop 回避哲学、distinctive 美学
2. **`web-design-guidelines`** (Vercel 製) — Web Interface Guidelines に WebFetch 照合、違反 file:line 列挙
3. **`ui-ux-pro-max`** — 67 styles / 96 palettes / 57 font pairings の DB 照合
4. **`vercel:react-best-practices`** — TSX 品質チェック

## Design Evaluator の AIスロップ即不合格ルール

以下のいずれか 1 つでも検出されたら即不合格:

- NG カラー: `purple-*` / `violet-*` / `fuchsia-*` のグラデ、`#ff6a00` / `from-orange-*` の残存
- NG レイアウト: 白カード+角丸+中央配置のテンプレ繰り返し、shadow デフォルトプリセットのみ
- NG タイポ: Inter / Roboto 単一、font-weight 400/700 のみ、見出し-本文比 1.5 倍以下
- NG アイコン: emoji の UI 装飾使用、`aria-label` なし、Heroicons デフォルト濫用
- NG インタラクション: hover が opacity だけ、focus-visible なし、Esc 不対応モーダル

## スプリント契約の徹底

Anthropic Labs の「スプリント契約」を踏襲。各スプリントは Planner が以下を機械テスト可能な粒度で書く:

- 機能要件 (QA Evaluator が検証) — 例: `<URL> で <操作> すると <DOM 状態>`、Lighthouse モバイル 85+/95+/90+
- デザイン要件 (Design Evaluator が検証) — 例: NG カラー出現 0、タイポ階層 3 段以上明確、フォーカスリング全 interactive 要素

「美しい」「使いやすい」のような主観表現は契約上**禁止**。

## コンテキスト管理戦略

Anthropic Labs の知見:
- Sonnet 4.5 はコンテキスト不安が強くリセット必須
- **Opus 4.5 / 4.6 / 4.7 では問題ほぼ解消** → コンパクションで OK

→ Opus 4.7 (1M context) を全エージェント採用したため、**スプリント分割は粒度の最適化目的のみで、コンテキスト不安対策ではない**。

## やってはいけないこと (本構成の制約)

- Planner が技術詳細 (Firebase スキーマ、Tailwind クラス) を勝手に決める
- Builder がスプリント契約外の機能を勝手に追加 (スコープクリープ)
- Builder が `git push` / `firebase deploy` を実行 (YD 判断、サブエージェントは commit までで止める)
- QA/Design Evaluator が「概ね良い」で合格判定
- Design Evaluator がスキル参照なしで自分のセンスで採点
- 機密ファイル (`serviceAccountkey.json` / `.env.local`) を git add

## 次のステップ

1. **YD のビジョン (1〜4 行)** を待つ → `taskhub-planner` 起動
2. Planner が SPEC + DESIGN_DIRECTION + sprint-01〜N を生成
3. Sprint 01 から: builder 実装 → qa + design 並列評価 → 不合格なら修正イテレーション → 合格で次スプリント
4. 全スプリント完了 → 統合 QA → 本番デプロイ判断 (YD)

## 関連実装

- `~/projects/salamat-task-hub/.claude/agents/taskhub-planner.md` (200 行)
- `~/projects/salamat-task-hub/.claude/agents/taskhub-builder.md` (152 行)
- `~/projects/salamat-task-hub/.claude/agents/taskhub-qa-evaluator.md` (207 行)
- `~/projects/salamat-task-hub/.claude/agents/taskhub-design-evaluator.md` (250 行)
- `~/projects/salamat-task-hub/.claude/sprints/` (Planner が今後生成)
- `~/projects/salamat-task-hub/.claude/reports/` (Evaluator が今後生成)

## ✅ うまく行ったこと

- 現状 UI 把握 (8 画面 + globals.css + spec + HANDOVER) を並列 Read で 1 ターンで完了
- spec と実装の乖離 (寒色系設計 vs 実装オレンジ) を仕様読み込みで即検出
- Anthropic Labs ハーネス設計の 4 基準・スプリント契約・AIスロップ NG リストを 4 エージェントに自然に埋め込めた
- Design Evaluator に 4 スキル参照必須化を組み込むことで、Claude の自己評価の甘さ問題に直接対処
- Skill ツールを `tools` フィールドに含めることで、サブエージェント内からスキル呼び出し可能にした (初版で抜けてた → 即修正)

## ❌ 詰まったこと

- **初版で Skill ツールの追加忘れ**: design-evaluator / builder の `tools` フィールドに `Skill` を入れ忘れた。スキル参照必須なエージェントが呼び出せない状態。書き出した直後に気づいて Edit で修正。再発防止: スキル参照を仕様に書いたら tools にも `Skill` を入れる
- **AskUserQuestion が拒否された**: 4 問×複数選択 (デザイン方向性 / 主役 user / 参考ツール / orange 処理) を一気に出したら YD に中断された。前回提案を完了させる前に方針転換の指示が来た可能性 + 重い UI を嫌った可能性。次回は質問は文章ベースで簡潔に、または 1〜2 問に絞る
- **Bash で `(app)` ディレクトリ名のシェルエスケープに注意**: `src/app/(app)/` を find / cp で扱う際にシングルクォート必須 ([[2026-05-19_TaskHub_git整理_GitHub連携]] でも同じ問題)
- **active_projects.md #6 が長らく「Vercelデプロイ完了」と誤記**だった件は 5/19 に訂正済みだったが、本作業中に再確認。Firebase Hosting で確定

## 📋 次回同じことをするときのチェックリスト

新しいプロジェクトで「フロントエンド根本見直し」級の大型タスクを GAN ハーネスで進める手順:

1. **現状把握フェーズ**: spec / HANDOVER / 全画面ファイルを並列 Read。実装と仕様のズレを洗い出す
2. **方針議論フェーズ**: 5 大課題サマリ + YD の好み (寒色 / 本物 / 数字) を踏まえて、デザイン方向性 / 主役 user / 参考ツールを文章ベースで確認
3. **エージェント配置**: 
   - プロジェクト直下 `.claude/agents/` に置く (グローバル `~/.claude/agents/` ではなく、プロジェクト制約を frontmatter / 本文に書き込めるため)
   - `agents/` `sprints/` `reports/` の 3 ディレクトリを mkdir で先に作る
4. **エージェント定義の必須項目**:
   - YAML frontmatter: `name` / `description` (Use proactively 条件含む) / `tools` (Skill 必要なら必ず含める) / `model: opus`
   - 本文: 必読コンテキスト (並列 Read 指示) / 評価基準 / 出力フォーマット / やってはいけないこと / 完了報告フォーマット
5. **役割分離の徹底**:
   - Planner は「何を」、Builder は「どう作るか」、Evaluator は「動くか・美しいか」
   - QA と Design を 1 つにまとめない (デザイン採点が甘くなる)
6. **スプリント契約の機械テスト可能化**:
   - 「美しい」「使いやすい」禁止
   - 具体的 URL / 操作 / 期待 DOM / Lighthouse 数値 / NG カラー出現 0 / ARIA 属性
7. **AIスロップ NG リストを Design Evaluator にハードコード**:
   - 紫グラデ / 白カード+角丸+中央配置 / Inter 単一 / emoji 装飾 / hover:opacity / focus-visible 欠如
8. **モデル選定**: 全エージェント Opus (Sonnet で評価器を回すと甘くなる、Anthropic Labs 知見)
9. **コンテキスト管理**: Opus 4.6+ ならコンパクションで OK、リセット不要。スプリント分割は粒度最適化のためだけにやる
10. **保存タイミング**: ハーネス構築完了時点で decisions に保存。Sprint 開始後は各 sprint 終了時に追記 (要に応じて)

## 参考

- Anthropic Labs ハーネス設計研究 (Planner / Generator / Evaluator + スプリント契約)
- DAW 実験 ($124.7 / 3時間50分) / 2D レトロゲームメーカー実験 ($200 / 6時間)
- GAN (Generative Adversarial Networks) の生成器×判別器の対立構造
- Claude Code サブエージェント機能 (`~/.claude/agents/` or `<project>/.claude/agents/` に YAML frontmatter + 本文)
