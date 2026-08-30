---
type: knowledge
domain: filmmaking
created: 2026-08-31
tags: [gigafile, 納品, 舞台収録, cli]
---

# ギガファイル便 CLI 納品ワークフロー (プペル定点 2026-08-31)

ブラウザを使わず、Mac 上の Python スクリプトで直接ギガファイル便に送る。YD が同じ Mac で Chrome を使っていても競合しない。

## ツールの場所
- `/Volumes/Extreme pro/プペル/_tools/gfup.py`: 1グループ分 (複数ファイル) をアップロードし、まとめてDL用URLを発行。結果は同ディレクトリの `results.jsonl` に追記
- `/Volumes/Extreme pro/プペル/_tools/run_all.py`: リネーム + 全グループ順次アップロード (完了済みはスキップ、再実行可)
- 実行: `nohup caffeinate -i python3 run_all.py > upload.log 2>&1 &` (caffeinate でスリープ防止)

## プロトコル (gigafile.nu の upload.js から)
- `GET https://gigafile.nu/` の `var server = "NN.gigafile.nu"` で担当サーバーを取得
- `POST https://{server}/upload_chunk.php` に multipart: id (uuid hex), name, chunk, chunks, lifetime (3/5/7/14/30/60/100), file。チャンクは 100MB。最後のチャンクの応答 JSON に `url` が入る
- チャンクは「完了順」が守られないと欠落する。並列で流す時は、チャンク i の末尾バイトを i-1 完了まで送らない (gfile ツールと同じ手)。これを守らないとサイズが 100MB 単位で欠ける
- まとめ: `POST https://{server}/matomete_get_url.php` に urls[] / dlkeys[] / file_name / dlkey / lifetime。同じ session (cookie) で行う。file_name に `\ / : , ; * ? " < > |` は不可
- 検証: `https://{server}/download.php?file={id}` を stream GET して Content-Length をローカルサイズと比較
- 実測速度: 約 60〜70 MB/s (YD 宅回線、2026-08-31)

## ナンバー名の読み取り
- 開始 5〜20 秒で投影映像に「<名前> number」の字幕。タイミングがばらつくので、ffmpeg の `fps=0.5,scale=576:-1,tile=5x4` で 40 秒分のコンタクトシートを作って一覧で読む
- エンドロール (最後のクリップ) に全ナンバー名と出演者が出るので、正式名称の照合に使える

## ファイル名の記号
- macOS で `/` `:` は使えないので全角 `／` `：` に置換 (例: `Hi,Me：)`、`KAЯO×Pooh／Haruki`)
