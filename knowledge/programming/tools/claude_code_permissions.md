---
type: knowledge
domain: programming/tools
created: 2026-05-19
last_updated: 2026-05-19
tags: [claude-code, permissions, workflow, automation]
priority: high
---

# Claude Code Permissions — 全プロジェクト自動承認システム

> 2026-05-19 確立。「Push/Deploy 以外は全自動承認」を全プロジェクト共通で実現する設計。
> YDの「1日中認証ダイアログ消すのが面倒」問題を根本解決。

---

## 🎯 設計思想

- **デフォルトは自動承認** (Edit / Write / 大半の Bash コマンド)
- **危険な操作だけ確認** (`git push`, `vercel deploy`, `npm publish`, `sudo`)
- **完全禁止は使わない** (柔軟性を保つ)
- **グローバル設定で全プロジェクトに一括適用** (個別設定の手間ゼロ)

---

## 📂 ファイル構成

### グローバル: `~/.claude/settings.json`
全プロジェクトに自動適用される基本設定。
- `defaultMode: "acceptEdits"` — ファイル編集系のダイアログを最初からスキップ
- `allow` リスト — 自動承認するコマンド (pnpm, npm, git add/commit, ls, cat, ffmpeg, vercel ls/env など)
- `ask` リスト — 確認プロンプトが出るコマンド (git push, vercel deploy, npm publish, sudo)

### グローバル: `~/.claude/settings.local.json`
個人専用、git に commit されない (`.gitignore` 対象)。
過去に許可した個別コマンドの履歴アーカイブ。Claude Code が自動で追記する。
**push/deploy 系は除外済み** (2026-05-19 整理)。

### プロジェクト個別: `<project>/.claude/settings.json`
必要なときだけ `claude-init` コマンドで生成。
グローバル設定の allow を継承しつつ、プロジェクト固有の ask ルールを追加できる。

---

## 🛠 設定の優先順位

Claude Code の権限ルールは以下の順で評価される (上が強い):

1. プロジェクト個別の `.claude/settings.local.json`
2. プロジェクト個別の `.claude/settings.json`
3. グローバル `~/.claude/settings.local.json`
4. グローバル `~/.claude/settings.json`

`deny` > `ask` > `allow` の優先順なので、`ask` に入れたものは下位の `allow` を上書きする。

---

## ⚡ 使い方

### 既存プロジェクトで Claude Code 起動するだけ

```bash
cd ~/projects/vidkit
claude
```

→ グローバル設定が自動適用される。何もしなくていい。

### 新規プロジェクトで個別設定を入れたい時

```bash
cd ~/projects/new-project
claude-init
```

→ `.claude/settings.json` がプロジェクトに生成される。
チームで共有したい時は `git add .claude/settings.json && git commit`。

### `claude-init` の中身 (`.zshrc` に定義)

任意のディレクトリで実行可能なシェル関数。
既存ファイルがあると上書き確認を出す。

---

## 🚨 ask に入っているコマンド (確認プロンプトが出る)

これらは「実行前に YD に聞いてから動く」コマンド:

- `git push` / `git push:*`
- `vercel` / `vercel deploy` / `vercel --prod` / `vercel --yes`
- `npm publish` / `pnpm publish` / `yarn publish`
- `gh release create`
- `sudo`

理由: 外部に対して不可逆な変更を起こすコマンドだから。
ファイル削除 (`rm -rf` 等) は ask にしてない (作業中に頻繁に発生するため)。

---

## 🔄 グローバル `~/.claude/settings.json` の主要 allow

ファイル系: `Edit`, `Write`, `MultiEdit`, `Read`, `Glob`, `Grep`

パッケージマネージャ: `pnpm:*`, `npm:*`, `npx:*`, `yarn:*`, `bun:*`, `pip:*`, `uv:*`

Git (push 以外): `add`, `commit`, `status`, `diff`, `log`, `branch`, `checkout`, `switch`, `restore`, `reset`, `stash`, `pull`, `fetch`, `merge`, `rebase`, `rm`, `mv`, `tag`

GitHub CLI (読み取り系のみ): `gh repo view/list`, `gh pr list/view`, `gh issue list/view`, `gh auth status`

