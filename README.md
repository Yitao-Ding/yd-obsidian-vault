# YD's Obsidian Vault

> YDのための「Claudeの外部記憶」を実現するObsidian Vault
> Karpathy LLM Wikiパターン + 堀口英剛氏のMistakes方式を採用
>
> 構築日: 2026-05-18

## 🎯 このVaultが解く問題

新セッションのClaudeに、毎回ゼロから「自分が誰で、何を作っていて、どこまで進んでいるか」を説明し直す手間を無くす。

## 📁 ディレクトリ構成

```
~/ObsidianVault/
├── CLAUDE.md                       # Wiki全体のルールブック (憲法)
├── 00_CLAUDE_BOOT.md               # 起動手順
├── README.md                       # このファイル
├── identity/                       # YDの不変情報
├── current_state/                  # 「今」の状態 (動的)
├── mistakes/                       # Claudeの過去のミス記録 ★最重要
├── daily/                          # 日々の作業ログ
├── decisions/                      # 意思決定の記録
├── knowledge/                      # 領域別ノウハウ
├── raw/                            # 生データ (vidkit出力、論文等)
├── wiki/                           # LLM生成の統合ノート
├── archive/                        # 完了プロジェクト
└── log.md                          # 操作履歴
```

## 🚀 新しいClaudeセッションの始め方

### Claude Code の場合

```bash
cd ~/ObsidianVault
claude
```

そして:

```
> CLAUDE.md を読んでから、YDの現状を要約して
```

### Claude デスクトップアプリの場合

Desktop Commander 経由で:

```
> /Users/ittou/ObsidianVault/CLAUDE.md を読んで、必要なファイルも読み込んで、
> 現状を要約して
```

### 他AI (ChatGPT/Gemini) の場合

このVaultをzipしてアップロード or 主要ファイルを直接コピペ

## 📝 運用ルール

- 新しい意思決定 → `decisions/YYYY-MM-DD_<内容>.md`
- 新しい知識 → `knowledge/<領域>/<名称>.md`
- Claudeのミス → `mistakes/claude_mistakes.md`
- 進捗変化 → `current_state/active_projects.md` を更新
- 毎回 `log.md` に1行追記

## 🔄 Git運用

```bash
# 変更をコミット
cd ~/ObsidianVault
git add .
git commit -m "Update: <内容>"
git push
```

## 🔐 機密情報

機密情報は別Vault (`~/ObsidianVault-confidential/`) に隔離。
このVaultはGitHub Private に push 可能なレベルに保つ。

## 📚 詳細ドキュメント

- 設計の経緯: `~/Downloads/obsidian-vault-setup/01_設計書.md`
- 構築指示: `~/Downloads/obsidian-vault-setup/99_CC_実行指示.md`
