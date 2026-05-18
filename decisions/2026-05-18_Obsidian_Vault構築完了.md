---
date: 2026-05-18
type: decision
category: infrastructure
tags: [obsidian, vault, claude, memory]
---

# Obsidian Vault 構築 — Claudeの外部記憶インフラ確立

## 背景

YDは長期間、Claudeとの新セッション開始のたびに「自分が誰で、何を作っていて、どこまで進んでいるか」を毎回ゼロから説明し直す必要があった。

公式メモリ機能だけでは以下4つの限界があり解決できていなかった:

1. **ブラックボックス** — 中身が見えない
2. **判断基準が不明確** — AIが勝手に保存・破棄
3. **他AIと共有できない** — Claudeロックイン
4. **領域分けが難しい** — 撮影・就活・Salamat・学業がごちゃ混ぜ

2026年4月にKarpathyが提唱した「LLM Wikiパターン」と、堀口英剛氏が動画で紹介した「Mistakesフォルダ方式」が、この問題を構造的に解決できる設計だと判明した。

## 選択肢

- **A. メモリ機能をうまく使い続ける** — 中身が見えない、限界突破できない
- **B. Notion をハブにする (既にMCP接続あり)** — ネット必須、他AIから直接読みづらい
- **C. Obsidian + LLM Wiki パターン** — Markdownベース、ローカル完結、Git管理、他AIから読める

## 決定

**C を採用**

サブ決定:
- **規模**: C (全部1Vault、機密分離なし)
- **同期**: Git + GitHub Private リポジトリ (yd-obsidian-vault)
- **iCloud**: 使わない
- **言語**: CLAUDE.md は日本語ベース
- **フル装備**: Obsidian本体・コアプラグイン・コミュニティプラグインまで全部設定
- **構築実行者**: Claude Code (--dangerously-skip-permissions モード)

## 理由

1. **Markdown のみで構成 → 全AIから読み書き可能** (ChatGPT/Gemini/Claude/Cursor)
2. **ローカル完結 → ネット不要、プライバシー確保**
3. **Git管理 → バージョン履歴、GitHub Private でバックアップ**
4. **5分でセットアップ可能な確立されたパターン** (2026年4月公開)
5. **Mistakesフォルダで Claude の教育コストを「投資」化できる**

## 実装内容

### ディレクトリ構造 (確定版)

```
~/ObsidianVault/
├── CLAUDE.md                  # Wiki憲法
├── 00_CLAUDE_BOOT.md          # 起動シーケンス
├── identity/                   # 不変情報 (5ファイル)
├── current_state/              # 「今」の状態 (5ファイル)
├── mistakes/                   # Claudeのミス記録 ★最重要
├── daily/                      # 日次ノート
├── decisions/                  # 意思決定記録
├── knowledge/                  # 領域別ノウハウ (8領域)
├── raw/                        # 生データ
├── wiki/                       # LLM生成統合ノート
├── archive/                    # 完了プロジェクト
└── log.md                      # 操作履歴
```

### 構築実行 (Step 1-13 全完了)

- Step 1-7: Claude Code が自動実行 (ディレクトリ作成、ファイル配置、Obsidian設定)
- Step 8: YDが手動 (Obsidian起動 + Vault選択)
- Step 9: YDが手動 (コミュニティプラグイン3つインストール: dataview, templater-obsidian, calendar)
- Step 10-13: Claude Code が自動実行 (Git init, GitHub Push, エイリアス追加, 動作確認)

### 配置されたミス記録 (mistakes/claude_mistakes.md)

過去会話を徹底調査して見つけた17件の実際のミス事例を初期データとして仕込んだ:

- カテゴリA (ツール使用): 3件 — ローカルファイルアクセス忘却など
- カテゴリB (技術評価): 3件 — 実装難易度の過大評価など
- カテゴリC (コミュニケーション): 4件 — タメ口応答など
- カテゴリD (文脈・記憶): 3件 — 過去会話の検索不足など
- カテゴリE (提案・出力): 3件 — 推奨案を出さないなど
- カテゴリX (メタ): 1件

## 動作確認結果

新しいClaude Codeセッションで「CLAUDE.md を読んで、YDの現状を要約してください」とテスト:

- ✅ 起動シーケンス通り7ファイル並列読み込み
- ✅ 敬語で完璧な要約
- ✅ アイデンティティ・進行中5プロジェクト・完成済2プロジェクト・就活・価値観の軸を網羅
- ✅ 能動的な質問2つ (次の着手、log.md追記) を提示

**完璧に成功。Claudeの記憶喪失問題が構造的に解決された瞬間。**

## 同期インフラ

- **エイリアス追加** (`~/.zshrc`):
  - `vault` — Vault ディレクトリに移動
  - `vsync` — git add + commit + push 一発
  - `vstatus` — git status
  - `vlog` — git log -20

- **GitHub**: https://github.com/Yitao-Ding/yd-obsidian-vault (Private)

## その後・今後の課題

### 確認した重要な事実

**情報の自動連携はされない**。Vault に何を入れるかは「明示的に書き込む」必要がある。

- Claudeデスクトップアプリ会話 → Anthropicサーバー (Vaultには来ない)
- Claude Code → ~/.claude/projects/ (永続じゃない、Vaultには来ない)
- Obsidian Vault → 誰かが書き込まないと空のまま

### 運用フェーズの進め方

- **Phase 1 (今〜2週間)**: 手動運用。会話終わりに「保存しますか?」と確認する習慣を作る
- **Phase 2 (2週間〜1ヶ月)**: 半自動化。明らかに重要なパターンは確認なしで保存
- **Phase 3 (1ヶ月後)**: 真の自動化。Claude Code に hook 設定

詳細は [[knowledge/programming/tools/obsidian_vault]] 参照。

## 関連

- [[active_projects]] — Vault が解決する問題
- [[recent_decisions]] — 直近の意思決定一覧
- [[claude_mistakes]] — このVaultが守るべき教訓
- [[obsidian_vault]] (knowledge) — 運用ルール詳細

## 出典

- Karpathy "LLM Wiki pattern" (2026年4月公開、Twitter/X)
- 堀口英剛 YouTube 動画 — Mistakesフォルダの実運用紹介
- 本セッション (Claude デスクトップアプリ、2026-05-18)
