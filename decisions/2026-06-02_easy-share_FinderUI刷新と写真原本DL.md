---
type: decision
date: 2026-06-02
project: easy-share
tags: [easy-share, frontend, finder-ui, r2, download, git, vercel]
---

# easy-share: Finder風UI刷新 + 写真原本DL(2026-06-02)

> 正本は `~/projects/easy-share/HANDOVER.md`。本ノートは意思決定と知見の記録。
> 関連: [[2026-05-31_easy-share設計確定]]

## 背景 / きっかけ
YD「ファイルの取り回しがやりにくいから、macのfinderのように作り替えて」→ さらに
「元データ(とりあえず写真系列だけ)も同時にアップロードとサイトからダウンロードできるように」。

## 決定したスコープ(YD回答で確定)
- 対象 = **Web閲覧アプリのUIのみ**(Mac取り込みpipelineは触らない)。
- 範囲 = **読み取り専用のまま**(ファイル移動/リネーム/削除はやらない)。現アーキ(R2公開/manifest駆動)維持。
- 表示モードは**ユーザーが切替できる**こと。
- 原本DLは**写真系列のみ**(動画原本は数百MBで重いので除外)。

## 実装(本番反映済 https://easyshare-fx.vercel.app)
### Finder風UI
- 4表示モード(アイコン/リスト/カラム=Miller Columns 3ペイン/ギャラリー)+ツールバー切替(⌘1-4)。
- サイドバー = スマートグループ(すべて/最近/動画/写真=**プロジェクト横断**)+ プロジェクト別(件数バッジ)。
  ← 「取り回しの悪さ」の核(横断・ソート・密度変更ができない)に直接効くのが横断スマートグループ。
- ソート/種別フィルタ/横断検索/アイコンサイズ、Quick Look(既存Lightbox再利用+role=dialog/フォーカストラップ)、
  URL共有(`?view=&g=&p=&sort=&asset=`)、localStorage永続化、レスポンシブ、a11y強化。
- 構成: `web/src/components/browser/`(Sidebar/Toolbar/Card/PreviewSurface/PreviewPane/4ビュー/hooks/urlState)新設、
  `GalleryApp.tsx` を状態オーナーに書換。色管理(.media-managed)とWebGL LUT(VideoView)は温存。

### 写真原本アップロード & DL
- `ingest/process.sh`: `UPLOAD_ORIGINALS` を **0 / photos / all** の3モードに(従来は 0/1 の数値)。
  photos=写真系列のみR2へ。`Content-Disposition: attachment` を付与。
- `config.sh` を `UPLOAD_ORIGINALS=photos`。
- Web: 「元データをDL」を**写真のみ表示**(動画原本未対応=404防止、`!isVideo` ゲート)。
- 既存本番写真 **77枚(2.15GB)を R2 にバックフィル**(2026-05-31=8 / sample=4 / 写真=65)。

## 開発の進め方(ultracode)
設計workflow(3視点=Finder忠実度/モバイル/IA・状態 を分解→統合)→ 実装 → レビューworkflow
(4次元レビュー、41件提起→各指摘を反証検証→18件確定→反映)→ Playwright実機検証。

## 技術知見(再利用価値)
- **Next.js 16 で URL同期**: `useSearchParams` はプリレンダー時に Suspense を要求する罠 → 使わず
  `window.history.replaceState` で書き、初期化はマウント effect で `window.location.search` を parse。
  公式SPAガイドのシャロールーティング準拠。Suspense ラップ不要になる。
- **クロスオリジンDLの罠**: `<a download>` の download属性は**クロスオリジンでは無視される**。
  R2側に `Content-Disposition: attachment` を付ければ強制DL+正しいファイル名(URL末尾)になる。
  rclone は `--header-upload "Content-Disposition: attachment"` で付与。
- **CORSキャッシュ罠(前回知見・厳守)**: `VideoView` の video `poster` 属性は削除済のまま(再付与すると
  R2のVary:Origin + immutable で CORSブロック→真っ黒)。
- **日本語プロジェクト名**: `assetUrl()` が各パスセグメントを encodeURIComponent。R2キー `写真` も
  `%E5%86%99%E7%9C%9F` で 200 を確認。
- **button入れ子=不正HTML**: プレビューを `<button>` で包む中に VideoView の再生ボタンがあると
  hydration error。写真だけbutton包み・動画は素のまま(PreviewPane で分岐)。

## git / デプロイの判断
- easy-share は親 `~/projects`(git repo)直下に**未追跡**だった。`.gitignore` は easy-share ルート想定で
  書かれていた → **easy-share を独自リポジトリ化**(`git init`)して初コミット。親リポは汚さない。
- 大容量バイナリは git に入れない: `sample/`(RAW 1GB)・`web/public/sample/derivatives/`(250MB)を .gitignore 追加。
  本番はR2なのでデモ素材はgit不要。secrets/LUT も従来通り除外。**push はまだ**(YD許可待ち)。
- **Vercel エイリアスの罠**: `vercel --prod` は本番デプロイを作るが `easyshare-fx.vercel.app` は自動更新されない
  (別名 `web-ten-azure-33` に当たるだけ)。デプロイ後に
  `vercel alias set <deployment> easyshare-fx.vercel.app` が必要(手動運用)。本番ドメイン化すれば自動化可。

## 残課題
B案の色を実機でYD確認 / git push 可否 / カスタムドメイン化 / 旧look-*.mp4・Blob掃除 /
動画原本DL(config=all + Webゲート解除)。
