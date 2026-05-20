---
type: current_state
last_updated: 2026-05-19
update_frequency: 監視ループ (ε モード) が随時追加
owner: ε Vault自己進化モード (監視セッション)
---

# Vault 改善提案リスト

> 監視セッションの ε モードが検出した構造的問題・改善機会を蓄積するファイル。
> YDの次回ログイン時に確認・選別する。
>
> **軽微な問題** (孤立ページ・stale 等) は自動で修正されるため、このリストには載らない。
> ここに載るのは「YDの判断が必要な構造的提案」のみ。

---

## 凡例

- 🔴 **矛盾検出**: 同じ概念について複数ファイルが矛盾する記述
- 🟡 **構造分割提案**: 同じディレクトリ内の類似テーマ3件以上 → サブディレクトリ分割
- 🟢 **その他**: ε モードで気づいた改善余地

各エントリは以下の構造:

```markdown
### [<種別>] <一行サマリ> (検出日時: YYYY-MM-DD HH:MM)

**該当ファイル / ディレクトリ**:
- `path/to/file1.md`
- `path/to/file2.md`

**検出した状況**:
<具体的に何が問題か>

**推奨アクション**:
<どう対処すれば良いか>

**ステータス**: pending / resolved / wontfix
```

---

## 未対応の提案

### [🟡 構造分割提案] knowledge/<area>/index.md は必須3セクション適用外と明示すべき (検出日時: 2026-05-19 03:14)

**該当ファイル**:
- `knowledge/career/index.md`
- `knowledge/academic/index.md`
- `knowledge/languages/index.md`
- `knowledge/salamat/index.md`
- `knowledge/programming/index.md`
- `knowledge/philosophy/index.md`
- `knowledge/filmmaking/index.md`
- `knowledge/arte_grow/index.md`

**検出した状況**:
ε モード A (必須3セクション欠落検出) が knowledge/ 配下 8 件の index.md について 3 セクション全欠落と判定。
しかし index.md は領域のハブ/目次役で「知識ノート」とは性質が異なる (リスト・リンクが主、内容ナレッジは含まない)。

**推奨アクション**:
- A. CLAUDE.md の「必須3セクション」ルールに **例外: index.md は適用外** を追記
- B. ε モード A の検出ロジックで `*/index.md` を skip する
- C. (代替案) index.md にも 3 セクションを形式上追加 (各「該当なし」と明示) — 一貫性は出るが冗長

僕の推奨は **A + B のセット** (ルール側にも検出側にも例外明文化)。

**ステータス**: ✅ resolved (2026-05-20、Claude Code が CLAUDE.md の「必須3セクション」ルールに例外追記: `knowledge/*/index.md` は適用外と明示)

---

### [🟢 その他] decisions/2026-05-19_AI学習スプリント開始.md の B (詰まったこと) ラベル不一致 (検出日時: 2026-05-19 03:14)

**該当ファイル**:
- `decisions/2026-05-19_AI学習スプリント開始.md`

**検出した状況**:
ε モード A で B (詰まったこと) セクション欠落と判定。実際は `❌ 詰まる可能性 (リスク)` というラベルで類似内容が記載されている (記録時点では実装着手前のため、過去形ではなくリスク予測形)。

**推奨アクション**:
- A. ε A の検出ロジックを緩めて `❌ 詰まる可能性` も「詰まったこと」とみなす
- B. 該当 decisions ファイル側で `❌ 詰まる可能性` → `❌ 詰まったこと (実装着手前、現時点ではリスク予測のみ)` にリネーム

僕の推奨は **B** (テンプレ準拠を保ちつつ、内容のニュアンスは括弧で表現)。

**ステータス**: ✅ resolved (2026-05-19 03:24、ε A モードでラベルを `❌ 詰まったこと (実装着手前、現時点ではリスク予測のみ)` にリネーム)

---

