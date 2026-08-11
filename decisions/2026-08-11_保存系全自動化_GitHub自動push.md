# 2026-08-11 保存系全自動化 — 全プロジェクトのGitHub自動push新設

## 状況

- MacBook Pro故障で「push未のローカルコミット消失」(CC-business/youtube bgm等) を経験した直後
- YD指示: 「これからは保存系を全部自動でやってほしい。GitHubにも自動で保存してほしい」
- 既存の自動保存は Vault のみ (`com.yd.vault-autosync`、30分毎 commit+push)

## 決定・実装内容

1. **`com.yd.projects-autosync` 新設** (launchd、30分毎 + ログイン時):
   - スクリプト = `~/.local/bin/projects-autosync.sh`
   - `~/Projects/*` + `~/organize_tool` の全gitリポジトリを走査 → 変更あれば `autosave: <日時>` でcommit → 現在ブランチをpush (remote無し/オフラインは黙ってスキップ)
2. **`~/.claude` の要復元物をVaultへミラー** (同スクリプト内):
   - `CLAUDE.md` / `settings.json` / `keybindings.json` / `skills/` → `ObsidianVault/system_backup/claude/` (vault-autosyncが30分毎にGitHubへpush)
   - **`~/.claude.json` や認証情報は絶対にコピーしない** (シークレット保護)
3. **organize_tool をgit化** → GitHub private `Yitao-Ding/organize-tool` 新規作成・push済 (ログ/キャッシュは.gitignore)

これで「機体が壊れても失われるのは最大30分」の状態になった (Vault / 全プロジェクト / Claude設定・スキル)。

## ✅ うまく行ったこと

- vault-autosync と同型のシンプルなzsh+launchd構成を横展開しただけなので、実装〜稼働確認まで数分
- `gh repo create organize-tool --private --source=. --push` 一発でリポジトリ新設+push完了
- 初回実行でVaultミラー (`system_backup/claude/`) が正しく生成された

## ❌ 詰まったこと

- launchd のデフォルトPATHには `~/.local/bin` が無く、gh (git credential helper) が見つからないとpushが全滅する → スクリプト冒頭で `export PATH="$HOME/.local/bin:..."` を明示して回避
- 該当なし (他は素直に通った)

## 📋 次回同じことをするときのチェックリスト

1. launchdスクリプトは必ず冒頭でPATHを明示 (`~/.local/bin` を含める)
2. 認証情報を含むファイル (`~/.claude.json` 等) はバックアップ対象から除外する
3. 新しいプロジェクトを作ったら: `~/Projects/` 配下に置けば自動でautosync対象になる。ただし **remoteが無いとpushされない** → `gh repo create <name> --private --source=. --push` を初回に1回やること
4. 稼働確認: `launchctl list | grep autosync` (exit 0) + `/tmp/projects-autosync.err` が空
5. autosaveコミットが混ざるのが嫌なリポジトリが出たら、スクリプトの除外リスト方式に改修する (現状は全リポジトリ一律)

## 関連

- [[2026-08-11_MacBookPro故障_MacBookNeo移行_Vault復旧]] — この決定の動機
- `~/.local/bin/projects-autosync.sh` / `~/Library/LaunchAgents/com.yd.projects-autosync.plist`
