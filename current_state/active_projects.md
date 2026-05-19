---
type: current_state
last_updated: 2026-05-19 (AI学習スプリント開始 + 朝ブリーフィング基盤完成 + 教科書システム第1号完成、就活終了)
update_frequency: 週1回以上
---

# 進行中プロジェクト一覧

> このファイルはYDの「今アクティブに動いてるもの」のスナップショット
> 完了したものは `archive/` へ、休眠中は `archive/sleeping/` へ移動

## 🟢 アクティブ・優先度高

### ★ AI学習スプリント (2026-05-19 開始、最重要) ★

- **状況**: ✅ 学習基盤構築完了 (セッションC、2026-05-19 03:00) — `learning/` 配下 28ファイル
- **目標**: 4資格取得 + 朝ブリーフィングで継続的なインプット
  1. Anthropic Academy 全18コース (2026-05-19 → 06-02、2週間スプリント)
  2. Claude Certified Architect Foundations (60問、5領域、2026-06中旬)
  3. Google AI Professional Certificate (Coursera、3ヶ月Google AI Pro付き、2026-06-16 → 07-31)
  4. Google Cloud Generative AI Leader ($99、90分、2026-08、余裕あれば)
- **次のアクション** (Week 1):
  - [ ] **Day 1 (今日 5/19)**: AI Capabilities and Limitations + AI Fluency: Framework & Foundations
  - [ ] Day 2 (5/20): Claude 101 + Introduction to Claude Cowork
  - [ ] Day 3 (5/21): Claude Code 101 + Claude Code in Action
  - [ ] Day 4-7: API → MCP → MCP Advanced → Agent Skills + Subagents
  - [ ] Week 2 (5/26 → 6/2): Cloud (Bedrock/Vertex) + AI Fluency 各業界版
- **受講メアド**: `save.yitao@gmail.com` (AI系専用)
- **進捗管理**: `learning/ai_certifications/` 配下、各コースの frontmatter `status:` で機械可読化
- **関連**:
  - [[../learning/README]]
  - [[../learning/ai_certifications/README]]
  - [[../learning/ai_certifications/anthropic_academy/README]] (18コース全体マップ)
  - [[../decisions/2026-05-19_AI学習スプリント開始]]
  - [[../knowledge/programming/tools/textbook_engine]] — 教科書システム (セッションA で構築済み、運用へ)

### 1. Obsidian Vault 構築 (今このタスク)
- **状況**: Claude Code に渡す設計書作成中
- **次のアクション**: Claude Codeで `~/ObsidianVault/` を構築
- **完了基準**: 新セッションで「YDの状況を要約して」と聞いて適切に応答できる
- **関連**: `~/Downloads/obsidian-vault-setup/`

### 2. vidkit (動画前処理CLI)
- **状況**:
  - dance モード完成 (TIME Instagram_最終２.mp4 で実機テスト済み)
  - **autocut モード完成 (2026-05-18)** — FCP用無音カット FCPXML 1.13 出力、lecture/vlog 2プリセット、Skill `~/.claude/skills/fcp-autocut/` 登録済み
  - **tighten モード完成 (2026-05-19)** — 既存FCPプロジェクトの各クリップ内の残り無音を再カット、合成テストFCPXMLで検証済 (1clip→3clips、3.6s削除、xmllint通過)
  - **tutorial モード完成 (2026-05-19)** — URL/ローカル自動判別、dance パイプライン相乗りで Claude Code が自走実装する PROMPT.md を生成
  - **--vault-path オプション完成 (2026-05-19)** — `<vault>/raw/vidkit/<mode>/` への出力に全モード対応
  - lecture モードは未完成 (pyannote HF_TOKEN 待ち)
