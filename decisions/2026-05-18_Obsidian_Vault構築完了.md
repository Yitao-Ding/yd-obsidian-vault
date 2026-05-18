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

## 補足: Claude Code 側の実装メモ (2026-05-18 追記)

本構築を実行した Claude Code セッションの実装ノウハウ (権限モード比較・プロンプト設計・強み弱み) は別ファイル [[claude_code]] に分離記録。

実装時に追加で発生した小さな判断:

- **プラグインバイナリ (main.js / styles.css) は Git 追跡対象から除外** — 計約 1.6MB。manifest.json と data.json のみ追跡し、別端末で clone した時は Obsidian で同名プラグインを再インストールする運用にした
- **`.obsidian/appearance.json` も追跡対象から除外** — 端末ごとのフォントサイズ等が混入しないように
- **事前 `settings.json` の `permissions.allow` がほぼ全工程の鍵だった** — `Bash(git:*)` `Bash(mkdir:*)` `Write(~/ObsidianVault/**)` `Edit(~/ObsidianVault/**)` などを許可リストに登録しておいたことで、Step 1-13 のうち権限プロンプトが必要だった場面はほぼ無かった。`--dangerously-skip-permissions` より安全で、通常モードより速い中庸案

## 関連

- [[active_projects]] — Vault が解決する問題
- [[recent_decisions]] — 直近の意思決定一覧
- [[claude_mistakes]] — このVaultが守るべき教訓
- [[obsidian_vault]] (knowledge) — 運用ルール詳細
- [[claude_code]] (knowledge) — Claude Code 側の実装ノウハウ

## 出典

- Karpathy "LLM Wiki pattern" (2026年4月公開、Twitter/X)
- 堀口英剛 YouTube 動画 — Mistakesフォルダの実運用紹介
- 本セッション (Claude デスクトップアプリ、2026-05-18)

---

## ✅ うまく行ったこと

### 設計フェーズ
- **Karpathy + 堀口方式のハイブリッド**: 「LLM Wiki」の構造論と「Mistakesフォルダ」の運用論を組み合わせたら相性が良かった。片方だけだと不完全。
- **YDのプロファイル徹底調査**: 過去会話を `conversation_search` で念入りに掘って、identity/ にプロファイル詳細を仕込んだ。これが「YDの状況を要約して」テストの成功要因。
- **Mistakesフォルダの初期データ17件**: 過去のClaudeのミスをカテゴリ別 (A〜E + X) で17件仕込んだ。これがあるおかげで、次のClaudeが同じ失敗をしない仕組みが初日から稼働。
- **設計書をMarkdown化**: 663行の設計書を Claude Code に渡して、設計通りに構築できた。指示書ファースト方式の成功例。

### 構築フェーズ
- **Claude Code の `--dangerously-skip-permissions`**: Vault構築のような可逆な作業では完全バイパスが超効率的。許可ダイアログで止まらないので一気通貫。
- **Step 8/9 で意図的に止める設計**: Obsidian起動・プラグインインストールという**手動操作が必要な部分だけ YD に渡す**設計が、混乱なく進んだ要因。
- **コアプラグイン JSON 直書き**: Obsidian の `.obsidian/core-plugins.json` を直接書くことで、起動と同時に Graph view 等が有効化された。
- **エイリアス自動追加**: `vault / vsync / vstatus / vlog` を `.zshrc` に書き込むまで自動化した結果、運用が今日から始められる状態に。

### 動作確認
- **新セッションが起動シーケンス通り7ファイル並列読み込み**: 設計通り。これが「Claudeの記憶喪失解決」の証拠。
- **敬語+完璧な現状要約+能動的質問**: テスト応答の質が想定以上に高かった。

## ❌ 詰まったこと

### 認識ずれ
- **「自動で連携される」と勘違いしがち**: Vault作っただけで Claudeアプリの会話が自動で入ると思いがち。実際は明示的に書き込まないと空のまま、と気づくのに時間がかかった (途中で議論)。
- **iCloud に同期する案を一度経由**: 結局 Git のみで決着したが、iCloud + Obsidian の組み合わせには `.obsidian/workspace.json` の同期衝突問題があると知り回避。

### 構築中の小ハマり
- **Claude Code が Step 4で一度ストップ**: 最初の Bash コマンドが許可ダイアログで止まった → `--dangerously-skip-permissions` モードで再起動して解決。
- **Claude Code が `claude_code.md` を「作った」とログに書いたが実体なし**: 自己申告と実態がズレるケースがあると判明。Claudeの作業の検証は実ファイルで確認すべき教訓。
- **Obsidian の日本語UI**: 設定画面の「コミュニティプラグイン」「閲覧」「インストール」「有効化」の日本語訳を、英語UIと対応付けて案内する必要があった。

### 概念的な詰まり
- **「機密Vault」は不要だった**: 最初は別Vault分離案も検討したが、YDの実際の機密度を考えると 1Vault + `_private/` + `.gitignore` で十分と判明。最初から1Vaultでよかった。
- **「フル自動化」は今日できない**: メモリ機能とのリアルタイム同期は技術的に不可能。Phase 1 → 2 → 3 の段階運用に落ち着いた。

## 📋 次回同じことをするときのチェックリスト

### Vault を別の人 (or 別環境) でゼロから構築するとき

#### 事前準備
- [ ] macOS + Homebrew 環境を確認
- [ ] GitHub CLI (`gh`) 認証済み (`gh auth status` で確認)
- [ ] Obsidian アプリのライセンス確認 (個人利用は無料)
- [ ] iCloud と Git の同期戦略を最初に決める (両立しない)
- [ ] CLAUDE.md の言語を決める (日本語 / 英語)

#### 設計フェーズ
- [ ] 対象ユーザーの**過去会話を徹底調査** (identity/ の精度を上げるため)
- [ ] **Mistakesフォルダの初期データを仕込む** (ここを薄くするとVault運用が形骸化する)
- [ ] 規模を決める: 1Vault (推奨) / 機密分離 / 複数Vault
- [ ] 同期戦略を確定: Git + GitHub Private (推奨) / iCloud / 両方

#### 構築フェーズ
- [ ] 指示書を Markdown で先に書く (Claude Code に渡しやすい)
- [ ] Claude Code は `--dangerously-skip-permissions` モードで起動 (可逆作業なら)
- [ ] **手動操作が必要な部分は意図的に止める設計**にする (Step 8/9 のような中断点)
- [ ] `.obsidian/*.json` を直接生成して、起動と同時に環境完成状態にする
- [ ] エイリアスを `.zshrc` に追加して、運用が即始められる状態にする

#### 動作確認
- [ ] **別のターミナル**で新セッションを起動 (エイリアス反映確認も兼ねる)
- [ ] 「私の現状を要約してください」テスト
- [ ] 敬語チェック / 過去ミスを認識してるかチェック / 進行中プロジェクト把握チェック

#### よくある落とし穴
- [ ] **「自動連携される」と誤解しない**: Vault は箱、中身は明示的に書く
- [ ] Claudeの「作りました」報告は実ファイルで検証 (自己申告とズレることあり)
- [ ] iCloud + Obsidian は `.obsidian/workspace.json` の同期衝突に注意
- [ ] Claude Code の許可モード選択を最初に決める (毎回ダイアログで止まると地獄)

#### 運用開始後 (最重要)
- [ ] **Phase 1 (手動運用) を最低2週間続ける**: 何が重要かの判断基準を体感で得る
- [ ] 50件保存達成したら Phase 2 (半自動) を検討
- [ ] Phase 3 (フル自動) は最低1ヶ月後
