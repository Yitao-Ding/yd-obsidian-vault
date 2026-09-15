---
name: fcpxml-auto-edit-pipeline
description: 講演・説明会の撮影素材 + Canva スライドから、カット・スライド差し込み・顔追従の寄り/丸窓・字幕入りの FCP プロジェクトを FCPXML で自動生成する手順と、FCP 12.3 で実測した FCPXML の罠
metadata:
  type: knowledge
  created: 2026-09-15
  origin: ハタチたち5 説明会動画 (平成たち祭 vol.2、2026-09-14 収録) の編集
---

# FCPXML で説明会動画を自動編集する手順 (2026-09-15 ハタチたち5 説明会)

固定カメラ1台で撮った 10:52 の説明会 (FX30 4K 59.94p) と Canva のスライド 21 枚から、Apple 発表会風の編集を FCPXML として生成して Final Cut Pro に読み込んだ。作業一式は `/Volumes/Extreme pro/平成たち祭２/ハタチたち説明会&座談会/_cc_work/` (build_fcpxml.py が生成器、plan_shots.json がショット割り、cuts_final.json がカット、subtitles.json が字幕)。納品は同階層の `_FCP納品/`。

## 流れ

1. 音声を 16k mono に抜いて mlx-whisper large-v3-turbo で単語タイムスタンプ付き文字起こし (10分で31秒)。50ms RMS プロファイルと無音区間も出す
2. カットリストは文字起こしの区切りで下書き → 各 in/out を RMS で「発話の始端-0.12s / 終端+0.18s」に自動で寄せる。噛み・言い直し・「次のページお願いします」・1.3秒超の間を除去。10:52 → 8:39
3. Canva は共有リンクだと MCP の export / copy が権限で拒否される。read-design (テキスト・サムネ) は通る。書き出しは YD の Chrome で共有 > ダウンロード (PNG 全ページ、MP4 全ページを1本)。MP4 は静止ページ 5 秒・動画ページは動画長で並ぶので、PNG との一致でページ境界を割る (似た動画が続くページは目視で境界確定)
4. 動画ページは時間方向の標準偏差で動く領域を検出して切り出し、縦動画はぼかし背景で 16:9 に。ページの冒頭 1.2 秒は Canva の入場アニメなので捨てる
5. 話者の顔トラッキングは YuNet (opencv `FaceDetectorYN`、opencv_zoo の onnx) が Haar より圧倒的に良い (79% 検出)。Haar で穴埋め、外れ値除去、中央値 + Savitzky-Golay で仮想カメラの軌道にする。検出が数秒以上途切れる区間 (話者が画面外や後ろ向き) は寄り/丸窓を使わないショット割りにする
6. 丸窓は ffmpeg の sendcmd で crop の x,y を毎フレーム更新して切り出し、円マスクを alphamerge、1920x1080 の透過 ProRes 4444 に焼く (右下版・左下版の2ファイル)
7. FCPXML を生成 → xmllint で FCP 同梱の DTD (`Final Cut Pro.app/Contents/Frameworks/Interchange.framework/Versions/A/Resources/FCPXMLv1_13.dtd`) に対して検証 → `open -a "Final Cut Pro" x.fcpxml` で読み込み → 再生ヘッドを Control+P + タイムコードで飛ばして screencapture で実画を確認

## うまく行ったこと

- 単語タイムスタンプ + RMS で決めたカット点は、FCP で聴き直さなくても切れ目が自然だった
- スライド全面のときに、動画入りページは PNG ではなく Canva 書き出し MP4 の該当区間を接続すると、ページ内の動画が動いた状態で出せる
- 字幕はベーシックタイトル (`uid=".../Titles.localized/Bumper:Opener.localized/Basic Title.localized/Basic Title.moti"`) に text-style (font="Hiragino Sans" fontFace="W6") を付ける形でそのまま出た。位置は param name="Position" key="9999/999166631/999166633/1/100/101"
- FCP の GUI 操作は System Events (ダイアログのボタン) + Quartz の CGEvent クリック (Terminal が前に出ていると座標クリックが Terminal に当たるので、先に `tell application "Final Cut Pro" to activate`) で回った

## 詰まったこと

- **FCPXML の `adjust-transform position` の単位は px ではない**。FCP 12.3 のインスペクタで実測したところ XML の値 × 10.8 (= 1080/100、プロジェクト高さの 1/100) が px 表示になった。px のつもりで書くと寄りが真っ黒 (画面外) になる
- **`adjust-conform type="none"` で小さい素材を等倍に置くのは効かなかった** (320px の丸窓が画面いっぱいに拡大された)。透過素材はプロジェクトと同じ 1920x1080 で焼いて変形なしで置くのが確実
- **静止画 (`video` 要素) のキーフレームに interp / curve 属性を付けると param ごと無視される** (「キーフレームの補間属性をサポートしていない」)。属性を書かないこと
- **接続クリップの offset は「項目は編集フレームの境界上にありません」の警告が出る**。59.94p 素材を 30p に入れたとき、親の start と子の offset を 1001/60000 でも 2002/60000 でも 100/3000 でも警告は消えなかった。FCP が最寄りフレームに寄せるので実害はない
- **`open -a "Final Cut Pro" x.fcpxml` で FCP を起動すると、読み込み後にメインウインドウが無い状態で止まる**ことがある。その状態で kill すると読み込んだイベントがライブラリに登録されずフォルダだけ残る。先に FCP を普通に起動してライブラリを開いてから XML を open する
- Canva の SIFT 特徴で投影スクリーンの四隅を求めようとしたが、ページごとに結果が 100〜250px ずれた。イベント後にスライドが編集されていて、収録時の投影内容と現行 PNG が一致しないため。スクリーン差し替え自体は、話者がスクリーンの前に立つ時間が 88% だったので見送った
- 自分の tracking の中間結果を CSV に書くとき、クラス名 `st` を AppleScript の変数に使うと予約語で落ちる。細かいがハマる

## 次回同じことをするときのチェックリスト

1. FCP を先に起動してライブラリを開いておく。それから `open` で fcpxml
2. position は px / 10.8 で書く。丸窓などの透過素材はプロジェクト解像度で焼く。キーフレームに interp / curve を付けない
3. 読み込んだら Control+P でタイムコードを打って viewer を screencapture し、寄り・丸窓・字幕・エンドカードの5〜10点を必ず目視する (v1〜v4 は全部これで問題が見つかった)
4. Canva は共有リンクのままでは MCP で書き出せない。Chrome の共有 > ダウンロードで PNG と MP4 を落とす。MP4 のページ境界は PNG 一致で割り、似た動画が並ぶページは export のコンタクトシートで目視
5. 顔トラッキングは YuNet。検出が途切れる区間は寄り/丸窓を計画から外す (ショット割りを track_gaps で自動チェック)
6. 話者がスクリーンの前に立つ収録では、スクリーン差し替えは最初から諦める
