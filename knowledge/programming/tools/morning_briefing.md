---
type: knowledge
created: 2026-05-19
last_updated: 2026-05-19 (API依存撤廃版に書き換え)
tags: [tool, automation, briefing, claude-code-headless, macos-say, google-drive, cron, max-20x]
---

# morning-briefing — 朝ブリーフィング自動配信パイプライン (Max 20x 完結版)

> 毎朝 7:30 JST に走り、ニュース・ポッドキャスト・推薦書・推薦コースを **Claude Code (`claude -p`) で整形** → 縦長 PDF + **macOS `say -v Kyoko` TTS mp3** → Google Drive に自動アップする YD 専用パイプライン。**API課金ゼロ**。

## 概要

- **パス**: `~/projects/morning-briefing`
- **言語**: Python 3.11 (uv 管理)
- **主要依存**: feedparser / weasyprint / jinja2 / mutagen / google-api-python-client
  - **anthropic / openai パッケージは依存から削除済み** (API課金回避)
- **LLM**: `claude -p` ヘッドレス実行 (Max 20x 枠内)
- **TTS**: macOS 標準 `say -v Kyoko` + ffmpeg
- **トリガー**: `crontab` 経由で 07:30 JST 毎朝 (handler: `run.sh`)
- **出力**: Google Drive `Morning Briefing/2026-MM/2026-MM-DD_morning_briefing.{pdf,mp3}`
- **関連意思決定**: [[2026-05-19_AI学習スプリント開始]] / [[2026-05-19_API依存撤廃_Max20x完結化]]

## アーキテクチャ

```
config.yaml (監視対象)
    ↓
[collectors]  RSS / Web 取得
    ├ news.py        ニュース 3 カテゴリ (AI/撮影/開発)
    ├ podcast.py     ポッドキャスト 8 番組 (英5+日3)
    ├ books.py       Gates Notes + 古典ローテーション (哲学/経営/文学/経済/科学)
    └ course.py      Vault 進捗 → 次の Anthropic Academy コース
    ↓
[synthesizer]  subprocess('claude','-p', prompt) → JSON 抽出 → fallback あり
    ↓
[renderer]
    ├ pdf.py   Jinja2 + WeasyPrint → 縦長 A4 雑誌風 PDF
    └ tts.py   `say -v Kyoko -o aiff` → ffmpeg → mp3 + ID3 タグ
    ↓
[uploader]  Google Drive OAuth2 (scope: drive.file) で月フォルダにアップ
```

## ディレクトリ

```
morning-briefing/
├── src/
│   ├── collectors/     base.py, news.py, podcast.py, books.py, course.py
│   ├── synthesizer/    briefing.py        (subprocess claude -p)
│   ├── renderer/       pdf.py, tts.py     (WeasyPrint + say + ffmpeg)
│   ├── uploader/       drive.py           (OAuth2)
│   ├── utils/          config.py, logger.py, http.py
│   └── main.py         Typer CLI エントリ
├── templates/
│   ├── briefing.html.j2
│   └── briefing.css    雑誌風 (ネイビー+ゴールド)
├── config.yaml         RSS URL・TTS設定 (voice/rate/bitrate/max_chars)
├── .env.example        BRIEFING_OWNER のみ (APIキー不要)
├── credentials/        Google OAuth client_secret.json + token.json (gitignored)
├── output/             PDF / mp3 / raw.json (gitignored)
├── logs/               cron.log + briefing.log (gitignored)
├── run.sh              cron 用 (DYLD_FALLBACK_LIBRARY_PATH を設定)
└── install_cron.sh     crontab 登録/解除/表示
```

## ✅ うまく行ったこと