- **次のアクション** (優先度順):
  - [ ] **★ lecture モード仕上げ** (pyannote HF_TOKEN セットアップ → YD作業) — HuggingFace で `pyannote/speaker-diarization-community-1` のリクエスト承認 → `.env` に `HF_TOKEN` 設定
  - [ ] 実 FCP プロジェクト (例: 平成たち祭・蛹) を Export XML → tighten 実機検証
  - [x] **tighten/tutorial の Skill 化完了** (2026-05-19 00:52) — `~/.claude/skills/fcp-tighten/SKILL.md` (4.7KB) / `~/.claude/skills/video-tutorial/SKILL.md` (5.2KB)
  - [x] **vidkit を git 初期化 + GitHub Private に push 完了** (2026-05-19 ~01:00) — `177a2f2 Initial commit` → `29deb68 docs: lecture セットアップ + tighten 実機検証手順を追加` (~01:05) → **`40cef7d fix: variable-fps 動画 (Zoom録画/screen capture) を autocut/tighten で扱えるように` (~02:00)**。origin = https://github.com/Yitao-Ding/vidkit.git、現在クリーン
- **将来のFCPXMLオペレーション候補**: speaker-filter / marker-batch / beat-snap (蛹用途) / roles-bulk — `parse_fcpxml` + `write_fcpxml_from_parsed` の汎用モジュールが揃ったので追加コストは低い
- **パス**: `/Users/ittou/projects/vidkit`
- **関連**: `knowledge/programming/tools/vidkit.md`, `decisions/2026-05-18_FCPXML_ラウンドトリップ採用.md`, `decisions/2026-05-19_vidkit_tighten_tutorial_完成.md`

### 3. 平成たち祭 動画制作
- **状況**: 撮影完了 (5/6)、DaVinciプロジェクト修復が必要だった
- **次のアクション**:
  - [ ] Hi,Me:) 「さなぎ」DaVinci ファイル修復確認
  - [ ] リサーチレポートを踏まえた構成案決定
  - [ ] 編集・カラーグレーディング
- **リサーチ済**: `~/Downloads/hatachi_tachi_video_research_report.md`
- **構成案**: A (シネマティック) / B (大人数群舞) / C (ドキュメンタリー) / ハイブリッド

## 🟢 アクティブ・優先度中

### 4. Salamat WBSサイト
- **状況**: ✅ **Phase 1 一気実装完了 (2026-05-19)** — Hero 全面リデザイン (ZoomParallax×MeshGradient 300vh) + 全カード Gallery4 統一 + Cobe 地球儀 + LocationTag (Cebu/Tokyo現地時刻) + Glowing Shadow 装飾 + List⇄Orbital ビューモード切替。tsc/build/dev HTTP200 + YD 目視確認 OK。Vercel デプロイは Phase 2 で実施。
- **公開URL (v2版、未更新)**: https://salamat-website-v2.vercel.app (まだ Phase 1 はローカルのみ)
- **次のアクション (Phase 2)**:
  - [ ] Phase 1 を Vercel に再デプロイ
  - [ ] #07 Magnetic+Fey ボタンを主要 CTA に適用
  - [ ] #01 Three.js パーティクル背景を Hero/Action 以外のセクションに
  - [ ] モバイル fallback の本格実装 (シェーダー→2D、ZoomParallax 簡略、地球儀静止画)
  - [ ] 旧コード cleanup (`country-hero`/`country-cards-band`/`dot-map` CSS、`PhilippinesMap`/`JapanMap`)
  - [ ] 写真ファイル名 cleanup (`ph-*` が日本、`jp-*` がフィリピンの実態と逆になっている)
  - [ ] circular ストーリーレイアウトの Gallery 化判断
  - [ ] 下層ページ実装 (About / Activities / Reports / News / Members / Support / Transparency)
  - [ ] お問い合わせフォーム + CMS連携 (microCMS/Notion/Sanity)
  - [ ] 独自ドメイン取得 → Vercel に紐付け
- **チーム**: YD (実装) + Riko (内容) + Haruka (構成) + Rena (デザイン)
- **スタック**: Next.js 16 + React 19 + Tailwind v4 + three + cobe + @paper-design/shaders-react + framer-motion + embla-carousel-react + Sora/Zen Maru/Noto Sans JP
- **パス**: `~/Downloads/07_開発・アプリ制作/salamat-website-v2`
- **Vercel scope**: yitao-dings-projects、プロジェクト名 `salamat-website-v2`
- **現行サイト**: `salamat-toyo.web.app` (Firebase、置き換え対象)
- **関連**: `knowledge/salamat/wbs_team.md`, [[2026-05-19_Salamat_WBS_Phase1実装]], [[vercel]], `~/Downloads/07_開発・アプリ制作/salamat-website-v2/design-brief.md`

