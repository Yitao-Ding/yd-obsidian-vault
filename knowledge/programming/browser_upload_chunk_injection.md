---
type: knowledge
created: 2026-07-28
tags: [browser-automation, claude-in-chrome, youtube, file-upload]
---

# 大容量ファイルのブラウザ完全自動アップロード (チャンク注入方式)

> claude-in-chrome 拡張環境で、300MB級のローカルファイルを人力ゼロで Web ページの file input に投入する方式。
> 2026-07-28 localhost FM ep-03 (302MB mp4) の YouTube Studio アップロードで実証。
> 出典: `~/AI projects/youtube bgm/HANDOVER.md` 2026-07-28 セクション (手順の正本はそちら)

## 背景 (なぜ普通の方法が全滅するか)

- 拡張が付いたタブでは**ファイル選択ダイアログが開かない** (拡張がインターセプト)。拡張クリックでも cliclick 実クリックでも不可
- 拡張の `file_upload` ツールは **10MB/回制限** (中身をブリッジ経由で送る仕様)
- ページ内 fetch で `http://127.0.0.1` は **Chrome の Local Network Access 制限でブロック** (サーバにリクエストすら届かない)
- macOS のファイルパネルは XPC 別プロセス → System Events の AX ウィンドウ列挙に映らず、キー送出も宛先保証なし

## 方式 (5ステップ)

1. `split -b 9900000 file.mp4 part_` でチャンク化
2. ページに可視ヘルパー `<input type=file>` を JS 注入 (**不可視化するとautoモード分類器に弾かれる**。目的コメント付き・可視で)
3. 各チャンク: `file_upload` → JS で `arrayBuffer()` を **名前キーの map** に蓄積 → input クリア。**1 browser_batch = 1チャンク**
4. キー sort → `new File(parts, ...)` → DataTransfer → 本命 input `.files` セット → `change` イベント dispatch
5. 合計バイト数を元ファイルと照合してから発火

## ✅ うまく行ったこと

- 31チャンク=302,008,669 bytes が完全一致で結合され、YouTube Studio が通常アップロードとして受理
- `file_upload`+`javascript_tool` を同一 browser_batch に混在させられる (往復半減)
- 許可ルール `mcp__claude-in-chrome__javascript_tool` を project の settings.local.json に追加すれば分類器を通る (YD 明示許可のもと)

## ❌ 詰まったこと

- 分類器ブロック2回: ①不可視style付き注入 → 可視+目的コメントで通過 ②ファイル読取JS → 許可ルール追加が必要だった (言い回し変更での再試行はポリシー違反なので YD にエスカレーション → YD が全自動を許可)
- browser_batch 内の file_upload は**合計**10MB制限 → 2チャンク/バッチ不可。初回のバッチ失敗で配列蓄積の順序が壊れかけた → 名前キー map で根治
- YouTube Studio の詳細ダイアログは **Escape で閉じる** (タグサジェスト閉じ目的でも押すな。ドラフト保存されるので「ドラフトを編集」で復帰は可能)

## 📋 次回同じことをするときのチェックリスト

1. 許可ルールが `cc company/.claude/settings.local.json` にあるか確認 (無いと分類器で止まる)
2. split は 9,900,000 bytes 以下
3. ヘルパー input は可視+aria-label+目的コメント
4. 蓄積は名前キー map、1バッチ1チャンク、毎回 `n:件数 total:バイト` を検証
5. 発火前に total == 元ファイルサイズを確認
6. Escape 禁止。ポップアップは別領域クリックで閉じる