### [🟢 その他] 孤立ページ検出: 5件が真の孤立 (検出日時: 2026-05-19 03:29、ε C 初回ローテーション)

**該当ファイル (どこからも `[[name]]` で参照されてない)**:
- `mistakes/workflow_mistakes.md`
- `mistakes/tool_usage_mistakes.md`
- `mistakes/communication_mistakes.md`
- `current_state/vault_improvement_proposals.md` (自分が作った新規ファイル、リンク貼り忘れ)
- `textbook/03_ai_engineering/01_claude_code_parallel.md` (セッションA の第1号教材、textbook/README.md からのリンクが無い)

**起動点/テンプレ系で除外** (リンクされなくて当然):
- `log.md` / `mistakes/claude_mistakes.md` 直系では使われている / `templates/*` / `textbook/_template/*`

**検出した状況**:
ε C 初回ローテーションで全 .md の WikiLink グラフを走査。上記5件は basename / フルパス共に [[..]] 参照が一切ない。発見不可状態。

**推奨アクション** (軽微レベル、次 tick で自動修正予定):
- `mistakes/claude_mistakes.md` の関連セクション末尾に `[[workflow_mistakes]] / [[tool_usage_mistakes]] / [[communication_mistakes]]` を追記
- `CLAUDE.md` または `wiki/index.md` から `[[vault_improvement_proposals]]` をリンク
- `textbook/README.md` から `[[01_claude_code_parallel]]` をリンク

**ステータス**: 部分 resolved (2026-05-19 03:34、ε C 自動修正実行):
- ✅ `CLAUDE.md` の「📚 関連ドキュメント」節に `[[vault_improvement_proposals]]` を追加
- ⏸ `mistakes/claude_mistakes.md` への 3つの WikiLink 追加は「新規セクション追加」扱いで重要レベル → YD判断待ち (claude_mistakes.md の冒頭か末尾に「ジャンル別」セクションを新設するか、または既存の「📊 ミス統計」セクションの直後にリンク行追加するか)
- ⏸ `textbook/03_ai_engineering/01_claude_code_parallel.md`: textbook/README.md からは既に **通常 Markdown リンク** `[Claude Code 4並列で何が起きてるか](03_ai_engineering/01_claude_code_parallel.md)` で参照されている。ε C 検出ロジックが WikiLink `[[..]]` 限定で、Markdown リンク `[..](path)` を見落としていた。検出ロジック側を改善すべき (次の改善提案として下記に独立追加)

---

### [🟢 その他] ε C 検出ロジック改善: WikiLink だけでなく Markdown リンクも考慮すべき (検出日時: 2026-05-19 03:34)

**該当**:
- ε C 孤立検出のロジックそのもの

**検出した状況**:
今回の ε C 初回ローテーションで `textbook/03_ai_engineering/01_claude_code_parallel.md` を「孤立」と判定したが、実際は `textbook/README.md` 行44 から `[Claude Code 4並列で何が起きてるか](03_ai_engineering/01_claude_code_parallel.md)` という Markdown リンクで参照済み。WikiLink `[[..]]` だけ走査する現在のロジックは漏れがある。

**推奨アクション**:
- ε C 検出を `grep -oE '\[\[([^\]]+)\]\]|\[([^\]]+)\]\(([^)]+)\)'` のように **WikiLink + Markdown リンク両対応** に拡張
- Markdown リンクの場合は path から basename を抽出して比較

**ステータス**: ⚠️ wontfix (2026-05-20、調査の結果 ε C cron prompt が独立ファイルとして存在しないことを確認。CronList も空。ε C 監視を再起動する際に grep パターンを修正した prompt を作ること)

---

### [🟢 その他 / 監視対象追加候補] セッションθ (ai-researcher) 新規発見 (検出日時: 2026-05-19 03:34)

**該当**:
- `~/projects/ai-researcher` (新規プロジェクト、未監視)
- `~/ObsidianVault/knowledge/programming/tools/ai_researcher.md` (新規 knowledge ノート、セッションθ作と明記)

