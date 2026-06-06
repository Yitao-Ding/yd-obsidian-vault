---
type: decision
date: 2026-05-31
project: easy-share
tags: [撮影, 色管理, ツール, FX30, S-Log3, PWA]
---

# easy-share 設計確定(社内 撮影素材 共有・プレビューツール)

## 背景・動機

ダンス×クリエイティブチーム(4 名)向けの社内ツール。今日撮った FX30 S-Log3 動画 + ARW RAW 写真を、その日のうちに**色を揃えて軽くプレビュー・共有**する。全員 MacBook(Apple Silicon)+ iPhone、Android/Windows 非対象。元データが重い(4K S-Log3 ≈ 1.5GB/分、RAW)ので、プレビューは見た目の画質を保ちつつ回線に左右されにくく。商用配布ではない社内ツール(= Max 20x 等の制約とは無関係)。

パス: `~/projects/easy-share/`

## 確定事項(YD 回答)

| 論点 | 決定 |
|------|------|
| Log 表示 | **flat + ルック切替**(`ingest/luts/*.cube` を各「ルック」として切替。YD が FX3 4 ルック提供 = Film Tone / Camp Moody / Blue Snow / Pure Night、.cube の出し入れで増減)。中立 LC-709 も任意で追加可 |
| 主目的 | 確認・共有(最終グレーディングは本番 NLE) |
| ストレージ | 専用オブジェクトストレージ(Cloudflare R2) |
| 取り込み | Mac 監視フォルダ(Mac 側で proxy 生成 → 自動アップロード) |
| 素材の整理 | **プロジェクト単位で振り分け**(①のフォルダ、②のフォルダ…) |
| 閲覧アプリ | **まず PWA**、必要なら後でネイティブ iOS |
| デザイン | **Spotify 風ダークテーマ**(YD 提供のデザインシステム) |

## 最重要の技術判断: 色一致は "partial"(8エージェント並列リサーチ + 敵対的検証で確定)

「全員 Apple だからタグを埋めれば色は完全一致」は**半分本当・半分嘘**。正確には:

1. **写真(静止画)は堅牢に一致**。WebKit/ColorSync が ICC を尊重、未タグは sRGB 扱いで破綻しない。→ Display P3 埋め込みで 4 台ほぼ一致。実質解決済み。
2. **動画はタグ次第**。未タグ動画が最大の事故源(Safari は未タグを BT.601 と誤解)。`color_primaries/transfer/matrix` + `range(full/limited)` を明示すれば一致。
3. **S-Log3 はそのままでは絶対に正しく出せない**。ITU-T H.273 に S-Log3 のコードポイントが無い(=タグ付け不能)、S-Gamut3.Cine→Rec.709 は非線形変換でタグでは表現できない。→ **必ず Sony 公式 LUT を焼く**。YD の「両方切替」= ①graded(LUT 焼き)+ ②flat(709 タグで復号、眠い)の 2 本生成で実現。
4. **画面状態の罠**: True Tone / 自動輝度 / Night Shift が各自オンだと、ファイルが正しくても見え方がズレる。**色を見る時は全員オフ**が運用必須。
5. **パネル個体差**: 工場校正済みでも dE ≈ 1.5–2.0、「近い」であって「画素一致」ではない。
6. 結論: ツールの立ち位置は「**全員が同じ標準変換を通した同じ色を見る信頼できる共通レビュー**」。最終色サインオフはキャリブレーション済みモニタ + DaVinci で。誇張しない。

## スタック確定

