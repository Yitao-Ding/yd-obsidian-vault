---
type: course_log
course_id: 04
course_title: Introduction to Claude Cowork
course_title_ja: Claude Cowork 入門
category: Product Training
difficulty: Beginner
priority: 中〜高 (Task Hub 設計に直結)
status: in_progress
recommended_day: Day 2 (2026-05-20)
started_at: null
completed_at: null
cert_url: null
last_updated: 2026-05-20
tags: [anthropic-academy, product, cowork, beginner, agents, plugins, skills]
---

# 04. Introduction to Claude Cowork (Claude Cowork 入門)

## 一言サマリ

Claude が**デスクトップ上のファイル・フォルダ・アプリを直接読み書きして仕事を完了させる**「Cowork モード」の入門。Chat (会話) でも Code (CLI) でもない、第三のモードで、Plugins / Skills / Global Instructions / Steering で「自分のチーム専用のAI同僚」に育てる。

## 学ぶ前の前提知識 (3行)

- Claude Chat と Artifacts を使ったことがある (Claude 101 受講済 or 同等の経験)。
- ローカルファイル/フォルダ構造の基本 (パス、権限) がイメージできる。
- 4並列 Claude Code 運用中の YD にとっては、「Code を持たない人向けの Cowork = なぜCLIではなくGUIエージェントなのか」の論点を仕入れにいくコース。

## このコースで答えが出る問い (5問)

1. **Cowork の "task loop" は具体的にどう動くのか？** (Plan → Execute → Steer → Validate の循環？)
2. **Global Instructions ↔ Project-level Instructions ↔ Skills の階層はどう切り分ける？** (Vault憲法と何が違う？)
3. **Plugins と Skills の関係は？** (Plugin = Skills + Connectors + Slash + Sub-agents のバンドル？)
4. **Cowork で安全に長時間タスクを回すための「Steering」テクは何があるか？** (途中介入のベストプラクティス)
5. **Cowork と Claude Code をどう使い分けるべきか？** (ファイル操作とCLI実装の境界)

## 主要キーコンセプト (5個)

### 1. Cowork Task Loop (タスクループ)
Cowork は「会話」ではなく「**作業セッション**」。YD がゴールを描写 → Claude が計画 → 実行 → 進捗を見せる → YD が舵を切る → 完成物を返す、というループ。
Claude Code との違いは「**ターミナルではなくデスクトップアプリの中で動き、ローカルファイル/Office アプリを直接編集する**」点。Excel / PowerPoint / Word の成果物を完成形で出すのが強み。

### 2. Global Instructions / Standing Context
Cowork に「自分の働き方・好み・禁止事項」を一度書けば、毎タスクで自動適用される設定。
階層は: **Global (全体)** → **Project / Folder (案件単位)** → **Task (1回限り)**。
YD の Vault 運用との対応: `~/.claude/CLAUDE.md` ≒ Global、`~/ObsidianVault/CLAUDE.md` ≒ Project、各セッションの指示 ≒ Task。

### 3. Skills (再利用可能な手順書)
「うちのチームの議事録テンプレ」「PR説明文の社内ルール」のように、繰り返す手順を Markdown + 補助ファイルでパッケージ化したもの。
Cowork が呼び出し可能にして使い回す。
YD の `knowledge/programming/tools/vidkit.md` 的なノートは Skills 化すれば 4並列 Claude Code と Cowork で共通利用できる。

### 4. Plugins (Skills + Connectors + Slash + Sub-agents のバンドル)
2026年1月30日に正式リリースされた、Cowork の拡張ユニット。Plugin 一つに「複数の Skills」「データコネクタ」「スラッシュコマンド」「サブエージェント」が同梱される。
社内/個人の専門領域 (例: 「観光地域づくり PM」「ライブ配信講師」) を 1Plugin にまとめれば、Cowork が**役割特化のスペシャリスト**として振る舞う。
Task Hub にこの構造を取り込めば、複数AI環境でも同じスペシャリストを起動できる。