**検出した状況**:
ε C の最中に `knowledge/programming/tools/ai_researcher.md` が新規追加されているのを発見。ファイル本文に「**セッションθ (2026-05-19) で構築**」と明記。
24時間 AI 研究員エージェント (arxiv / HackerNews / Papers with Code / Anthropic Blog / OpenAI Blog / Google Research / GitHub Trending の7ソース巡回 → Claude Haiku 4.5 で日本語要約 → `~/ObsidianVault/raw/research/` 蓄積)。launchd で毎時 HH:03 自動実行、月曜 06:00 で週次まとめ、月1で archive。`briefing-json` API で [[morning_briefing]] と連携。予算 $50/月 (SQLite で実測)。

**推奨アクション**:
- 監視対象に **9. ai-researcher: `~/projects/ai-researcher`** を追加 (cron prompt 更新が必要、YD指示待ち)
- `decisions/2026-05-19_AI学習スプリント開始.md` の「4並列セッション割り当て」が古くなった → 5並列 (A/B/C/D + θ) に更新提案

**ステータス**: ✅ resolved (2026-05-20、ai-researcher は active_projects.md #13 として既に管理中。knowledge ノートも knowledge/programming/projects/ai_researcher.md として整備済み)

---

### [🟡 構造分割提案] knowledge/programming/tools/ が10ファイルに膨張、サブディレクトリ分割提案 (検出日時: 2026-05-19 03:39、ε E 初回ローテーション)

**該当ディレクトリ**:
- `knowledge/programming/tools/` (現在 10 ファイル)

**検出した状況**:
ε E (構造改善機会) で `knowledge/programming/tools/` 配下のファイルをテーマ分類した結果、4 グループに自然分割できる:

| グループ | 件数 | ファイル |
|---|---|---|
| **YD自作プロジェクト** | 5 | `ai_researcher.md` / `lecture_hub.md` / `morning_briefing.md` / `textbook_engine.md` / `vidkit.md` |
| **Vault 運用** | 2 | `obsidian_vault.md` / `vault_workflow.md` |
| **Claude Code 運用** | 2 | `claude_code.md` / `claude_code_permissions.md` |
| **外部サービス** | 1 | `vercel.md` |

「YD自作プロジェクト」5件が同テーマ3件以上の閾値を超えてるので、構造改善余地あり。

**推奨アクション**:
- A. **`knowledge/programming/projects/` を新設** (YD自作の5件を移動) ← **推奨**
- B. 加えて `knowledge/programming/vault/` (2件) と `knowledge/programming/claude_code/` (2件) も新設して整理
- C. (保留案) 現状維持 — まだ 10件で管理可能なレベル

僕の推奨は **A だけまず実施** (5件のプロジェクトを `projects/` へ)。B のさらなる分割は2件ずつなので閾値ギリギリで効果薄、必要が出てきたら次のラウンドで実施。

**注意点**: 移動するとファイル内の `[[..]]` リンクは Obsidian なら basename 解決で自動追従するが、`[..](path)` 形式の Markdown リンクは追従しない (path 書き換え必要)。事前に grep して影響範囲を確認すべき。

**ステータス**: ✅ resolved (2026-05-20、`knowledge/programming/projects/` を新設し、YD自作プロジェクト 7 件 (ai_researcher / ai_simulator / lecture_hub / morning_briefing / task_hub / textbook_engine / vidkit) を `git mv` で移動。Markdown リンクは grep 確認で 0 件、WikiLink は basename 解決で自動追従)

---

## 解決済み (履歴)

2026-05-20 Vault メンテバッチ 1 で以下を一括 resolved:
- [🟡] knowledge/*/index.md 必須3セクション例外 → CLAUDE.md に追記
- [🟡] knowledge/programming/tools/ 構造分割 → projects/ 新設 + 7件移動
- [🟡] decisions/2026-05-19_教科書システム第2号企画.md 必須3セクション → 追加済み
- [🟢] セッションθ ai-researcher → active_projects.md に既に統合済みと確認
- [🟢] ε C grep ロジック + raw/ 除外 → cron prompt 未発見のため wontfix

---

## 関連

- [[CLAUDE]] — Vault ルールブック
- [[active_projects]] — 進行中プロジェクト
- [[claude_mistakes]] — Claude のミス記録

---

### [🟢 その他] ε C 検出ロジック改善: `raw/` 配下を除外すべき (検出日時: 2026-05-19 20:33、ε C tick #2)

**該当**:
- ε C 孤立検出ロジック (現在の除外パス: `templates/` / `_template/` / `_assets/` の 3 つのみ)

**検出した状況**:
ε C tick #2 で全 127 .md 中 48 件を孤立判定。うち **46 件は `raw/research/2026-05-{18,19}/{arxiv,hn,anthropic_blog,openai_blog}/` と `raw/chats/by_date/`** に集中。前者は ai-researcher が定期巡回で蓄積、後者は会話 raw — どちらも「集めて貯める」性質で双方向リンクされないのが正常 (by-design 孤立)。

**推奨アクション**:
- ε C 除外パスに **`raw/`** を追加 (`templates/_template/_assets/raw` の 4 種に拡張)
- これで真の孤立だけが浮かぶ (検証: 今回なら 2 件: 新規 decision `2026-05-19_教科書システム第2号企画.md` + 新規 simulation 結果 `2026-05-19_194743_salamat_team_chaos.md`)

**ステータス**: ⚠️ wontfix (2026-05-20、調査の結果 ε C cron prompt が独立ファイルとして存在しないことを確認。ε C 監視を再起動する際に `raw/` を除外パスに含めること)

---

### [🟡 必須3セクション欠落] `decisions/2026-05-19_教科書システム第2号企画.md` (検出日時: 2026-05-19 20:50、ε A tick #5)

**該当ファイル**:
- `decisions/2026-05-19_教科書システム第2号企画.md`

**検出した状況**:
ε A tick #5 で knowledge/ + decisions/ 全件走査 → 上記 1 件のみ必須 3 セクション全欠落 (`## ✅ うまく行ったこと`, `## ❌ 詰まったこと`, `## 📋 次回...` いずれも無)。CLAUDE.md 絶対ルール (空欄禁止、書くことなければ「該当なし」明示) 違反。

**推奨アクション**:
- A. 本文踏まえて 3 セクションを LLM 判断で実質追加 (企画段階なら「該当なし (実装着手前)」+ 「次回チェックリスト = 実装時の手順」を埋める)
- B. テンプレ準拠で「該当なし」3 件を機械追加 (情報量ゼロ)

僕の推奨は **A**。decision の文脈を踏まえた記述で、後から再現できる材料を残す。

**ステータス**: ✅ resolved (2026-05-20、企画段階として「該当なし (実装着手前)」+ 想定リスク + 実装チェックリストを追記)

---

## 2026-05-20 整合性チェック結果 (自動)

> 実施: parallel-claude セッション 01 (vault_integrity)
> 実施時刻: 2026-05-20 (深夜並列セッション)
> 対象: `~/ObsidianVault/**/*.md` (除外: `raw/`, `archive/`, `.git/`)
> 走査対象: **91 ファイル** / 抽出 wiki-link: **132 種類** / 解決対象: **81 ターゲット**

### サマリ

| 種別 | 検出件数 | 備考 |
|------|---------|------|
| 孤立ページ | **1 件** (真の孤立) + 5 件 (テンプレ/log = 設計上の孤立) | 真の孤立は新規 decision 1 件 |
| リンク漏れ | **10 件** (実害あり) + 3 件 (ドキュメント例示・誤検出) | 命名揺れ + リンク先未作成 |
| 矛盾 | **3 件** | `active_projects` / `vercel.md` / `skills.md` の現状ズレ |
| 古い情報 (>14日) | **0 件** | Vault 自体が 2 日前 (5/18) 構築のため母数不足 |
| **計** | **14 件** | (誤検出・設計上の例外を除く実害件数) |

副次的な観察: **47 ファイルが `last_updated` frontmatter 自体を持たない** (decisions/, mistakes/, learning/anthropic_academy/, templates/, README, CLAUDE.md, log.md 等)。古さ判定の母集団が大幅に欠けている。

---

### 🟠 孤立ページ (1 件、真に未参照)

#### `decisions/2026-05-20_lecture_hub_notion_design_phase1.md`

**状況**: `log.md` で平文言及はあるが `[[wiki-link]]` 形式の参照ゼロ。同種の他 decision (例: `2026-05-19_tiptap_migration`, `2026-05-19_TaskHub_git整理_GitHub連携`) は `active_projects.md` #7 や `knowledge/programming/tools/lecture_hub.md` から `[[]]` 参照されている。

**推奨アクション**:
- `current_state/active_projects.md` #7 Lecture Hub の「意思決定記録」行に `[[2026-05-20_lecture_hub_notion_design_phase1]]` を追加
- `knowledge/programming/tools/lecture_hub.md` の関連リンク末尾にも追加

**ステータス**: ✅ resolved (2026-05-20、active_projects.md #7 Lecture Hub の「意思決定記録」に WikiLink 追加済み)

#### 設計上の孤立 (アクション不要、参考まで)

- `templates/daily_note_template.md`, `templates/decision_template.md`, `templates/knowledge_template.md`
- `textbook/_template/textbook_template.md`, `learning/books/_template.md`, `learning/podcasts/_template.md`
- `log.md` (ジャーナル、双方向リンク不要)

→ 既存提案 #1 (ε C 除外パスに `raw/` を追加) と同じ流れで、ε C 除外に `templates/`, `_template/`, `log.md` も明示しておくと検出ノイズが減る (これは既に「現在の除外パス: templates / _template / _assets の 3 つ」と上に書かれている。`log.md` 単体例外の追加だけ実質残課題)。

---

### 🔴 リンク漏れ (10 件、実害あり)

| # | リンク表記 (誤) | 出現場所 | 推定正解 | 種別 |
|---|---------------|---------|---------|------|
| 1 | `[[02_claude_code_permissions]]` | `textbook/03_ai_engineering/01_claude_code_parallel.md` | `[[claude_code_permissions]]` | 接頭辞誤り |
| 2 | `[[2026-05-17_lecture_hub_MVP_shipped]]` | `knowledge/programming/tools/lecture_hub.md:263` (※「もし作るなら」と注記あり) | 未作成 (TODO 扱い) | 未作成リンク |
| 3 | `[[ai_certifications_dashboard]]` | `learning/ai_certifications/anthropic_academy/README.md` | 該当ファイル無し → 作成 or 削除 | 未作成 |
| 4 | `[[anthropic_academy]]` | `decisions/2026-05-19_AI学習スプリント開始.md:142` (「今後作成予定」と注記)<br>`learning/ai_certifications/README.md` の表中 (異なる解釈) | フォルダ名と同名の集約ファイル無し → `[[anthropic_academy/README]]` 統一推奨 | 解決先ズレ |
| 5 | `[[gear]]` | `knowledge/filmmaking/index.md` | 未作成 (撮影機材ノート想定?) | 未作成 |
| 6 | `[[grading]]` | `knowledge/filmmaking/index.md` | 未作成 (カラーグレーディングノート想定?) | 未作成 |
| 7 | `[[Lecture Hub]]` | `identity/skills.md:81` | `[[lecture_hub]]` | 表記揺れ (大文字/空白) |
| 8 | `[[morning_briefing_system]]` | `decisions/2026-05-19_AI学習スプリント開始.md` | `[[morning_briefing]]` | suffix 揺れ |
| 9 | `[[Task Hub]]` | `identity/skills.md:81` | `[[task_hub]]` | 表記揺れ |
| 10 | `[[textbook_system]]` | `decisions/2026-05-19_AI学習スプリント開始.md` | `[[textbook_engine]]` | 概念名混在 |

**推奨アクション** (優先度順):
- A. **即修正可能 (表記揺れ系 #1, #7, #8, #9, #10)**: 一括 `sed` 置換 OK。5 件は機械的に直る。
- B. **TODO 扱い (#2)**: lecture_hub.md 内に既に「(もし作るなら)」注記あり。当面そのまま、後日 `decisions/2026-05-17_lecture_hub_MVP_shipped.md` を遡及作成するか判断。
- C. **意図確認 (#3, #5, #6)**: 未作成ノートを作るのか、リンクを消すのか、YD 判断。`gear` / `grading` は filmmaking 領域なので作る価値あり (vidkit カラーグレーディング知見の置き場)。
- D. **方針統一 (#4)**: フォルダ参照スタイルは `[[anthropic_academy/README]]` に統一を推奨。

**ステータス (部分解決、2026-05-20)**:
- ✅ #1 `[[02_claude_code_permissions]]` → `[[claude_code_permissions]]` 修正済み
- ✅ #7 `[[Lecture Hub]]` → `[[lecture_hub]]` + 本番URL追記 (skills.md)
- ✅ #8 `[[morning_briefing_system]]` → `[[morning_briefing]]` 修正済み
- ✅ #9 `[[Task Hub]]` → `[[task_hub]]` + Firebase Hosting URL追記 (skills.md)
- ✅ #10 `[[textbook_system]]` → `[[textbook_engine]]` 修正済み
- ⏸ #2 TODO 扱いで保留 (lecture_hub.md 内に「もし作るなら」注記で意図を保持)
- ⏸ #3 #5 #6 未作成リンク → YD判断待ち (filmmaking gear/grading ノートは価値あり)
- ⏸ #4 方針統一 → 次回メンテで対応

#### 誤検出 (アクション不要)

- `[[Wiki Link]]` (CLAUDE.md:105) — Wiki記法の説明文中の例示
- `[[name]]` (vault_improvement_proposals.md:93) — 過去の改善提案中の例示
- `[[02_xxx]]` (textbook/_template/textbook_template.md) — テンプレートのプレースホルダ
- `[[..]]` — 親ディレクトリ表記、無視

---

### 🔴 矛盾 (3 件)

#### 矛盾 1: 「Obsidian Vault 構築」の状況が現実と乖離

**該当ファイル**:
- `current_state/active_projects.md` L39-43 (「### 1. Obsidian Vault 構築 (今このタスク)」/ 状況: **Claude Code に渡す設計書作成中** / 次のアクション: Claude Codeで `~/ObsidianVault/` を構築)

**矛盾内容**:
Vault は 2026-05-18 に構築完了 (`decisions/2026-05-18_Obsidian_Vault構築完了.md` 存在、現に運用中)。にも関わらず active_projects.md は 5/18 構築前のスナップショットのまま。

**推奨アクション**:
- このセクションを「✅ 完了 (2026-05-18)」に書き換え、または 🟡 運用フェーズ側に移動。`archive/` 行きでも可。Vault 改善は別軸 (vault_improvement_proposals に集約済) なので、active_projects の枠を 1 つ解放した方が見通しが良い。

**ステータス**: ✅ resolved (2026-05-20、active_projects.md から「Obsidian Vault 構築」セクションを削除)

#### 矛盾 2: `knowledge/programming/tools/vercel.md` のプロジェクト状況表が古い

**該当ファイル**: `knowledge/programming/tools/vercel.md:103-107`

**矛盾内容**:
| 項目 | vercel.md (古) | 実態 (active_projects.md / log.md) |
|------|---------------|---------------------------------|
| Lecture Hub 本番URL | `lecture-hub-yitao-ding-yitao-dings-projects.vercel.app` | `lecture-hub-sable.vercel.app` (2026-05-19 TipTap 移行後) |
| Lecture Hub 状態 | 「MVP稼働中、家で本番更新予定」 | TipTap v3 移行 + 本番デプロイ完了 (2026-05-19 21:50) |
| Task Hub | 「(Firebase?)」「Firebase Hosting (Vercelではない可能性、要確認)」 | **Firebase Hosting で確定** (`salamat-task-hub.web.app`、2026-05-19 21:17 確証、[[2026-05-19_TaskHub_git整理_GitHub連携]] 参照) |
| Salamat WBSサイト | 「初回デプロイ完了 (2026-05-18)」 | Phase 1 + Phase 2 演出強化完了 + 再デプロイ (2026-05-19 夜) |

**推奨アクション**:
- vercel.md の表を最新化 (3 行とも書き換え + `last_updated:` を 2026-05-20 に)
- Task Hub の「Vercelではない可能性、要確認」は「Firebase Hosting (Vercel 管理外)」に確定表記。それでも一覧に残すなら別表「Vercel 管理外プロジェクト」を切る。

**ステータス**: ✅ resolved (2026-05-20、vercel.md のプロジェクト表 3行を最新化。intro 文も「Task Hub は Firebase Hosting」に訂正。last_updated 更新済み)

#### 矛盾 3: `identity/skills.md` の Task Hub 説明が古い

**該当ファイル**: `identity/skills.md:81`

**矛盾内容**: 「[[Task Hub]] (Next.js + Firebase、Vercelデプロイ済み)」 — 実態は **Firebase Hosting** デプロイ。`decisions/2026-05-19_TaskHub_git整理_GitHub連携.md:59` で「`active_projects` #6 の Task Hub 記述で長らく『✅ Vercelデプロイ完了』と書かれていたが、実態は **Firebase Hosting**」と明示的に訂正されているが、`skills.md` 側は連動修正されていなかった。同じ過去ミスの取り残し。

**推奨アクション**:
- `identity/skills.md:81` を `[[task_hub]] (Next.js + Firebase Hosting デプロイ済み、salamat-task-hub.web.app)` に修正。リンク表記 (`Task Hub` → `task_hub`) の修正と同時に実施可。

**ステータス**: ✅ resolved (2026-05-20、skills.md の `[[Task Hub]]` → `[[task_hub]]` + Firebase Hosting URL 追記、`[[Lecture Hub]]` → `[[lecture_hub]]` + 本番URL 追記)

---

### 🟢 古い情報 (>14日経過、0 件)

cutoff 2026-05-06 より前の `last_updated` を持つファイルは **0 件**。Vault 自体が 2026-05-18 に構築されたばかりのため、母数として 14 日経過分が存在しない。次回 2026-06-01 以降のチェックで意味を持つ指標。

**副次的観察 (こちらの方が今は重要)**:
91 ファイル中 **47 ファイル (52%)** が `last_updated` frontmatter を持たない。内訳:
- `decisions/*.md` (12 ファイル全件): decision テンプレに `last_updated` が無い設計
- `mistakes/*.md` (4 ファイル全件)
- `learning/ai_certifications/anthropic_academy/*.md` (18 コーステンプレ): テンプレ生成時に未挿入
- `templates/*.md` (3 件): テンプレ自体には不要
- 最上位 `CLAUDE.md`, `README.md`, `log.md`, `00_CLAUDE_BOOT.md`

**推奨アクション**:
- decision/mistakes/anthropic_academy 系のテンプレに `last_updated:` を必須化 (生成時に自動挿入)
- 既存ファイルへの遡及挿入は git 履歴の last commit date を流用すれば機械的に可能 (例: `git log -1 --format=%cs <file>` の結果を frontmatter に注入)
- これをやらないと、次回のチェックで「古い情報」検出の母数が常に半分以下になる