### 5. Arte Grow (社会起業)
- **状況**: Type B モデル確定、9月フィリピン視察計画中
- **次のアクション**:
  - [ ] 9月視察の具体プラン作成
  - [ ] 視察前のリサーチ深化
  - [ ] メンバー (Rena, Haruka含む) の役割明確化
- **モデル**: Pride, Not Dependency
- **共同創業**: Taichi (19歳)
- **関連**: `knowledge/arte_grow/`

## 🟡 完成済み・運用フェーズ

### 6. Task Hub (タスク管理アプリ)
- **状況**: ✅ Vercelデプロイ完了、運用可能状態
- **パス**: `/Users/ittou/projects/salamat-task-hub`
- **スタック**: Next.js + Firebase + Tailwind CSS v4
- **次のアクション** (将来):
  - メンバー招待フロー仕上げ
  - PWA最終調整
  - 商用化検討 (大学サークル向けフリーミアム)
- **関連**: `knowledge/programming/tools/task_hub.md`

### 7. Lecture Hub (個人ナレッジハブ)
- **状況**: ⚠ Phase 2 + 家でやる残作業 (env/migration/Blob) 完了 (2026-05-19)、ただし **BlockNote 0.51 + React 19 + Next.js 15.5 の renderSpec エラーで本番デプロイ blocked**
- **URL**: `https://lecture-hub-yitao-ding-yitao-dings-projects.vercel.app/` (5/17 の MVP 版のまま。認証後の routes は migration 適用で 500 のはずだが実用前のため実害なし)
- **パス**: `/Users/ittou/projects/lecture-hub`
- **スタック**: Next.js 15.5 + React 19.2 + Supabase Postgres (Auth なし) + Drizzle + BlockNote 0.51 (ariakit) + AI SDK + pgvector + Dexie
- **2026-05-19 進捗**:
  - ✅ Phase A/2 を local 確定 (commit `c0f42bb` + SQL 修正 `2aca0d4`、未 push)
  - ✅ Supabase SQL Editor で 0002/0003 適用 (0002 は `name[] @> text[]` 型エラーで初回失敗 → `exists` 句で書き直して成功)
  - ✅ `.env.local` に Anthropic / Google / CRON_SECRET (生成) + 後に Vercel Blob の `BLOB_READ_WRITE_TOKEN` 自動投入
  - ✅ Vercel env (production/development) を整理: 旧 Supabase 系 3 つを削除、新 3 つを投入。Preview は CLI が対話必須で投入失敗 (ダッシュボード手作業ペンディング)
  - ✅ Vercel Blob 統合済み (`vercel env pull` で全 env 同期、`DATABASE_URL` が development に無かったので追加投入後に再 pull で復元)
  - ❌ `pnpm dev`: tasks / search / chat / admin の routes は動作、エディタ (`/p/[id]`) は renderSpec エラーで描画 NG (3度の修正 = audio rename / file系除外 / schema 外し すべて解消せず)
  - ❌ `vercel --prod`: BlockNote ブロッカーで未実行
- **次のアクション**:
  - [ ] **BlockNote 互換性問題の解決** — BlockNote downgrade (0.50系) / 別エディタ (TipTap・Lexical・Plate・Novel) 移行検討 / upstream fix 待ち
  - [ ] その後 `vercel --prod` で本番更新
  - [ ] Vercel Preview 環境への env 投入 (ダッシュボード手作業)
  - [ ] OPENAI_API_KEY: 今回スキップ (Whisper 用、後で Groq 対応を別タスクで)
- **意思決定記録**: [[2026-05-18_lecture_hub_個人用転換]]
- **関連**: `knowledge/programming/tools/lecture_hub.md`、[[claude_mistakes]] B-4 (BlockNote 互換性)

