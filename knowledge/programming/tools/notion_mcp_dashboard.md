---
type: knowledge
created: 2026-07-11
tags: [notion, mcp, dashboard, linked-view, workflow]
---

# Notion MCP ダッシュボード構築 (リンクドビュー方式)

> 出典: 2026-07-11 Yitao Film 案件管理「試作ダッシュボード3案」セッション (Claude Code / ultracode Workflow)。
> 既存DBを一切壊さず、ダッシュボードページを `notion-create-view` のリンクドビューだけで組む方法と、実戦で踏んだ罠の記録。

## 概要

- 対象: 既存3DB (👤クライアント / 🎬案件 / ✅タスク) の上に試作3案 (A=コックピット / B=パイプライン / C=ミニマル) を構築
- 親ページ: 🧪 試作3案 https://app.notion.com/p/399d8d9d19c2814c8fefdc0987e2d52a
- 体制: Workflow 7エージェント (builder=Opus 4.8 ×3並列 / verifier=Sonnet 5 ×3 / fixer=Opus ×1)、計19ビュー+詳細デモ3ページ、約507kトークン・約10分
- Fable5 が司令塔 (設計ブリーフ・検証設計)、実装は Opus以下 — YD指定のモデル分業で運用

## ✅ うまく行ったこと

- **ページ構築順序の制御**: 「`notion-update-page` insert_content (position=end) → `notion-create-view` (parent_page_id)」を交互に呼ぶと、呼んだ順=ページ上の並び順になる。ビューの位置制御はこれ一択
- **チェックボックスの view DSL フィルタ**: `FILTER "完了" = "__NO__"` が一発で通る (`= false` へのフォールバック不要だった)
- timeline (`TIMELINE BY "納品日"`) / chart number (`CHART number AGGREGATE count|sum ON "報酬"` + `HEIGHT small`) / board `GROUP BY` / gallery、全部 create-view の configure 一発成功。「No approval received」一過性エラーは今回ゼロ
- **builder と verifier を別モデルにする独立検証**が機能: builder (Opus) が自己検知できなかった critical (テーブル生タグ化) を verifier (Sonnet) が捕捉 → fixer で修復。2026-05-26 決定「evaluator は builder と別モデル」の実証例
- サブエージェントのプロンプトに「絶対禁止」節 (下記) を明記したことで、3並列でも実データ破壊ゼロ

## ❌ 詰まったこと

- **`<details>` 生タグの不安定さ**: insert_content で入れると summary がリテラル文字列化し、余分な `</details>` テキストブロックが残ることがある → **heading-toggle 構文 (`## 見出し {toggle="true"}` + タブインデント子) が安定**
- **`<table>` を1行に詰めると生タグ化**: エスケープされて `\<table header-row=...` の文字列としてページに表示される (C案デモで critical 化)。**タブインデントの複数行 `<table>` 構文で書く**。セル内にリンクがあると特に崩れやすい
- **ビューは columns / トグルの中に置けない**: create-view は常にページ末尾追記。KPIチップの横並びは API では不可 → 作成後に人間が Notion UI でドラッグして列にする (10秒)
- **`<database url="既存DBのURL">` を content に書くと実DBが移動する (破壊)**。リンクドビューは `notion-create-view` か `<database data-source-url="collection://...">` のみ。`<page url="既存URL">` も同様に移動になる
- `FREEZE COLUMNS` は fetch の view JSON に出ないため API では検証不可 → UI 目視のみ
- Notion MCP には**ページ削除 (ゴミ箱) ツールがない**。DB のゴミ行は `notion-move-pages` で DB 外へ退避 → UI で削除、が現実解

## 📋 次回同じことをするときのチェックリスト

1. 対象DBを fetch して data_source ID・プロパティ名・**select 選択肢の完全一致文字列** (絵文字後の半角スペース含む) を控える
2. `notion://docs/enhanced-markdown-spec` と `notion://docs/view-dsl-spec` を ReadMcpResourceTool で読む (構文を推測しない)
3. ページ骨格 create-pages → セクションごとに insert_content ⇄ create-view を交互
4. トグルは heading-toggle 構文、テーブルは複数行タブインデント
5. サブエージェント委任時は「絶対禁止」(database url 直書き / 既存ページ update / update-data-source / 既存DBへの create-view) をプロンプトに明記
6. builder と verifier は**別モデル** (今回 Opus / Sonnet)
7. 完了後、ホーム・親ページを自分の目で fetch して既存DBが動いていないか実物確認 (A-16 の規律)

## 📚 関連

- [[yitao-film-notion-handoff]] — 案件管理の正本 (ID・スキーマ・投入手順・MCPの癖 §0)
- [[reusable]] — 「builder/verifier 別モデル」パターンの昇格候補
- [[obsidian_vault]]
