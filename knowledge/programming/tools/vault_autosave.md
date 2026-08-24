---
type: knowledge
domain: programming/tools
created: 2026-08-24
last_updated: 2026-08-24
tags: [vault, autosave, claude-code, launchd, github, inbox]
priority: highest
---

# Vault 完全自動保存 (全経路)

## 概要

2026-08-24、YD 指示「チャットも Code も Cowork も iPhone も、会話から得られる情報は全部全自動で保存」を受けて構築。人が「保存して」と言う場面をゼロにする。実体は `~/.claude/vault-autosave/` の3スクリプトと launchd 2本、Claude Code の SessionEnd フック、GitHub リポ経由の inbox。

## 経路ごとの動き

Claude Code は、transcript (`~/.claude/projects/*/*.jsonl`) を機械的に読む。セッション終了時に SessionEnd フックが背景ジョブを起こし、さらに launchd (`com.yd.vault-autosave.watch`、30分毎) が直近24時間に更新された全 transcript の差分を処理する。差分は raw/chats/claude_code/ に会話ログとして残し (git 対象外)、400文字以上あれば `claude -p --model sonnet` に渡して decisions / knowledge / mistakes / current_state / log.md へ振り分ける。identity/ は直接触らず current_state/open_questions.md に「identity 更新提案」として書く。処理済み行数は state/offsets.json に持つので、開きっぱなしのセッションも30分毎に追記分だけ拾う。抽出が失敗した差分は次回やり直す。

デスクトップアプリのチャットと Cowork は、会話の中で Claude 自身が Desktop Commander (Cowork はデバイスブリッジ経由) で直接 Vault に書く。合図なし、返事のたび、末尾に「📥 Vault保存: ...」1行。ルールは Preferences と Cowork 全体指示に置いてある (アプリ設定なので Claude からは編集不可)。

iPhone のチャットなど Mac に届かない環境は、claude.ai の GitHub コネクタで `Yitao-Ding/yd-obsidian-vault` の `inbox/` に新規ファイルを置く。Mac 側の watch ジョブが pull → `claude -p` で振り分け → `archive/inbox_processed/` へ移す。起動時の読み込みも GitHub 経由で current_focus / active_projects / claude_mistakes の3ファイルを読む。

同期は `vault_sync.sh` (launchd `com.yd.vault-autosave.sync`、10分毎、watch の前後でも実行)。commit → fetch → merge (rebase ではない) → push。`.gitattributes` で log.md / mistakes/*.md / open_questions.md を merge=union にしてあるので、別 Mac との追記衝突は自動解決する。

## 運用

- 状態確認: `launchctl list | grep vault-autosave` (2本、exit 0)、`tail ~/.claude/vault-autosave/logs/autosave.log`、`tail ~/.claude/vault-autosave/logs/sync.log`
- 手動実行: `python3 ~/.claude/vault-autosave/vault_autosave.py watch` / `session <jsonl>` / `inbox`
- 抽出モデル変更: 環境変数 `VAULT_AUTOSAVE_MODEL` (既定 sonnet)。plist の EnvironmentVariables に足す
- 止める: `launchctl bootout gui/$(id -u)/com.yd.vault-autosave.watch` (sync も同様)。フックは `~/.claude/settings.json` の hooks.SessionEnd
- 抽出プロンプトは `vault_autosave.py` の EXTRACT_PROMPT / INBOX_PROMPT。振り分け先や禁止事項を変えたい時はここ
- 別 Mac (MacBook Neo、user yitao) 側の `com.yd.vault-autosync` と併走しても、merge=union と merge 方式の pull で衝突しない想定。ただし同じファイルの同じ行を両方で書き換えると merge 失敗 → sync.log に "manual merge needed" が出る

## ✅ うまく行ったこと

- transcript の構造 (type=user/assistant、content が str か blocks、isSidechain/isMeta で副系統を除外) が素直で、パーサは50行で足りた
- 初回テスト (cc_company の小セッション) で Sonnet が「就職しない決定 vs 8/24 の6社エントリー」の矛盾を見つけ、identity を書き換えずに open_questions へ提案として書いた。抽出プロンプトの「迷ったら保存しない側」「identity は提案止まり」が機能した
- 再帰防止は環境変数 `VAULT_AUTOSAVE=1` + 先頭 user 発言の `[VAULT-AUTOSAVE]` マーカーの二重。`claude -p` 自身の transcript は watch が skip する

## ❌ 詰まったこと

- 初回 sync で `git pull --rebase` が衝突。原因は 2026-08-11 の MacBook Neo 移行で remote に16 commit (Neo 側) が乗り、この MacBook Pro 側は 08-11 以降 push していなかった (故障扱いだったが実際は稼働中)。rebase をやめて merge 方式にし、log.md / mistakes を union で手動マージして解決。以後は merge=union で自動
- macOS に `flock` コマンドが無い。sync.sh は mkdir ロックに変更 (Python 側は fcntl.flock で可)
- Desktop Commander の start_process は 60 秒で MCP 側がタイムアウトする。`claude -p` を含むテストは背景実行 + ログ確認で行う

## 📋 次回同じことをするときのチェックリスト

1. transcript を読む前に `type` の種類を実物で確認する (last-prompt / attachment / file-history-snapshot など会話以外の行が多い)
2. 無人ジョブの `claude -p` は cwd=Vault にすると CLAUDE.md が読まれる。起動シーケンスを省略する旨をプロンプト冒頭で明示しないと毎回 identity 全件を読みに行く
3. 同期は rebase ではなく merge。append-only ファイルは `.gitattributes` で merge=union
4. launchd は PATH を明示 (`/opt/homebrew/bin` が無いと claude / gh が見つからない)
5. 二重保存の防止は「会話中の保存報告 (📥) を見て skip」+「同日 decisions を Glob」の2段。それでも重複したら月次メンテで統合
6. 別 Mac が生きているか不明な時は、remote の decisions を `git show origin/main:...` で読んでから merge 方針を決める