### 11. textbook-engine + 教科書システム (YD専用教科書)
- **状況**: ✅ 構築完了 (セッションA、2026-05-19 03:00) — 第1号PDF完成
- **パス**:
  - パイプライン: `~/projects/textbook-engine/` (Markdown→縦長A4 PDF、WeasyPrint + Mermaid + Pygments)
  - 教材リポジトリ: `~/ObsidianVault/textbook/` (5領域 + テンプレ + PDF出力)
- **使い方**: `cd ~/projects/textbook-engine && ./build.sh <md_path>` → `textbook/_output_pdf/` に出力
- **第1号**: `textbook/03_ai_engineering/01_claude_code_parallel.md` → A4 12ページ / 753KB
- **次のアクション** (運用):
  - [ ] 第2号以降のテーマ選定 (HTML/CSS基礎、Vercel、Git実践、Python基礎、Next.jsなど)
  - [ ] Google Drive 自動アップ (セッションBが朝ブリーフィング用に構築する基盤に相乗り)
  - [ ] フォント差し替えオプション (Noto Sans CJK / 明朝体プリセット) — 必要なら
  - [ ] textbook-engine を Private GitHub に push (現状ローカルのみ) — YD指示時
- **関連**: [[textbook_engine]]、[[2026-05-19_AI学習スプリント開始]]

### 12. morning-briefing (朝ブリーフィング自動配信、Max 20x 完結版)
- **状況**: ✅ 全パイプライン完成 (セッションB、2026-05-19 10:16) — `claude -p` + `say -v Kyoko` でフル動作確認済 (61.9秒、エラー0、PDF 247KB + MP3 3.68MB 生成)
- **パス**: `~/projects/morning-briefing/` (Python 3.11 + uv、ローカルのみ)
- **スタック**: feedparser + weasyprint + jinja2 + google-api-python-client (OAuth2) + crontab
  - **LLM**: `claude -p` ヘッドレス呼び出し → Max 20x 枠内、**API課金なし**
  - **TTS**: macOS 標準 `say -v Kyoko` + ffmpeg → **API課金なし**
  - **anthropic / openai パッケージは依存から削除済**
- **配信形式**: 縦長A4雑誌風PDF + 日本語TTS mp3 → Google Drive `Morning Briefing/2026-MM/` 自動アップ
- **配信時刻**: 毎朝 07:30 JST (`./install_cron.sh` で登録、未登録)
- **内容構成**: 今日のハイライト → 業界ニュース3本 (AI/撮影/開発) → ポッドキャストサマリ3本 → 推薦書 → 推薦コース → 締めの一言
- **次のアクション** (YD作業、APIキー不要に簡素化):
  - [ ] Google Cloud Console で OAuth クライアント発行 → `credentials/client_secret.json` 配置
  - [ ] `cd ~/projects/morning-briefing && uv run python -m src.uploader.drive --auth` でブラウザ認証 (初回のみ)
  - [ ] `./run.sh` で手動フルテスト (Drive アップロード確認)
  - [ ] `./install_cron.sh` で 07:30 JST cron 登録
- **設計判断**: LLM = `claude -p` (Max 20x 枠完結)、TTS = `say -v Kyoko` (macOS 標準、無料)、PDF = WeasyPrint、Drive = OAuth2 scope=drive.file (個人Drive 安全)、cron = crontab
- **コスト**: **完全無料** (Drive APIは無料枠、LLM/TTSは Max 20x 内)
- **計測値**: 全体 61.9 秒 (claude -p 整形 38秒、収集22秒、PDF/TTS 各1秒)
- **方針転換**: [[2026-05-19_API依存撤廃_Max20x完結化]] (朝のYD指摘で初版書き換え)
- **関連**: [[morning_briefing]]、[[2026-05-19_AI学習スプリント開始]]、[[2026-05-19_API依存撤廃_Max20x完結化]]、[[claude_code_permissions]]

