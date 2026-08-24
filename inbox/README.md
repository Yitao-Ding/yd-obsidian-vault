---
type: meta
created: 2026-08-24
---

# inbox — Mac に届かない環境からの投函口

iPhone のチャットやブラウザの Claude など、Desktop Commander もデバイスブリッジも使えない環境の Claude は、Google Drive コネクタで Drive の `ObsidianVault-inbox` フォルダに Markdown ファイルを置く (Google ドキュメントに変換しない)。Mac の Google Drive デスクトップ (save.yitao@gmail.com) が `~/Library/CloudStorage/GoogleDrive-save.yitao@gmail.com/マイドライブ/ObsidianVault-inbox/` に同期し、自動保存ジョブ (`~/.claude/vault-autosave/`、30分毎) がこのディレクトリに取り込んで decisions / knowledge / mistakes / current_state / log.md に振り分け、処理済みファイルは `archive/inbox_processed/` に移す (Drive 側の元ファイルは `_processed/` へ)。Drive の `_boot/` には起動用の写し (current_focus / active_projects / recent_decisions / claude_mistakes / 00_CLAUDE_BOOT) が毎回書き出される。

Mac 上で直接ここに置いてもいい。ファイル名は `YYYY-MM-DD_HHMM_<内容>.md`。既存ファイルの編集はしない。

```
---
type: inbox
source: iphone-chat | web-chat | cowork
date: YYYY-MM-DD HH:MM
kind: decision | knowledge | progress | mistake | identity | log
---
# <一行の見出し>

## 本文
(保存したい内容。決定なら状況と選んだ理由、知識なら手順と詰まった点)

## 会話の要点
(この会話で何が話され、何が決まったか。3〜10行)
```

`_` や `README` で始まるファイルは処理対象外。
