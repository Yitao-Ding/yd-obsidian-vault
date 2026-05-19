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

**ステータス**: pending

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

**ステータス**: pending (次の ε C ローテーション tick で対応予定、cron prompt の検出ロジック修正は YD判断後)

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

**ステータス**: pending (YD判断待ち、監視対象追加するか)

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

**ステータス**: pending (YD判断待ち、大構造変更扱い)

---

## 解決済み (履歴)

(空)

---

## 関連

- [[CLAUDE]] — Vault ルールブック
- [[active_projects]] — 進行中プロジェクト
- [[claude_mistakes]] — Claude のミス記録