- **取り込み(Mac 側ネイティブ)**: bash + `sips`(ARW→P3 HEIC/JPEG)+ `ffmpeg hevc_videotoolbox`(10bit HEVC proxy、LUT 焼き)+ `rclone`(R2)+ `fswatch`(監視)。クラウド変換より色が正確(Apple RAW 現像 > LibRaw)+ 無料 + 速い。
- **ストレージ**: Cloudflare R2(egress 0)。容量増えたら Backblaze B2 + Cloudflare に差し替え可(同 S3 API)。Google Drive は配信バックエンド失格(再圧縮・色管理なし・API 制限)。
- **配信 + 認証**: Cloudflare Access(Zero Trust 無料、4 メール許可、Email OTP、アプリ側コード 0 行)。★ **本番は「単一オリジン」必須**(2026-05-31 本番手順リサーチ `winjwy5ic` で確定): app と素材を別ホストにして別々に Access で守ると `<img>/<video>` のサブリソースが 302 ログインに飛んで**素材が壊れる**(Safari ITP/Chrome 3rd-party cookie でさらに確実)。→ app と `/media/*`(R2)を同一ホストから出し Access 1 個で全保護。
- **閲覧アプリ**: Next.js 16(App Router)PWA。`next/image` は使わない(ICC を剥がす)→ 素の `<img>/<video>`。自前ライトボックス(flat⇄graded 切替 + DL)。★ **本番デプロイは Phase 1 の「Vercel」から all-Cloudflare(@opennextjs/cloudflare)推奨に修正**(単一オリジン Access が最も素直 + Vercel×CF proxy の SSL/ACME 罠回避。コードはそのまま)。
- **Sony LUT**: 公式無料 .cube あり(grading guide の `Look_profile_for_resolve_S-Gamut3.cine_Slog3.zip` → LC-709 Type A=中立)。`.cube` は gitignore なので `git add -f` で1ファイルだけ共有。DaVinci CST で自前生成も可。

## 実機検証で潰した実バグ(信頼性の根拠)

1. `hevc_videotoolbox` が出力オプションだけだと `color_primaries/transfer` を書かず `unknown`(=未タグ動画=Safari 誤解)→ **`setparams` フィルタで焼き込み**修正。`bt709/bt709/bt709/tv/hvc1` が全部乗るのを ffprobe で確認(ffmpeg 8.1 実機)。
2. ffprobe 出力順ズレで検証ゲートが常に外れる → フィールド名で取得に修正。
3. 1 ファイル破損で全バッチ停止(`set -e`)→ per-asset 耐性に修正。
4. Tailwind v4 で `@theme inline` を使ったため カスタム色ユーティリティ(`bg-spotify` 等)が未生成 + `body var()` 参照も壊れた → `@theme`(非 inline)に修正 + dev クリーン再起動。**スクショでは気づけず計算済みスタイル検証で発見**。

## いまの状態(Phase 1 完成・実機動作確認済)

- `DESIGN.md` / `README.md` / `ingest/{process.sh,watch.sh,config.example.sh,luts/}` / `web/`(Next.js PWA)
- 合成素材 2 プロジェクト(①_MV撮影 / ②_イベント密着、計 8 点)で取り込み→ギャラリー→ライトボックス(flat/graded 切替・色タグ表示・DL)まで Playwright でフル動作、コンソールエラー 0、本番ビルド通過。
- 色タグ・P3 プロファイルは実値で検証済(計算済みスタイルで Spotify 配色 #1ed760 等も確認)。

## 残課題 / YD 作業(本番化に必要)

- Cloudflare(R2 バケット + カスタムドメイン + Access 4 メール)/ アプリ用ドメイン / `brew install rclone fswatch`
- **Sony 公式 S-Log3 S-Gamut3.Cine → Rec.709 LUT(.cube)** を `ingest/luts/` に
- チーム合意: 色を見る時は True Tone/自動輝度/Night Shift オフ
- **本物の FX30 S-Log3 + ARW で実取り込みテスト**(合成素材は配管の証明まで。実色が 4 台で揃うか本物で 1 回通す)

## フェーズ計画

- Phase 1(完了): 監視フォルダ + 単一 HEVC proxy(graded+flat)+ P3 HEIC/JPEG + R2 + PWA ギャラリー + Access
- Phase 2: HLS マルチビットレート(弱回線耐性)、10bit HEIC、CIRAWFilter で RAW 色質、menubar アプリ化
- Phase 3: HDR(HLG)、検索/タグ/お気に入り、コメント/承認、(必要なら)ネイティブ iOS

## 関連

- リサーチ生データ: workflow `wafcglinq`(設計: color/RAW/Log video/storage/ingest/app-stack + 敵対的検証 2 本)+ `winjwy5ic`(本番手順: Sony LUT / R2+rclone / R2 ドメイン+CORS / Access サブリソース問題 / デプロイ)
- `~/projects/easy-share/DESIGN.md`(全文設計)/ `SETUP.md`(本番セットアップ手順書)
