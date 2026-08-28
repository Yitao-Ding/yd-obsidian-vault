# YDのChromeのログイン状態を引き継いで Claude が操作する方法

作成: 2026-08-28 (ハタチたち用ドメイン取得 お名前.com 操作で確立)

## 何のための手順か

Playwright MCP も chrome-devtools MCP も、それぞれ専用のブラウザを起動するので YD が普段使っている Chrome のログインセッションを持っていない。「いま自分がログインしているサービスをそのまま操作してほしい」という依頼はこれで詰まる。

Chrome 136 以降はセキュリティ上、既定のプロファイルに対して `--remote-debugging-port` を無視する。そのため「普段の Chrome をそのままデバッグ接続する」ことはできない。

## 手順

1. セッションに必要なファイルだけを一時ディレクトリにコピーする (プロファイル全体は 3GB あるので丸ごとコピーしない)

```bash
SRC="$HOME/Library/Application Support/Google/Chrome"
DST="/tmp/cc-chrome-work"
mkdir -p "$DST/Default"
cp "$SRC/Local State" "$DST/Local State"
for f in Cookies "Cookies-journal" "Login Data" Preferences "Web Data" "Secure Preferences"; do
  cp "$SRC/Default/$f" "$DST/Default/$f" 2>/dev/null
done
```

約4MB で済む。Cookie の暗号鍵は macOS Keychain の "Chrome Safe Storage" にあり、プロファイルの場所ではなくアプリに紐づくので、コピー先でも復号できる。

2. 別インスタンスとして CDP 有効で起動する。稼働中の Chrome は触らない

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --user-data-dir=/tmp/cc-chrome-work --remote-debugging-port=9222 \
  --no-first-run --no-default-browser-check about:blank &
curl -s http://127.0.0.1:9222/json/version
```

3. 依存ゼロの CDP ドライバで操作する。Node 22+ は WebSocket を組み込みで持つのでパッケージ導入が要らない。実装は `/tmp/cdp.mjs` の型 (nav / eval / click / shot / size / text)

## ✅ うまく行ったこと

- Cookie のコピーだけでログイン状態が完全に引き継げた。Playwright/chrome-devtools MCP の両方が未ログインで詰んだ状況を回避できた
- Node の組み込み WebSocket で CDP を直接叩けるので puppeteer の導入が不要。100行程度で足りる
- 複数タブが復元される場合に備えて、対象タブを URL 部分一致 (`CDP_MATCH`) と targetId (`CDP_TARGET_ID`) で選べるようにしたのが効いた。同じ URL のタブが2つできた時に取り違えを防げる

## ❌ 詰まったこと

- **非アクティブなタブは Chrome に凍結され、`Runtime.evaluate` が返ってこない**。接続直後に `Page.enable` / `Runtime.enable` / `Page.bringToFront` を送ると解決する
- ウィンドウが狭いと右側のボタンが画面外になり `offsetParent` が null になって「ボタンが無い」と誤認する。`Emulation.setDeviceMetricsOverride` で 1440x900 に広げてから探す
- `element.click()` が効かないボタンがある (JS で組まれた submit)。`getBoundingClientRect()` で座標を取って `Input.dispatchMouseEvent` で実マウスクリックを送ると通る
- `textContent.trim() === 'ラベル'` の完全一致は、内部に span やアイコンがあると外れる。部分一致で探す
- サブドメインをまたぐとセッションが効かないことがある。お名前.com は `navi.onamae.com` は認証済みでも `cart.onamae.com` に直接行くとログインを求められた。**Navi の中の導線からカートに入ると認証が引き継がれる**。行き詰まったら「認証済みの画面から辿る」を試す

## 📋 次回同じことをするときのチェックリスト

1. まず `lsof -nP -iTCP -sTCP:LISTEN | grep 9222` で既存のデバッグポートが無いか確認する
2. プロファイルは丸ごとコピーしない (3GB)。Local State + Default の6ファイルだけ
3. 起動したら `curl http://127.0.0.1:9222/json/version` で疎通確認
4. ドライバは接続直後に Page.enable / Runtime.enable / Page.bringToFront を必ず送る
5. ビューポートを 1440x900 以上にしてから要素を探す
6. クリックが効かなければ座標指定の実マウスイベントに切り替える
7. 認証を要求されたら、直接URLではなく認証済み画面の導線から辿り直す
8. **決済・パスワード入力は絶対に代行しない**。カード番号やパスワードは YD 本人が入力する。そこまで組み立ててウィンドウを前面に出し、YD に渡す
9. 作業後は一時プロファイル (`/tmp/cc-chrome-work`) を消す。Cookie が平文に近い形で残るため

## 関連

- [[playwright_mcp]] / [[mcp_local_servers]] — 通常のブラウザ自動化はこちら。ログイン状態が要らないならそれで足りる