### 13. ai-researcher (24時間 AI 研究員エージェント、セッションθ)
- **状況**: ✅ **Max 20x 完結化 完了 (セッションθ、2026-05-19 10:22)** — 朝の YD 指摘 ([[2026-05-19_API依存撤廃_Max20x完結化]]) を受け、Anthropic SDK + ANTHROPIC_API_KEY 依存を撤廃し `claude -p` (Claude Code ヘッドレス) に書き換え。実走 (collect --max-articles 2) で end-to-end 動作確認済、Vault `raw/research/2026-05-19/anthropic_blog/` に2記事書き出し、JSON schema による構造化出力 (importance/categories/related_projects 全フィールド埋まり) 確認。月課金 $0、Max 20x プラン枠で完結
- **パス**: `~/projects/ai-researcher/` (Python 3.11 + uv)
- **スタック**: **anthropic SDK 削除済** / arxiv + feedparser + bs4 + typer + tenacity + SQLite (重複・呼び出し履歴)
- **LLM 経路**: `claude -p --output-format json --json-schema ... --system-prompt ... --no-session-persistence --disable-slash-commands --permission-mode bypassPermissions --model claude-haiku-4-5` (stdin プロンプト)
- **ベンチ**: 1 記事 25-35 秒、21 件 / run で約 14 分 (pace_seconds=6 込み)
- **dry-run 実績 (2回目)**: raw 45 → dedup 43 → relevant 21 (threshold 3.0)
- **launchd**: 3 本 (`collect` 毎時 HH:03 / `weekly` 月06:00 / `archive` 月初04:00) — 書き換えで影響なし、`launchctl list | grep ai-researcher` で確認可
- **次のアクション** (任意):
  - [ ] (任意) `.env` の `GITHUB_TOKEN` 設定で GitHub API rate limit 60→5000/h
  - [ ] 翌朝 `~/ObsidianVault/raw/research/2026-05-20/` に記事が積まれてるか目視
  - [ ] `uv run ai-researcher status` で月間 headless 呼び数 + ソース別件数を確認
- **将来案**: papers_with_code 不安定 → scrape 化、github_trending を `weekly` window に、朝ブリーフィングとの `briefing-json` 接続をセッションβ側で有効化、embedding 類似検索 (Lecture Hub の pgvector 同居)
- **関連**: [[ai_researcher]] (knowledge/programming/tools/、Max 20x 完結版に全面更新)、[[morning_briefing]] (連携先、`raw/research/` 共有)、[[2026-05-19_API依存撤廃_Max20x完結化]] (本書き換えの意思決定)、`learning/research_interests.yaml` (興味プロファイル)

## 🟢 就活関連

### 8. 就活ES
- **状況**: 主要設問は完成済み
- **第一志望**: JICA (総合職)
- **その他**: DMM Global、DeNA、伊藤忠エネクス、大和証券
- **次のアクション**:
  - [ ] 未提出ESの最終確認
  - [ ] 面接対策
- **強み3本柱**:
  1. Apple販売 (iPhone日本1位)
  2. Salamat代表 (260名、フィリピン政府交渉)
  3. 税理士法人「ともに」(相続1年、独立担当)
- **関連**: `knowledge/career/`

## 🔴 一時停止・休眠

### 9. Yitao Film ポートフォリオサイト
- **状況**: 詳細プロンプト作成済み、本実装は後回し
- **理由**: 既存の `yitao-ding.github.io` で当面凌げる

### 10. 平成たち祭の応募フォーム企画
- **状況**: フォーム作成済み (Google Forms + GAS)
- **企画名**: 映像写真企画 (Yitao Film + Studio Metaliana)
- **期間**: 5/12〜6/30

## 📊 ステータス凡例

| 色 | 意味 |
|-----|------|
| 🟢 アクティブ | 今週・今月動く |
| 🟡 運用フェーズ | 完成、メンテナンスのみ |
| 🔴 休眠 | 一時停止、後で再開 |
| ⚫ 完了 | `archive/` に移動済み |

## 🔄 更新ルール

- 週1回は YDが確認する
- プロジェクトのステータス変化があったら即更新
- 完了したら `archive/YYYY-MM_<プロジェクト名>.md` に移動

## 📚 関連

- [[recent_decisions]]
- [[current_focus]]
- [[open_questions]]
- [[tools_available]]
