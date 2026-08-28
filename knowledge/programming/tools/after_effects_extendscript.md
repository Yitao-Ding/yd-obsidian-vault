# After Effects ExtendScript (Claude Code 経由)

## 概要

Claude Code の after-effects スキルが osascript 経由で ExtendScript を AE に送る仕組み。AE の UI 操作・レンダーキュー制御を自動化する。

## osascript のタイムアウト問題

osascript (AppleScript → ExtendScript ブリッジ) はデフォルト約2分でタイムアウトし exit code 1 を返す。**AE 内の処理は継続している**。

- 4K 130秒 (約7,800フレーム) のレンダーは osascript タイムアウト後も AE が走り続けていた
- タスク通知で「failed」と報告されても即座に諦めない

### 確認方法

```bash
# AE プロセスが生きているか
ps -o %cpu=,comm= -p <AE_PID>

# 出力ファイルが育っているか
stat -f%z "/path/to/output.m4v"
```

### 正しいレンダー監視パターン

```bash
F="/path/to/output.mp4"
for i in $(seq 1 120); do
  if [ -f "$F" ]; then echo "RENDER FINISHED: $F"; break; fi
  sleep 15
done
```

- 出力ファイルが `.m4v` (中間) → `.mp4` (完成) に変わった時点で完了
- 15秒間隔ポーリングで最大30分待つ設計が安定

## AE レンダー設定 (日本語版 AE)

日本語版 AE での設定名:

- レンダー設定: 「最良設定」
- 出力モジュール: H.264 15Mbps のテンプレートが入っている場合はそれを使う

`プペル_エンドロール_v2.aep` で確認済みの設定:

| コンプ | 解像度 | fps | 尺 | 出力 |
|--------|--------|-----|----|------|
| PartA_v2 | 3840×2160 (書き出し1/2=1920×1080) | 59.94 | 130s | H.264 / 33MB |
| BG_v2 | 3840×2160 | 59.94 | - | - |
| PartB_v2 | 3840×2160 | 59.94 | - | - |

## 日本語版AE の ExtendScript 注意点

日本語版AEでは `layer.property("Source Text")` のような**表示名でのプロパティアクセスが失敗する**。matchName を使う必要がある。

```javascript
// NG: 日本語版AEでは表示名が日本語になっておりアクセス失敗する
var td = layer.property("Source Text");

// OK: matchName は言語に依存しない
var td = layer.property("ADBE Text Document");
```

確認済みの matchName:
- `ADBE Text Document` — テキストレイヤーの文字列
- `ADBE Layer Overrides` — エッセンシャルプロパティ (AE CC 2018以降)

## Remotion + AE ハイブリッドワークフロー

コードで背景を生成している Remotion プロジェクトを AE で直接編集可能にする手順。

背景を動画として書き出す: コード生成の要素 (パーティクル・乱流・Perlin 背景等) は AE で再現するより Remotion でそのまま書き出すほうが速い。

```bash
# Remotion で背景コンポだけをレンダー (4K ProRes が推奨)
npx remotion render BgOnly /path/to/BG.mov \
  --codec=prores \
  --prores-profile=4444 \
  --width=3840 --height=2160
```

その上の要素 (テロップ・カード・クレジット) は ExtendScript で AE レイヤーとして生成する。

プペルエンドロール v17_AE の構成:
- `ENDROLL_v17_AE` — マスター 123秒
- `ROLL_中身` — 演目テロップ141レイヤー + カード67レイヤー (230レイヤー計)
- `PARTB_メイキング` — メイキング10カット + 下端グラデ (32秒)

レイヤー命名規則: `演目番号_種別_コンテンツ` (例: `1-1_title_Hi,Me:) number`, `カード枠_1-1`)。この命名で AE の検索機能から直接絞り込める。

## ✅ うまく行ったこと

- osascript タイムアウト後にファイルポーリングに切り替えたら、レンダー完了を正確に検知できた
- `runner.sh --background` でバックグラウンド実行し、タイムアウトを Task 側に逃がす設計が有効

## ❌ 詰まったこと (追記: 2026-08-28)

- `--background` フラグなしだと Claude のメイン処理がブロックされる (osascript が返らない)
- `runner.sh --background` でも Task が exit 1 で終了する → これは osascript タイムアウトであり、AE のレンダー失敗ではない
- 出力ファイルが `.m4v` (一時ファイル) として生成されてから `.mp4` (完成) に切り替わるタイミングがあり、`.m4v` の存在だけで完了と判断すると早すぎる
- 日本語版AEで `layer.property("Source Text")` が失敗: matchName に切り替えが必要 (上述)
- ExtendScript が重い (レイヤー数230超) 場合、osascript タイムアウトが発生しても AE 内では処理継続。`.aep` ファイルのポーリングで完了を検知するのが正解

## 📋 次回同じことをするときのチェックリスト

1. AE を起動済みの状態で実行する (起動からは別途処理が必要)
2. `runner.sh --background` でスクリプトを送る
3. Task の exit code 1 は無視し、出力ファイルのポーリングに切り替える
4. AE プロセス PID と出力ファイルパスを記録してから監視ループを回す
5. 完了後 ffprobe で duration / size を検証し、念のため代表フレームをスクリーンショット確認
