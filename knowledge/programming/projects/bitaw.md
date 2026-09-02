---
type: knowledge
domain: programming/projects
last_updated: 2026-09-02
status: active
trigger_keywords: [Bitaw, ビサヤ語, セブアノ語, Cebuano, 会話アプリ, claude -p relay, WhisperKit]
---

# Bitaw — ビサヤ語会話特化 iPhone アプリ (3人用)

パス: `~/AI projects/bitaw/` (git 管理、push 未)。引き継ぎ正本: 同ディレクトリの `HANDOVER.md`。仕様: `docs/SPEC.md`。決定: [[2026-09-02_Bitaw_ビサヤ語会話アプリ_設計確定]]。

## 構成

- `ios/` SwiftUI (iOS 17+, Xcode 26.6, xcodegen `project.yml`)。SPM: WhisperKit (argmaxinc/argmax-oss-swift 1.1.0) + supabase-swift 2.55。フォント Nunito (Duolingo の Feather Bold の代替、fontsource の woff2 を fonttools で ttf 化)
- `content/course.json` 教材正本 (97 フレーズ / 9 ユニット / 19 レッスン / シナリオ 6)。アプリは `Documents/course.json` があれば同梱より優先 (再ビルド不要の差し替え)
- `server/relay.mjs` Mac 常駐 Node サーバー。`POST /v1/roleplay` で `claude -p` (OAuth、API キー無し) を起動。launchd plist と Tailscale Funnel 手順は `server/README.md`
- `scripts/gen_content.py` (claude -p で教材追加) / `scripts/gen_audio.py` (ElevenLabs でお手本音声一括生成)
- `supabase/migrations/001_init.sql` profiles テーブル + RLS + XP 減少防止トリガー

## 画面と体験 (Duolingo 準拠)

パス (色付きユニットヘッダー + 左右にずれる丸ノード + スタート吹き出し) → 全画面レッスン (進捗バー、確認ボタン、緑/赤の判定パネル、間違いは最後に再出題) → XP と正確さ。問題は 聞いて意味を選ぶ / 日本語からビサヤ語を選ぶ / まねして言う / 日本語だけ見て言う / 返事する の 5 種。会話練習は相手のセリフ (ビサヤ語 + カタカナ + 日本語) に対してマイクで話し、文字起こしを直して送ると、返答と一緒に自分の発言の訂正とヒントが返る。ランキングは週間 XP (JST 月曜リセット)。

## claude -p を速く安く呼ぶ設定 (実測 2026-09-02)

| 設定 | cache_creation | 1 ターン |
|---|---|---|
| 素の `claude -p --system-prompt ...` | 131k〜137k tokens | 10〜36 秒 |
| + `--setting-sources "" --strict-mcp-config --mcp-config mcp.json` | 0 | 3 秒 (一言) |
| + `--json-schema` | 0 | 18〜20 秒 |
| プロンプトで JSON 指示 + 受信側検証 | 0 | 4〜8 秒 |

`mcp.json` は `{"mcpServers":{}}`。`--mcp-config '{}'` は不正扱い。`CLAUDE_CODE_SIMPLE=1` と `--bare` は OAuth を読まないので不可。`ANTHROPIC_API_KEY` は子プロセス環境から削除する。

## 教材パイプライン (2026-09-02 追加)

`scripts/batch_topics.py` に 23 場面のトピック表があり、1 場面 = 1 回の `claude -p` (opus、1〜2 分) でユニットを生成する。続けて `scripts/review_content.py` (ネイティブ視点で誤りだけ直す。最初の版は note を「〜である」調に書き換えてしまったので「Report ONLY real mistakes」に絞った) → `scripts/normalize_course.py` (カナの「・」を空白に、em dash 除去、version +1) → `scripts/validate_course.py` → `scripts/add_scenarios.py` (会話シナリオ追記)。`scripts/finish_after_review.sh` がレビュー完了を待って正規化・検証・ビルド・commit まで自動で行う。API が遅い時間帯は 1 チャンク 3〜5 分かかり 240 秒でタイムアウトすることがある (再実行で通る)。

## 翻訳タブと配布 (2026-09-02 夕方追加)

翻訳タブ: 日本語 (入力 or WhisperKit の日本語文字起こし) → relay `POST /v1/translate` → `server/translate_system.md` のプロンプトで {ceb, kana, ja, literal, breakdown[], grammar, pronunciation, alternatives[]}。音声は `GET /v1/tts?text=` が `ELEVEN_API_KEY` があれば ElevenLabs (Mac の `server/tts-cache/` に sha1 でキャッシュ、iPhone 側も Caches/tts にキャッシュ)、無ければ 404 でアプリがインドネシア語音声に落ちる。`AudioPlayer.speak()` が同梱 mp3 → relay TTS → 代読の順に試す。保存フレーズは SwiftData `SavedPhrase`、フレーズタブの先頭に出て「練習」(PracticeSheet = 単発のまねして言う)。

配布: Xcode に Apple ID を入れると `xcodebuild -allowProvisioningUpdates` が証明書と Provisioning を自動発行。App Store Connect のアプリ記録が無いと `exportArchive` は `missingApp` で落ちるので、Xcode Organizer の Distribute App (TestFlight Internal Only) で記録を作らせた (作成直後の GET が空で error 0 になるが記録はできている)。以後は `xcodebuild -exportArchive -exportOptionsPlist ExportOptions.plist` (destination upload) で自動アップロードできる。

Tailscale: tailnet は Apple ID (yitao0907@gmail.com) でサインイン。Google で入ると別 tailnet になり「node not found」。Funnel は `tailscale funnel --bg 8787` → `https://macbook-pro-2.tail60869d.ts.net`。

iPhone から Claude サブスク枠を直接使う手段は無い (Claude アプリを他アプリから呼べない)。Mac 中継が唯一の経路。渡航中は MacBook 持参 + 電源 + WiFi が前提。

## ✅ うまく行ったこと

- 初回ビルド成功、シミュレータ実操作で レッスン → 判定 → 会話練習 (relay 経由の訂正付き返答) まで確認
- Duolingo の配色トークン名 (feather / macaw / cardinal / eel / swan) をそのまま Swift の enum にした → 実装中に迷わない
- 3D ボタン (下辺 4pt の濃色 + 押下で 4pt 沈む) を ButtonStyle 1 つで共通化

## ❌ 詰まったこと

- WhisperKit のモデル DL 中に採点を呼ぶと未ロードエラー → 読み込み Task を保持して await する形に修正
- computer use で Simulator の下部をクリックすると Aqua Voice の不可視ウィンドウに当たる → osascript で Simulator を左上 (60,40) に移動して回避。キー入力は computer_type だと長押し扱いでアクセント候補が出る → osascript の keystroke を使う (スペースが落ちる)
- 会話相手の声は事前生成できない (動的文)。インドネシア語音声で代読中

## 📋 次回同じことをするときのチェックリスト

1. YD 作業の順番は HANDOVER.md (Team 署名 → ElevenLabs → relay 常駐 + Funnel → Supabase → TestFlight)
2. 教材を増やしたら `gen_audio.py` → 再ビルド。セブアノ語話者に一度読んでもらう
3. relay.log の `cost` が $0.5 級に戻ったら `--setting-sources` が効いていない
4. 発音判定の閾値は実機で 2〜3 フレーズ試してから決める (`SpeechScorer.passThreshold`)
5. シミュレータ検証は `defaults write com.yitaoding.bitaw relay.baseURL http://localhost:8787` で relay を直接指す