### 5. Steering (進行中の舵取り)
Cowork が長時間タスクを実行している最中に、YD が介入して「方針を変える」「中間成果を確認する」「巻き戻す」操作。
ベストプラクティス:
- 短いゴール記述で始めず、**チェックポイント** (どの粒度で見せてほしいか) を最初に指定する
- 自動承認の範囲 (ファイル書き込み / Web操作 / 外部送信) を **Plugin単位で明示**
- Diligence (4D の D) は人間側に残す ─ 重要判断は必ず Steer

## コース構成 (公式モジュール)

公式: https://anthropic.skilljar.com/introduction-to-claude-cowork

1. **Meet Claude Cowork** — Cowork とは / セットアップ / できること
2. **Hand Claude Cowork Your First Task** — エンドツーエンドで最初のタスクを回す
3. **Make Claude Cowork Yours** — 速く良い結果を出す / Standing Context (Global/Projects) / Skills / Plugins
4. **Use Claude Wherever You Work** — Claude in Chrome / Claude for Microsoft 365
5. **Sharing and Safety in Claude Cowork** — 安全運用 / Plugin の Skills 検証 / チーム共有
6. **Wrap Up and Next Steps** — クイズ

所要: 約30〜45分目安 (Module 5 の安全運用パートが比較的厚い)。修了証あり。

## YDの今やってる仕事との接続点

| プロジェクト | 接続点 | 期待効果 |
|------|--------|---------|
| **Salamat WBSサイト** | Plugin として「Salamat 監修ルール + Vault憲法 + Notion連携」を1パックに | 監修サイクル全体を Cowork に任せて Diligence チェックだけ人間が担う |
| **Arte Grow** | Cowork で「市場調査 → Excel KPI 表 → PowerPoint 提案書」までを Steering 介入で完走 | 営業資料の一次稿が一晩で出る |
| **Lecture Hub** | Skills として「lecture 撮影→FCP autocut→アップロード」運用を登録 | 講師オンボードを Plugin 配布で完了 |
| **Task Hub** | **本コースで一番収穫が大きい接続点。** Task Hub の中核設計に直接示唆。Skills/Plugins 構造を Task Hub のスキーマに合わせ込む | Task Hub が Anthropic の公式語彙と整合 → 移行コスト最小 |
| **vidkit** | dance / autocut / tighten / lecture / tutorial の各モードを Skills として正規化 | Cowork からも CLI からも同じ手順で叩ける |
| **4並列 Claude Code 運用** | Cowork の Steering 概念 = parallel-claude セッションの「介入ポイント設計」と直接対応 | textbook/ vol.2「並列運用の実務」章の語彙を Cowork に揃える |

## コース後にやる小さな練習問題 (3問、答え付き)

### Q1. Chat / Cowork / Claude Code の使い分けを1行ずつで
**A**:
- **Chat**: 「考えて答える」── 議論・要約・原稿の壁打ち
- **Cowork**: 「**手を動かして成果物を作る (GUI/ファイル/Officeアプリ)**」── Excel・PPT・複数ファイル編集
- **Claude Code**: 「**手を動かして成果物を作る (CLI/コードベース)**」── git管理されたプロジェクトの実装・リファクタ

### Q2. YD の Vault における「Global Instructions / Standing Context」の現実装は何で、Cowork の階層ではどこに対応するか
**A**:
- `~/.claude/CLAUDE.md` (グローバル全AI共通指示) → **Cowork の Global Instructions**
- `~/ObsidianVault/CLAUDE.md` (Vault 内ルール) → **Cowork の Project/Folder-level Instructions**
- 起動時必須シーケンス (`00_CLAUDE_BOOT.md`) → **Skills + Slash command 的役割**
- Vault憲法は Cowork のスタンディングコンテキスト概念を**先取りで再発明している**。Plugin 化すれば再利用が効く。

### Q3. 「Cowork に長時間 Excel 自動生成を任せる」場面で、Steering 介入ポイントを3つ設計せよ
**A**:
- (1) **計画提示時**: Claude が立てた計画を実行前にレビュー (列項目・データソース・式の方針)
- (2) **データ取得後・整形前**: 取得した生データの妥当性チェック (件数・欠損・外れ値)
- (3) **最終ファイル生成前**: フォーマット (色・幅・凡例) と式の通り計算結果を1サンプルだけ抽出して確認
- これは 4D の Discernment (出力品質評価) を**実行中に分散させる**設計。終わってから全件確認するより安全で速い。

