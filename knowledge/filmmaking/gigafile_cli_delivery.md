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

## Google Drive バックアップ (2026-08-31 追記)
- Mac の空き容量より大きいデータを Google Drive アプリ経由でコピーすると、ローカルキャッシュでディスクが溢れる。大容量は rclone で SSD から直接送る
- rclone リモート `gdrive_save` = save.yitao@gmail.com の Drive (5TB 契約、2026-08-31 時点で空き 1.9TB)。認証トークンは ~/.config/rclone/rclone.conf
- 実行例: `rclone copy "<src>" "gdrive_save:<folder>" --transfers 2 --drive-chunk-size 128M --stats 30s -v --log-file <log>`、終了後 `rclone check --size-only` で照合
- 実測 約45MB/s (2026-08-31)。保存先: マイドライブ/プペル_2026-08-30/

## SSD が I/O ハングした時の復旧と、納品済みファイルからの作業継続 (2026-09-04 追記)

外付け SSD (Extreme pro) が突然 I/O ハングし、`ls` も `lsof` も返らなくなった。`diskutil info` は即答するのに `ls` が固まる場合、ファイルシステム層が刺さっている。

- `diskutil unmountDisk force /dev/diskN` は `ls` が固まっていても通る。クライアントが応答しなくても、カーネル側は数十秒後に完了しているので、`df` から消えたかで判定する。書き込み中のプロセスが無ければデータは壊れない
- ただしアンマウント後に USB デバイス自体が固まったままだと、`diskutil list diskN` も `system_profiler SPUSBDataType` も応答しなくなり、再マウントできない。ここまで来ると物理的な抜き差ししかない。アンマウント済みなので抜線は安全
- ドライブが死んでも、ギガファイルに上げたファイルは `curl -c cookies.txt <ページURL>` でセッションを取ってから `curl -b cookies.txt "https://NN.gigafile.nu/download.php?file=<id>"` で回収できる。Google Drive のバックアップ (rclone) と合わせれば、SSD 無しで作業を続行できる
- `setsid` は macOS に無い。バックグラウンド常駐は `(nohup caffeinate -i ./script.sh >/dev/null 2>&1 &)` の形で括弧に入れる

## ffmpeg での音量計測 (2026-09-04)

- volumedetect / loudnorm の print_format / ebur128 の結果は **info レベル**で出る。`-v error` を付けると全部消えて何も返らない。`-hide_banner` だけにする。`-nostats` も ebur128 の逐次出力を消すので付けない
- 全体像: `ffmpeg -hide_banner -i F -map 0:a:0 -af loudnorm=I=-14:TP=-1:LRA=11:print_format=json -f null -` → input_i (統合LUFS) / input_tp (トゥルーピーク) / input_lra
- 時間変化: `-af ebur128=peak=true` の `t: / M: / S: / I:` を拾う。特定区間だけ小さいのか全体が小さいのかはこれで分かる
- 逆相チェック: `pan=mono|c0=0.5*c0-0.5*c1` の mean_volume が L / R 単独より 20dB 以上低ければ左右はほぼ同相 (モノラル再生で消えない)
- 帯域バランス: `lowpass=f=100` / `bandpass=f=300:width_type=o:w=2` / `highpass=f=2000` の 3点で mean_volume を比べる。300Hz 帯と 2kHz 以上の差が 15dB を超えると「遠い、こもった、小さい」と感じる

## 「音が小さい」の切り分け (プペル琴 2026-09-04)

FX30 のカメラマイクでホール収録した琴ナンバーが「めっちゃ音小さい」と言われたが、計測すると −11.3 LUFS / トゥルーピーク −1.45 dBTP で、配信基準 (−14 LUFS) より 2.7 LU 大きく、上げ代は 1.4dB しか無かった。短期ラウドネスも全編 −9〜−14 で一定。元の FX30 ファイル (PCM) が −11.44 LUFS で、書き出しは 0.1〜0.3dB 以内で一致していた。

原因はレベルではなく帯域バランス。300Hz 帯 −16.6dB に対し 2kHz 以上が −31.8dB (15.2dB 差)、クレストファクター 12dB。箱鳴りと客席のざわめきが支配的で、遠くて眠い音になっていた。**メーターが振れているのに小さく聞こえる時は音量ではなく音色を疑う。**

対処 (映像は再エンコードせず音声だけ差し替え):
`highpass=f=80,equalizer=f=280:t=q:w=1.0:g=-3.5,equalizer=f=4000:t=q:w=0.9:g=4,volume=2.4dB,alimiter=limit=0.891:attack=5:release=60:level=disabled`
結果 −11.09 LUFS (据え置き) で 300Hz と 2kHz 以上の差が 11.7dB に縮まり、体感が明確に上がった。

**alimiter の attack はそのまま先読み遅延になる** (attack=5 → 音声が 5ms 遅れる)。基準の映像に対して合わせ直す時は入力の `-ss` を同じ分だけ後ろにずらす。600Hz〜5kHz に帯域制限して相互相関を取れば、フィルタの位相回りに騙されずにずれを測れる (広帯域のままだと highpass の群遅延で 5〜8ms ずれて見える)。
