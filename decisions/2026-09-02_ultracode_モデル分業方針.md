---
type: decision
date: 2026-09-02
project: 全プロジェクト共通 (Claude Code / ultracode 運用)
related: "[[2026-07-13_PAA全面改修]]"
---

# ultracode のモデル分業方針 (企画・監視 = Fable、実装 = Opus 以下)

## 状況

2026-09-02 朝、YD が Claude Code の `/model` を Fable 5.1 + ultracode effort に設定した直後の指示。ultracode は多数のエージェントを Workflow で並列起動する運用で、全部 Fable で回すとコストと速度の無駄が大きい。2026-08-08 の旧指示「可能な限り Fable、Opus 4.8 は禁止」をこの分業に置き換える。

## 決定

YD 原文: 「ウルトラコードでたくさんエージェントを立ち上げて作業をするときは、企画や監視、考えるところは Fable 使って、実装は Opus 以下で適切なモデルを判断して作業して」

| 役割 | モデル |
|---|---|
| 企画 (planner / spec-reviewer)、監視・検収 (qa / design evaluator、敵対的 verify、judge、完全性チェック)、設計判断 | fable |
| 実装 (builder、コード修正、移植、テスト作成) | opus |
| 機械的作業 (置換、定型生成、集計、フォーマット) | sonnet / haiku |

判断基準: 「その出力を誰かが検証するか」。検証される側は Opus 以下、検証する側は Fable。effort も同様に verify / judge は high 以上、機械作業は low。

適用済み: `project-agent-application/.claude/agents/` の planner / spec-reviewer / qa-evaluator / design-evaluator を `model: fable`、builder / designer は `model: opus` のまま。Claude Code のメモリ (`model-policy-fable5.md`) も同内容に更新。

## うまく行ったこと

2026-07-13 の PAA 全面改修で「Fable = 監査/計画/検収、Opus・Sonnet = 実装」の分業を実際に回し、105 所見を 3 Wave で実装・検収できた実績がある。今回の指示はその運用を標準化したもの。

## 詰まったこと

該当なし (指示の反映のみ)。旧メモリの「Opus 4.8 禁止」を残したままだと Opus 全体を避ける誤読が起きるので、旧世代 ID を固定しなければ `opus` エイリアスで問題ない旨をメモリに明記した。

## 次回同じことをするときのチェックリスト

1. Workflow の `agent()` は役割ごとに `model` を明示する (省略するとセッションのモデル = Fable を継承して実装まで Fable になる)
2. 新しい `.claude/agents/*.md` を作る時は frontmatter の `model` を役割で決める
3. designer は灰色 (美的判断が主なら fable に上げてよい)。迷ったら YD に聞く
4. セッション本体のモデルは Claude 側から変えない (`/model` は YD の操作)
