# 2026-08-11 MacBook Pro故障 → MacBook Neo移行・Vault復旧

## 状況

- 旧MacBook Pro (M5 Max 36GB, user `ittou`) が故障。最後のVault auto-syncは **2026-08-11 02:21** (直前のlog: Project Agent App TestFlight build 3 提出完了まで記録済み)
- YDが同日MacBook Neoを購入 (user `yitao`)、Claude Code (Fable 5) で環境復旧を開始
- YDの指示「全部勝手にやって、とにかく前の情報を持ってきて」→ Claude Codeが自走で復旧

## 復旧できたもの

1. **Vault本体**: `github.com/Yitao-Ding/yd-obsidian-vault` → `~/ObsidianVault` にクローン。3,432ノート、最終同期2026-08-11 02:21なので**実質ロスほぼゼロ**
2. **iCloud Drive** (`~/Library/Mobile Documents/com~apple~CloudDocs/`): 旧Desktop/Documents/Downloads、workspace(salamat)、動画類、俺の動画素材、`Documents/Claude/Artifacts`(旧アーティファクト9件)、`Documents/organize_tool`
3. **Notion**: Yitao Film案件管理 (yitao-ding@ooo-studio.jp) 無傷
4. **Google Drive / Gmail / Calendar**: MCP経由で接続済み
5. **ポートフォリオサイト**: `~/Projects/yitao-ding.github.io` にクローン済み

## 失われた可能性が高いもの (旧Macディスク救出ができない場合)

- `/Users/ittou/projects/` のうち**GitHub未push分**: CC-businessのローカルコミット (`note-migration` branch、push未と記録あり)、平成たち祭の`_cc_work2/`作業資材、youtube bgm (`~/AI projects/`) など
- `~/.claude/` のセッション履歴・スキル (`fcp-autocut`等)・グローバルCLAUDE.md
- DaVinci Resolve / FCPのプロジェクトファイル (外付けSSD保存分は無事の可能性)

## ✅ うまく行ったこと

- Vaultを毎回auto-sync (git push) していた設計が完全に機能。故障19分前までの記録が残った
- iCloudのDesktop/Documents同期が旧Macの書類をほぼ全部保全していた
- 復旧の探索順: iCloud → MCP(Notion/Drive/Gmail) → GitHub の順で当たりを引けた

## ❌ 詰まったこと

- 新Macは開発ツールゼロ (Homebrew/node/gh未導入)。Homebrewはパスワード必須でYD作業待ち
- GitHub APIの公開repo一覧にvault repoが出ず、YDがURLを直接提示して発覚 (repo可視性の思い込みで探索を打ち切らない)
- Obsidian iCloud vaultフォルダは空で一瞬「ノート全損」と誤認しかけた → 実体はGitHubにあった
- Chrome拡張 (Claude in Chrome) は未インストールで接続不可のまま

## 📋 次回同じことをするときのチェックリスト

1. まずVaultをGitHubからクローン → `00_CLAUDE_BOOT.md` の起動シーケンスに従う
2. iCloud `com~apple~CloudDocs` とMCP (Notion/Drive/Gmail) は機体が壊れても生きている — 最初に見る
3. 「repoが見つからない」で諦めずYDに聞く (private/新規repoはAPI匿名アクセスに出ない)
4. 復旧優先順: Vault → GitHub push済みプロジェクト → iCloud書類 → ローカルのみのプロジェクト (最後は救出前提)
5. 新機体セットアップの定番: Homebrew → gh → gh auth login → private repo一括クローン → ~/.claude 再構築
6. **教訓: push未のローカルコミットは機体故障で消える。「セッション末に必ずpush」をルール化する価値あり**

## 追記 (2026-08-11 午後): 環境復元の実施結果

- ツール導入 (Homebrew不使用、全て `~/.local`): gh 2.97.0 / Node v24.19.0 LTS + npm 11.17 / uv
- organize_tool: iCloudから復元 → **`~/organize_tool`** に配置 (旧 `~/Documents/` はTCC保護でlaunchdから読めず移設)。watchdogをpip --userで導入、launchd `com.yd.organize-watcher` 稼働確認済
- Obsidian: `~/ObsidianVault` をアプリに登録済 (次回起動で開く)
- グローバル `~/.claude/CLAUDE.md` 再構築 (Vault起動シーケンス+スタイルルール)
- Vault自動同期: launchd `com.yd.vault-autosync` (30分毎 commit+push、pushはgh auth後に有効化)
- YD残作業: ① `gh auth login` (private repo復旧+push有効化) ② Claude in Chrome拡張インストール ③ Extreme pro接続確認