- **API課金ゼロ設計の達成**: 当初 Anthropic + OpenAI TTS で月$80-100 想定だったが、`claude -p` + `say -v Kyoko` で完全撤廃。Drive API のみ残るが OAuth2 で個人枠
- **`claude -p` ヘッドレス実行が一発で動いた**: subprocess.run で 8.5K 文字のプロンプト渡して 38秒で日本語 JSON が返る。Max 20x で動く点・stdout からそのまま JSON が取れる点・履歴を持たないのでセッション衝突しない点が地味に大きい
- **macOS `say -v Kyoko` の音質が十分**: AIFF 169KB → MP3 128k で 3.7MB、22.05kHz モノラル、聞き流し用途として違和感なし。OpenAI tts-1-hd と比較して自然さは劣るが、毎朝の情報インプットには問題なし
- **書き換え工数が小さかった**: synthesizer 1ファイル + tts 1ファイル + 設定3ファイルの差し替えで完了 (推定3時間 → 実時間1時間)。「API呼び出し部分だけ差し替え」の意思決定が効いた
- **fallback パターン**: `claude -p` 失敗時は raw データを PDF にして出す。配信止めない設計
- **iTunes Search API による RSS 解決**: 古い anchor.fm URL は全部死んでいたが、Apple Podcast ID から `https://itunes.apple.com/lookup?id=XXX&entity=podcast` で正しい RSS URL に逆引きできた。COTEN RADIO / Off Topic / a scope すべてこの方法で解決
- **WeasyPrint 縦長 A4 + Hiragino フォント**: フルカラー、雑誌風レイアウト、cover ページのネイビー+ゴールド配色が一発で決まった
- **生成結果の質**: 推薦書 why_jp が「Salamatの運営や映像制作で迷う場面の倫理軸」、closing が「昭和OSが沈む朝、自分のOSは何で書き直しますか」のように YD の現在のフェーズに絡む文章を Claude が出してくる

## ❌ 詰まったこと

- **設計初版が API 前提で組まれた**: 朝に YD から「APIで動かすAIは極力使わないで欲しい」の指摘 → 大幅書き換え。デスクトップ Claude が「Max 20x 使い切れない」という根本動機を忘れて API 課金前提で設計したのが原因 (詳細は [[2026-05-19_API依存撤廃_Max20x完結化]])
- **WeasyPrint の dyld エラー (Apple Silicon)**: `OSError: cannot load library 'libgobject-2.0-0'` で起動失敗。`pango`/`cairo`/`glib` は brew でインストール済みなのに見つからない。解決: `DYLD_FALLBACK_LIBRARY_PATH=/opt/homebrew/lib` を `run.sh` 冒頭に追加。cron は PATH が痩せるので明示が必須
- **RSS URL の半数が死んでいた**: Anthropic 公式 (404)、B&H Explora (403 UA ブロック)、GitHub Trending atom (406)、Megaphone all-in (404)、anchor.fm の COTEN/Off Topic/a scope (URL 古い)。修正先:
  - Anthropic: コミュニティ運営 RSS (`taobojlen/anthropic-rss-feed`)
  - GitHub Trending: コミュニティ運営 (`mshibanami.github.io/GitHubTrendingRSS`)
  - B&H: 削除 (No Film School + cinema5D で代替)
  - All-In: `https://allinchamathjason.libsyn.com/rss`
  - 日本語 3 番組: iTunes API から逆引き
  - 西野亮廣エンタメ研究所: Voicy 専用で RSS なし → 除外
- **Google Drive 認証はインタラクティブ**: 初回は OAuth フロー (`uv run python -m src.uploader.drive --auth`) でブラウザが開く。cron 自動実行のためには、初回のみ YD が手動で済ませる必要がある (リフレッシュトークンで以降は自動)
- **`claude -p` の応答時間が読めない**: 今回は 38 秒だったが、Max 20x の混雑状況・モデル選択によっては数分かかる可能性。タイムアウトは 600 秒に設定 (10 分)

## 📋 次回ゼロから同じものを作る場合のチェックリスト

### 事前準備

- [ ] Homebrew で `pango cairo glib harfbuzz pixman fribidi libffi ffmpeg` を確認 (`brew list | grep -iE 'pango|ffmpeg'`)。Apple Silicon は brew が `/opt/homebrew/lib` に置く点だけ注意
- [ ] **API キーは取得しない** (Anthropic / OpenAI とも不要)。Max 20x 枠と macOS `say` で完結する設計を最初から守る
- [ ] `which claude` / `which say` / `which ffmpeg` で全部利用可能を確認
- [ ] Google Cloud Console で新規プロジェクト → Drive API 有効化 → OAuth 同意画面「外部・テスト」+ テストユーザー `save.yitao@gmail.com` → OAuth クライアントID「デスクトップ」を作成、JSON を `credentials/client_secret.json` に保存

### セットアップ手順

```bash
mkdir -p ~/projects/morning-briefing && cd ~/projects/morning-briefing
uv init --python 3.11
# pyproject.toml に依存を書き込む (anthropic/openai は入れない)
uv sync
uv run python -m src.uploader.drive --auth  # ブラウザ認証
```

