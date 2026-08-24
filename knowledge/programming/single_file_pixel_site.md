---
type: knowledge
created: 2026-08-13
source: 2026-08-13 デート招待サイト制作セッション (IG reel @jenna.herself Db68GxHij4J の再現)
---

# 単一HTMLピクセルアートサイト (日本語フォント埋め込み) の作り方

バイラル系「Will you go out with me?」サイトの日本語クローンを1ファイルで作った時のノウハウ。
成果物: `~/Projects/date-invite/` (GitHub private `date-invite`)、Artifact公開済み。

## 構成パターン

- 日本語ピクセルフォント = **DotGothic16** (Google Fonts / OFL)。TTF 2.0MB を base64 で `@font-face` に直接埋め込む → 1ファイル完結、CSP制約 (Artifact) も回避
- ビルド: `index_src.html` に `__FONT_B64__` プレースホルダ → python3 one-liner で差し替え
- キャラは著作権回避のため**オリジナルのピクセル熊を文字列マップ+canvasで自作** (表情は座標オーバーレイ関数で差分描画)
- 背景 (街・丘・花・雲) はseeded乱数でcanvas描画、きらきら/ハートはrequestAnimationFrameのFXレイヤー
- カスタマイズ項目 (差出人名・手紙文面・選択肢) はスクリプト冒頭の `CONFIG` オブジェクトに集約

## ✅ うまく行ったこと

- ffmpegでリールから4秒毎フレーム抽出 → Readで画像確認 → サイトの全画面フロー (告白→YAY→日付→プラン→成立→love_note.txt) を正確に復元できた
- DotGothic16 TTF丸ごと埋め込み (base64 2.76MB) はサブセット化せず全文字対応 → 後からCONFIGの文面を書き換えても文字化けしない
- Chrome MCPでlocalhost経由の通しテスト → 実バグを2件その場で発見できた

## ❌ 詰まったこと

- **`<meta charset="utf-8">` を忘れて全文字化け**。Artifactはラッパーが補うがローカル/自前ホスティングでは必須。1ファイルHTMLでも必ず先頭に書く
- **`input type=date` は不完全入力でも通ることがある** → 成立画面が「NaN年NaN月NaN日」に。正規表現 `/^\d{4}-\d{2}-\d{2}$/` + `isNaN(new Date(...))` の二重チェックで解決
- file:// はChrome MCPで開けない → `python3 -m http.server` を挟む
- ピクセル雲を互い違いブロックで描いたらスペースインベーダーに見えた → 中央寄せの段積み矩形に修正

## 📋 次回同じことをするときのチェックリスト

1. 参照動画があればまず `ffmpeg fps=1/4` でフレーム抽出して全画面フローを書き出す
2. 日本語ピクセル表現は DotGothic16 一択 (Press Start 2P はラテン文字のみ)
3. `<meta charset>` → フォント埋め込み → 通しテスト (Chrome MCP + localhost) の順
4. キャラ素材は既存IP (Bubu/Dudu等) をコピーせず自作スプライトにする
5. ユーザー入力 (date等) は「空チェック」でなく「パース成功チェック」まで書く
