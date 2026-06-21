# rclone で R2(S3互換)既存オブジェクトの HTTP メタデータを直す

作成: 2026-06-17 / 関連: [[claude_mistakes]] B-6、easy-share `HANDOVER.md`

## いつ使うか
Cloudflare R2(や S3)に既にアップ済みのオブジェクトの `Content-Disposition` / `Content-Type` /
`Cache-Control` を、**本体を再アップせずに**変えたいとき。
(きっかけ: easy-share の「写真原本DL→iPhoneアルバムで灰色四角」バグ。原本の CD に filename が
無く iOS Safari が拡張子を落としていた。詳細は HANDOVER の罠セクション。)

## ハマりどころ(ここで詰まった)
1. **`--header-upload` はサーバサイドコピーでは Content-Disposition に効かない**。
   オブジェクトは書き換わる(btime 更新)が、メタデータは元の値が保持される(MetadataDirective=COPY 相当)。
   → **`--metadata --metadata-set <key>=<value>` を使う**。S3 backend が REPLACE で書く。
2. **同一パスへの `copyto src src` はスキップされる**(rclone が同一オブジェクトと判定、`--ignore-times` でも不可)。
   → **中間キー(例: `.fixhdr.tmp`)経由で往復**させる。
3. 公開 `r2.dev` は `immutable` キャッシュだが、メタデータ更新は**プレーンURLに即反映**された
   (HEAD で `cf-cache-status`/`Age` 無し)。エッジキャッシュで古いヘッダが残る事象は今回は起きず。

## 効いたパターン(1ファイル)
```bash
export PATH="/opt/homebrew/bin:$PATH"
B="r2:easy-share-media"
KEY="originals/写真/DSC00298/DSC00298.ARW"
TMP="originals/写真/DSC00298/.fixhdr.tmp"
CD='attachment; filename="DSC00298.ARW"'
CC='public, max-age=31536000, immutable'

# src -> TMP(メタデータをセットして実コピー)
rclone copyto "$B/$KEY" "$B/$TMP" --metadata \
  --metadata-set content-disposition="$CD" --metadata-set cache-control="$CC" --s3-no-check-bucket
# TMP -> src(--ignore-times で強制上書き、メタデータ再セット)
rclone copyto "$B/$TMP" "$B/$KEY" --metadata \
  --metadata-set content-disposition="$CD" --metadata-set cache-control="$CC" --ignore-times --s3-no-check-bucket
rclone deletefile "$B/$TMP" --s3-no-check-bucket
```

## 確認コマンド
```bash
# 実際のストア値(CDN を経由しない)
rclone lsjson "$B/$KEY" --metadata | python3 -c "import json,sys;print(json.load(sys.stdin)[0]['Metadata'])"
# 公開URLのヘッダ(キャッシュ回避は ?cb=$(date +%s))
curl -sI "https://pub-….r2.dev/<encoded-key>" | grep -iE "content-disposition|cf-cache-status|age"
```

## 一括版
easy-share に `ingest/fix-original-headers.sh` として実装(originals/ 配下を一括修復、bash3.2互換、検証付き)。

## 教訓
- iOS Safari で DL ファイルの拡張子を保ちたいなら **`Content-Disposition: attachment; filename="…"` を必ず付ける**
  (filename が無いと Content-Type から拡張子を推測 → octet-stream だと拡張子が落ちる)。
- 「壊れて見える」系はまず hash 一致 → OS デコード可否 → 配信ヘッダ、の順で切り分ける([[claude_mistakes]] B-6)。
