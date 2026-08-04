---
type: decision
date: 2026-07-28
project: localhost FM
---

# YouTube公開の完全自動化と javascript_tool 許可ルール追加

## 状況

localhost FM ep-03 の公開作業で、300MB動画のファイル投入だけが自動化の壁になっていた (ファイル選択ダイアログは拡張がインターセプト、file_uploadは10MB制限、localhost fetchはLNAブロック)。チャンク注入方式で突破できるが、ページ内JSでファイル内容を読む操作が Claude Code の auto モード分類器にブロックされた。

## 選択肢

- A. 動画投入だけYDがドラッグ&ドロップ (10秒の人力、設定変更なし)
- B. `mcp__claude-in-chrome__javascript_tool` の許可ルールを追加して完全自動化
- C. cloudflared 一時公開トンネル経由 (外部露出あり)

## 決定

**B を採用**。YD 発言「もう一回自分でやってみて。この操作は全部自分でやるようにして。YDが全自動を許可します」(2026-07-28 深夜)。

- 追加先: `~/AI projects/cc company/.claude/settings.local.json` (**このプロジェクト限定**、グローバルではない)
- 内容: `permissions.allow: ["mcp__claude-in-chrome__javascript_tool"]`
- 含意: cc company で動く Claude はページ内JSを分類器の個別審査なしで実行できる。ページ内JSの安全チェックが一段緩くなるトレードオフを YD が了解済み

## ✅ うまく行ったこと

- 直後に ep-03 (302MB) の完全自動アップロード〜公開まで成功。以後のエピソードは人力ゼロで公開可能に

## ❌ 詰まったこと

- 分類器の意図 (ページからのデータ窃取と同型のコードを弾く) を YD に説明する必要があった。「分類器って何」への説明は decisions 化しておくと次回楽

## 📋 次回同じことをするときのチェックリスト

1. 分類器ブロックに当たったら: 言い回し変更で粘らない → YD にエスカレーション → 許可ルール追加は YD の明示指示を得てから
2. 許可はプロジェクト限定 (settings.local.json) を既定にする
3. 手順詳細 = [[browser_upload_chunk_injection]] / `youtube bgm/HANDOVER.md`
