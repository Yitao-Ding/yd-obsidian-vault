# mistakes/tool_usage_mistakes.md — ツール使用関連のミス記録

> Claude のツール選択・呼び出し方に関する過去のやらかしを蓄積するファイル。
> Desktop Commander / MCP / Bash などのツールを「使えるのに使わなかった」「誤った使い方をした」ケースを記録する。
>
> 詳細カテゴリは [[claude_mistakes]] のカテゴリA を参照。
>
> 最終更新: 2026-05-18

---

## 記録テンプレート

### タイトル (頻度: 高/中/低、最終発生: YYYY-MM-DD)

**状況**:

**過去のやらかし**:
- YYYY-MM-DD:

**正しい挙動**:

**再発防止**:

---

## 記録

<!-- ここに新しい事例を追記 -->

### 大きなバイナリを base64 にしてツール引数1回で転送しようとした (頻度: 低、最終発生: 2026-08-24)

**状況**:
Cowork のクラウド環境で作った zip (8KB) を YD の Mac に置くため、base64 (17KB、1行) を Desktop Commander の write_file に貼り、Mac 側で `base64 -D` + unzip した。

**過去のやらかし**:
- 2026-08-24: 展開時に CRC エラー。md5 も不一致。ツール引数に書き写した base64 が途中で崩れていた。1ステップ無駄にした

**正しい挙動**:
テキストファイルは write_file の分割書き込み (≤50行、rewrite → append) で1ファイルずつ送り、送信後に両側の md5 を照合する。今回は6ファイル339行を10回の write で転送し、全一致を確認できた。

**再発防止**:
- 数KB を超える base64 / バイナリをツール引数で書き写さない。テキストなら分割 write_file、バイナリならデバイスブリッジ (device_commit_files) か SendUserFile で渡す
- 転送後は必ず md5 で照合してから「置いた」と報告する
---

## ❌ ミス: rcloneの転送後ハッシュ検証を「スタック」と誤診してkill→48GB再DL (発生: 2026-08-11) #tool

### 状況
Google Drive→HDDの559GiB rclone copy中、大容量ファイル4本が「100% / 0 B/s」のまま28分継続。

### 過去のやらかし
ネットワーク統計だけ見て「接続ゾンビ化」と断定しkill→再起動。実際はmulti-thread copyの転送後MD5検証 (12GB×4をHDDから読み直し、シーク競合で40分級) の最中だった。killで4本分=約48GBの再ダウンロードが発生。

### 正しい挙動
「0 B/s」はネットワークの数字でしかない。kill前に **iostat でディスクI/O** と **ps でrclone CPU** を確認する。ディスクが読み書きしていれば検証/書き込み中=正常。

### 再発防止
1. rclone大容量転送の完了間際の0 B/sは、まずiostat -d -w 3で対象ディスクのMB/sを見る (今回これで一発確定した)
2. multi-thread copyは完了後にローカル再読込のハッシュ検証がある仕様を前提に待つ (12GB×4なら15〜40分)
3. プロセスをkillする判断は「ネットワーク0 + ディスク0 + CPU 0」が揃ってから

---

### tool_search の1回の空振りで「Vault未接続」と宣言した (頻度: 中、最終発生: 2026-09-06)

**状況**:
デスクトップチャットの新セッションで YD が「おはよう」。起動シーケンスを走らせる前に利用可能ツールを確認したが、Desktop Commander を見つけられず「Vault未接続」と返した。YD が「これMacで送ってる、Vault読み込んで」と指摘して発覚。

**過去のやらかし**:
- 2026-09-06: `tool_search("read local file desktop commander filesystem")` と `tool_search("github repository read file contents")` の2回が Canva/Figma/Zoom を返しただけだったため、「この環境に Desktop Commander も GitHub コネクタも無い」と断定。同じセッション内で2回「Vault未接続」と宣言し、2回目の「おはよう」でも訂正しなかった。実際は `tool_search("Desktop Commander read_multiple_files list_directory")` で一発でロードできた。カタログには最初から Desktop Commander 26件 / Control your Mac / Control Chrome が載っていた

**正しい挙動**:
- 遅延ロード方式の環境では、tool_search はサーバー名とツール名をそのまま入れる。機能の説明文 (「read local file」「filesystem」) で引くと意味の近い他社ツールに埋もれる
- 空振りしたら「無い」ではなく「クエリが悪い」を先に疑う。ツールカタログの一覧に名前が載っているかを確認してから結論を出す
- 00_CLAUDE_BOOT.md §B の「ローカルファイルにアクセスできないとは絶対に言わない」に真っ向から反する宣言をしていた

**再発防止**:
- Vault を読む前の tool_search は必ず `Desktop Commander read_multiple_files list_directory` の形 (サーバー名 + 具体的ツール名) で引く
- 「Vault未接続」と書きそうになったら、その前に最低3通りのクエリで tool_search を試す。1回の空振りは根拠にしない
- YD が Mac から送っていることは既定値。接続が無いように見えたら環境ではなく自分の探し方を疑う
