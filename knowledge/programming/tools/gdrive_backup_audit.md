---
type: knowledge
domain: programming/tools
created: 2026-09-16
---

# 外付けHDD → Google Drive バックアップ差分監査 (rclone)

YITAO HDD と Extreme pro の中身が Google Drive に上がっているかを確認し、未バックアップ分だけをアップロードする手順。2026-09-16 に実施。

Drive 側はフォルダ構成がローカルと一致していない (ローカル `ぷぺる_タイムラプスメイキングなど` が Drive では `プペル_2026-08-30`、HiMe/ダンちゃりは `外部/` 配下、など)。パス単位の同期では判定できないので、**ファイル名 + サイズ** の集合で突き合わせる。

## 手順

1. ローカル全ファイル一覧
   ```
   find "/Volumes/Yitao HDD" "/Volumes/Extreme pro" -type f -not -name ".DS_Store" \
     -exec stat -f "%z %N" {} + > /tmp/local_all.txt
   ```
2. Drive 全ファイル一覧 (`--fast-list` 必須、無いと数倍遅い)
   ```
   rclone ls gdrive_save: --fast-list > /tmp/drive_all.txt
   ```
3. Python で `(basename, size)` の集合を作って突合。ローカル側の各ファイルが Drive 集合にあるかで判定し、無いものを `/tmp/missing.txt` に出す
4. 未バックアップ分を `--files-from` のリストにしてアップロード
   ```
   rclone copy "<src>" "gdrive_save:<dst>" --files-from /tmp/list_x.txt \
     --transfers 4 --drive-chunk-size 128M --drive-stop-on-upload-limit \
     --stats 60s --stats-one-line --log-file /tmp/x.log -v
   ```

`rclone about gdrive_save:` で残容量を先に確認すること。今回は未バックアップ 2,157GB に対し空き 1,715GB で入りきらず、優先度の低い素材を除外する判断が必要だった。

## ✅ うまく行ったこと

- 名前+サイズ突合は Drive 側のフォルダ構成がバラバラでも機能した。25,438 ファイル対 26,192 ファイルの照合が数分で終わる
- ローカル内の重複検出が副産物で得られた。`ぷぺるコレオ` が HDD 内 2 箇所に完全同一 (233GB)、`練習動画_2026-02` が `SSDのやつ/整理済み/` にも同一で存在 (61GB)。Drive には 1 回だけ上げて 293GB 節約
- 遅い転送を「そんなもの」と受け入れず `ifconfig` を見たら、USB LAN アダプタが 100baseTX でリンクしていた ([[claude_mistakes]] A-24 の教訓が効いた)

## ❌ 詰まったこと

- **macOS の Unicode 正規化 (NFD) で `--files-from` が全件マッチしない**。macOS のファイルシステムは濁点・半濁点を分解して保持する (`ぷ` = `ふ` + `゜`)。rclone はリスト中の文字列をそのまま照合するため、NFC で書いたリストはディレクトリ名の段階で `Excluded` になり、1 件も転送されないまま `rc=0` で正常終了する。**エラーが出ないので気づきにくい**
  - 解決: リストは `unicodedata.normalize('NFD', path)` で書く。`--files-from-raw` でも NFD なら通る、NFC は通らない
- 同じ理由で Python 側のパス接頭辞スライスも壊れた。`p[len(root)+1:]` の `root` がリテラル (NFC) で `p` が NFD だと長さがずれ、相対パスの先頭が欠ける。正規化形を揃えてからスライスする
- `rclone size` を Drive の大きいフォルダに対して実行すると数分かかる。全体像は `rclone ls --fast-list` を 1 回流して自分で集計したほうが速い

## 📋 次回同じことをするときのチェックリスト

1. `rclone about gdrive_save:` で空き容量を確認。ローカル総量と比べて足りるか先に判断する
2. `find -exec stat` と `rclone ls --fast-list` で両側の一覧を取る (バックグラウンド実行、HDD 側は数分)
3. 突合は `(basename, size)` で。パス構造は当てにしない
4. **`--files-from` のリストは必ず NFD で書く**。投入前に `rclone lsl <src> --files-from <list> | wc -l` がリスト行数と一致するか検証する。0 件でも rclone は成功扱いで終わるので、検証を飛ばすと「上げたつもり」になる
5. 転送が遅いときは `ifconfig <iface> | grep media` でリンク速度を確認。`100baseTX` なら約 11.8MB/s が上限
6. アップロード先は差分だけを入れるので、既存の Drive フォルダとは別に `HDDバックアップ差分_YYYY-MM` のような名前にして、差分であることを明示する

## 関連

- [[claude_mistakes]] A-24 (遅い処理を所要時間として受け入れない)
- [[media-organization-project]]