### 動作確認

```bash
# まず dry-run (TTS/upload なし)
DYLD_FALLBACK_LIBRARY_PATH=/opt/homebrew/lib uv run python -m src.main --dry-run

# upload なし + TTS あり
DYLD_FALLBACK_LIBRARY_PATH=/opt/homebrew/lib uv run python -m src.main --skip-upload

# フル実行
./run.sh
```

### cron 登録

```bash
./install_cron.sh         # 07:30 JST に登録
./install_cron.sh --show
./install_cron.sh --remove
```

### 落とし穴 (先回りで防げる失敗)

1. **「API 課金 = LLM処理」の常識を持ち込まない**: Max 20x ユーザーには `claude -p` がある。最初から API キー前提で設計しない
2. **DYLD_FALLBACK_LIBRARY_PATH を忘れる** → WeasyPrint が import 失敗。`run.sh` で必ず設定。cron の PATH は痩せるので `/Users/ittou/.local/bin` (uv) と `/opt/homebrew/bin` (brew/ffmpeg/say は標準) を `run.sh` で明示
3. **`claude -p` のプロンプトに「JSON のみ返せ」を強く書く**: 前置きや後書きが入ると `_extract_json` が拾うが、空白の `}` を含むなど exotic な出力に弱い。「JSON 以外のテキスト・コードフェンス禁止」を必ず明記
4. **RSS URL を信用しすぎる** → 半数死んでる。新規追加時は必ず `curl -I` で 200 確認
5. **anchor.fm の URL は短期 ID**: 番組の引っ越し・配信プラットフォーム変更で死ぬ。iTunes Search API で逆引きする手順を README に書いておく
6. **TTS の URL/英語固有名詞**: そのまま `say` に渡すと不自然。`<break/>` を入れた後 `_normalize_for_tts` で URL を除去、長さも max_chars (=1500) でクリップ
7. **say の文字数上限**: 実測 1500 字程度で問題なし。長すぎると AIFF が肥大化して ffmpeg 変換に時間がかかる
8. **Drive OAuth スコープは `drive.file` 限定**: アプリが作ったファイルのみアクセスできる安全側スコープ。`drive` フルスコープを取らない (個人 Drive を読みすぎる)
9. **cron 暴走対策**: `set -euo pipefail` を `run.sh` に入れる。タイムアウトは入れていないが、Python レベルで `requests` に 20 秒 timeout、subprocess に 600 秒 timeout を設定済み

### YD が運用中に手を入れる箇所

- 監視対象を変えたい → `config.yaml` の `news` / `podcasts` セクションを編集
- TTS の声・速度 → `config.yaml` の `tts.voice` (Kyoko/Otoya/Tina)、`tts.rate` (180 推奨、Kyoko は 170-200 が自然)
- 推薦書ソース → `~/ObsidianVault/reading/` を作って `status: unread` メタを書いておくと、そこから優先選択
- LLM モデルを変えたい → 呼び出し側 Claude Code の `/model` 設定で切り替わる (sonnet/opus/haiku)

## 計測値 (2026-05-19 初回フルテスト)

- 全体: 61.9 秒
  - 収集: ~22 秒 (RSS 9 件)
  - claude -p 整形: ~38 秒 (プロンプト 8.5K 文字、Max 20x、出力 JSON 完全)
  - PDF レンダリング: ~1 秒
  - say + ffmpeg: ~1 秒
- 出力:
  - PDF: 247KB (cover + 6 ページ)
  - MP3: 3.68MB (128kbps モノラル 22.05kHz、約 4 分 30 秒)

## 関連

- [[2026-05-19_AI学習スプリント開始]] — 仕様の出所、3並列セッションの一部
- [[2026-05-19_API依存撤廃_Max20x完結化]] — 設計方針転換 (本書の基盤)
- [[claude_code_permissions]] — Push/Deploy/sudo/rm の確認ポリシー
- [[vidkit]] — uv + pyproject の参考プロジェクト
- [[textbook_engine]] — WeasyPrint パターンの兄弟プロジェクト
- [[obsidian_vault]] — Vault 連携 (reading/, learning/ai_certifications/)
- 設定ファイル本体: `~/projects/morning-briefing/config.yaml`
- 実装: `~/projects/morning-briefing/src/`
