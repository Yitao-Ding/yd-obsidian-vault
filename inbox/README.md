---
type: meta
created: 2026-08-24
---

# inbox — Mac に届かない環境からの投函口

iPhone のチャットやブラウザの Claude など、Desktop Commander もデバイスブリッジも使えない環境の Claude が、GitHub コネクタでこのディレクトリに新規ファイルを置く。Mac 側の自動保存ジョブ (`~/.claude/vault-autosave/`、30分毎) が pull して decisions / knowledge / mistakes / current_state / log.md に振り分け、処理済みファイルは `archive/inbox_processed/` に移す。

ここに置くファイルは常に新規作成 (既存ファイルの編集はしない)。ファイル名は `YYYY-MM-DD_HHMM_<内容>.md`。

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