## 受講ログ

```
受講開始: 
受講完了: 
合計所要時間: 
```

### セクションごとのメモ

### 💡 受講後に埋めるキーコンセプト
(動画後に自分の言葉で書き直す)

### 🛠 Cowork で試したこと
(セットアップして実際に回した最初のタスク)

## ✅ うまく行ったこと
(動画前は空欄。受講後埋める)

## ❌ 詰まったこと
(動画前は空欄。受講後埋める)

## 📋 次回同じことをするときのチェックリスト

事前準備 (動画見る前):
- [ ] Claude デスクトップアプリ (Mac版) を最新版にアップデート
- [ ] Cowork 機能が UI に出ているか確認 (出ていなければ Settings で有効化)
- [ ] Cowork 用の「専用作業フォルダ」を `~/Documents/` 配下に1つ用意 (例: `~/Documents/cowork_sandbox/`)
- [ ] Global Instructions に書きたい内容を `~/.claude/CLAUDE.md` から抜粋して下書き
- [ ] 既存 Connectors の接続状況を一覧で把握 (Notion / Gmail / Calendar / Drive / Figma / Canva / Miro / Zoom)

動画中に手を動かす:
- [ ] Module 1 でセットアップを実機完了
- [ ] Module 2 で「最初のタスク」を実際に投げる (sandbox フォルダで害のない作業)
- [ ] Module 3 で Skills を1つ書く (例: 「Vault 形式のノート作成」)
- [ ] Module 3 で Plugin を1つインストールして挙動を見る
- [ ] Module 5 の安全運用パートはメモ厚めに (4並列運用のリスク管理に直結)

落とし穴・先回り:
- ⚠️ Cowork は**ローカルファイルに書き込む**。Vault や本業プロジェクトを最初の練習対象にしない。sandbox フォルダで試す
- ⚠️ Plugin の Skills は他人が書いたものを実行する場合がある。**Module 5 の「Validating skills for plugins」を必ず受講**してから入れる
- ⚠️ Cowork と Claude Code は**同じプロジェクトで同時に走らせない** (ファイル競合)。YD の 4並列運用は Code 側なので、Cowork は別案件で試す
- ⚠️ 4D の Diligence は Cowork でこそ意識する。「Excel完成しました」を鵜呑みにせず必ず1サンプル検算

## 🔗 関連

- 公式コース: https://anthropic.skilljar.com/introduction-to-claude-cowork
- Cowork 製品ページ: https://www.anthropic.com/product/claude-cowork
- Cowork 公式ヘルプ: https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork
- 全体マップ: [[README]]
- 同日ペア (Day 2): [[05_claude_code_101]] (実体は「Claude 101」)
- 関連深掘り: [[10_introduction_to_agent_skills]] / [[11_introduction_to_subagents]]
- YDの本業接続: [[active_projects]] / [[tools_available]] / [[task_hub]] / [[claude_mistakes]]

## 📚 情報源

- [Introduction to Claude Cowork 公式 (Anthropic Academy)](https://anthropic.skilljar.com/introduction-to-claude-cowork)
- [Claude Cowork 製品ページ](https://www.anthropic.com/product/claude-cowork)
- [Claude Cowork ヘルプセンター](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork)
- [Anthropic brings agentic plug-ins to Cowork (TechCrunch 2026-01-30)](https://techcrunch.com/2026/01/30/anthropic-brings-agentic-plugins-to-cowork/)
- [Anthropic Rolls Out Plugins for Claude Cowork Workflows (Reworked)](https://www.reworked.co/collaboration-productivity/anthropic-adds-plugins-to-claude-cowork/)
- [Class Central: Introduction to Claude Cowork](https://www.classcentral.com/course/anthropic-academy-introduction-to-claude-cowork-536159)

> ⚠️ 推測・捏造ガード: モジュール構成は公式ページからの引用、Plugin の構成 (Skills + Connectors + Slash + Sub-agents) は TechCrunch / Reworked / Anthropic ヘルプの記述ベース。Steering のベストプラクティス3点は YD の 4並列運用知見と公式の「context shapes Claude's plan」記述からの推論を含む。受講後に公式定義と照合して上書き推奨。