シェル基本: `ls`, `cat`, `head`, `tail`, `grep`, `rg`, `find`, `mkdir`, `rm`, `mv`, `cp`, `touch`, `chmod`, `tar`, `zip`, `curl`, `wget`, `jq`, `sed`, `awk`

メディア: `ffmpeg`, `ffprobe`, `qlmanage`, `sips`

Vercel (デプロイ以外): `vercel ls`, `vercel inspect`, `vercel link`, `vercel env`, `vercel domains`, `vercel project`, `vercel whoami`

WebFetch (主要ドメイン): github.com, anthropic.com, nextjs.org, react.dev, tailwindcss.com, vercel.com など

---

## ✅ うまく行ったこと

- **`defaultMode: "acceptEdits"` の発見**: これだけでファイル編集系の全プロンプトが消える。`--dangerously-skip-permissions` を毎回付けるより安全で楽
- **2層構成 (グローバル + プロジェクト個別)**: 通常はグローバルだけで足りる。チーム共有が必要なときだけプロジェクト個別 `.claude/settings.json` を git commit する選択肢が残る
- **`claude-init` 関数化**: 新規プロジェクトでもコマンド1発で同じ環境が再現できる
- **`settings.local.json` のアーカイブ価値を保持**: 過去に許可したコマンド履歴 (130行) を消さず、push/deploy 系だけ除外。Claude Code が次回以降の細かいコマンド許可をスキップできる

## ❌ 詰まったこと

- **`settings.local.json` の allow が下位を上書きする勘違い**: 最初 `settings.json` の ask を追加するだけで足りると思ったが、`settings.local.json` 側で `git push *` と `vercel --prod --yes` が既に allow 済みだったため、それを除外する必要があった。優先順位の確認重要
- **glob パターンの細かい挙動**: `Bash(vercel:*)` は `vercel ls` も `vercel deploy` も両方マッチする。ask 側に細かく書く必要があった (vercel ls は allow に明示)
- **`--dangerously-skip-permissions` の罠**: グローバルで `skipDangerousModePermissionPrompt: true` が既に入っていた。これは「危険モード起動時の警告を消す」設定で、通常モードの権限ダイアログをスキップする設定ではない。混同しやすい
- **CLAUDE.md の `mistakes/A-1` 案件**: 「`settings.local.json` 中身を見ずに上書きしたら過去の許可履歴を全部失う」という潜在リスクがあった。Read してから判断するのは正解だった

## 📋 次回同じことをするときのチェックリスト

### 新規 Mac セットアップ時

- [ ] `~/.claude/settings.json` をこのドキュメントの構成で作成
- [ ] `~/.claude/settings.local.json` は触らない (Claude Code が自動生成)
- [ ] `.zshrc` に `claude-init` 関数を追加
- [ ] `source ~/.zshrc` で読み込み

### 新規プロジェクト立ち上げ時

- [ ] 普段はグローバル設定だけで OK (何もしない)
- [ ] チーム共有したい / プロジェクト固有の ask を追加したい時のみ `claude-init` を実行
- [ ] `.claude/settings.json` を git commit するなら `.gitignore` から外す
- [ ] `.claude/settings.local.json` は必ず `.gitignore` に入れる (個人情報を含む可能性)

### 既存プロジェクトの権限見直し時

- [ ] `cat .claude/settings.json` で現状確認
- [ ] `cat ~/.claude/settings.local.json | grep -i "push\|deploy\|publish"` で危険コマンドが allow に入ってないか確認
- [ ] 入っていれば ask に移動するか削除

### よくある落とし穴

- [ ] `claude --dangerously-skip-permissions` を打つと ask も全部無視される。普段使い禁止
- [ ] `settings.local.json` を手動編集する時は Claude Code を一旦終了してから (起動中だと書き戻される可能性)
- [ ] `defaultMode: "acceptEdits"` は強力。共有 PC や本番環境では使わない
- [ ] Bash の glob パターンは細かく書かないと意図しないコマンドまでマッチする

---

## 🔗 関連

- [[CLAUDE]] — Vault 動作ルール
- [[claude_code]] — Claude Code 全般の運用 (まだなら作成)
- [[vault_workflow]] — 1日の運用フロー

---

## 📝 更新履歴

- 2026-05-19: 初版作成。グローバル設定 + `claude-init` 関数 + `settings.local.json` 整理を完了。
