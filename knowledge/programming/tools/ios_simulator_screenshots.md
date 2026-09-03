---
type: knowledge
domain: programming/tools
last_updated: 2026-09-03
status: active
trigger_keywords: [iOS, simulator, screenshot, App Store, simctl, status_bar]
---

# iOS Simulator App Store スクショ撮影手順

> App Store 提出用スクリーンショットを simulator から撮る際の手順。
> project-agent-application C-15 (2026-09-03) で確立。

## 基本手順

1. **status_bar をクリーン状態に固定**

```bash
# 充電マークを消す (discharging にすると充電アイコンが消える)
xcrun simctl status_bar booted override \
  --time "9:41" \
  --batteryState discharging \
  --batteryLevel 100 \
  --wifiBars 3 \
  --cellularBars 4 \
  --dataNetwork 5g-uc
```

2. **各画面に遷移してスクリーンショット撮影**

```bash
xcrun simctl io booted screenshot output.png
```

3. **status_bar をリセット**

```bash
xcrun simctl status_bar booted clear
```

## ターゲットサイズ

- iPhone Pro Max (6.7 inch): 1320×2868 px
- `sips -Z 800 input.png --out output_thumb.png` でサムネ確認

## 複数画面の一括撮影スクリプト例

```bash
D=/tmp/screenshots
shot(){
  sleep 3
  xcrun simctl io booted screenshot $D/$1.png >/dev/null
  sips -Z 800 $D/$1.png --out $D/${1}s.png >/dev/null
}
# 各画面へ cliclick で遷移 → shot <番号> を繰り返す
```

## cliclick での座標タップ

シミュレータウィンドウの実座標を取得してからタップする:

```bash
# Simulator ウィンドウの位置・サイズを取得
osascript -e 'tell application "System Events" to tell process "Simulator" to get {position, size} of window 1'
# → {x, y}, {w, h} が返る。画面の左上が (x, y) なので、
#    タップしたい相対座標を加算して cliclick に渡す
cliclick c:<absolute_x>,<absolute_y>
```

## ✅ うまく行ったこと

- `--batteryState discharging` で充電アイコンが消え、クリーンな外観になる
- `--time "9:41"` は Apple 公式スクショ標準時刻 (ベゼルと数字の見映えが最も良い)
- status_bar override は `xcrun simctl status_bar booted clear` で簡単にリセットできる

## ❌ 詰まったこと

- `cliclick c:x,y` はシミュレータが前面にないと効かない → `osascript -e 'tell application "Simulator" to activate'` で前面化してから実行
- ウィンドウ座標の絶対値がセッションごとに変わる → 毎回 `osascript` で取得し直す
- `--batteryLevel 100 --batteryState charging` だと充電アイコン (稲妻) が残る → `discharging` を使う

## 📋 次回同じことをするときのチェックリスト

1. `xcrun simctl status_bar booted override` を撮影前に実行 (忘れると充電アイコンや時刻がバラバラ)
2. `--batteryState discharging` を使う (`charging` は稲妻アイコンが出る)
3. Simulator を前面化してから cliclick タップ
4. 撮影後 `simctl status_bar booted clear` でリセット
5. App Store に入れる前に sips でサイズ確認 (Pro Max なら 1320×2868)

## 関連

- project-agent-application C-15 (2026-09-03): 6 枚を一括撮影して commit f443bbe に含めた
