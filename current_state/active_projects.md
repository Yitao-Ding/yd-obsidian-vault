---
type: current_state
last_updated: 2026-05-19 (AI学習スプリント開始 + 朝ブリーフィング + 教科書システム追加、就活終了)
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
- **状況**: ✅ 個人用転換 + Phase 2 全完了 (2026-05-18) — 本番デプロイ更新は家でやる
- **URL**: `https://lecture-hub-yitao-ding-yitao-dings-projects.vercel.app/` (2026-05-17 時点の MVP 版)
- **パス**: `/Users/ittou/projects/lecture-hub`
- **スタック**: Next.js + Supabase Postgres (Auth なし) + Drizzle + BlockNote + AI SDK + pgvector + Dexie
- **今日やったこと (2026-05-18)**:
  - 認証システム全削除 (Supabase Auth / Vault / RLS / api_tokens / ai_keys / `(auth)` / `/api/v1`)
  - 全クエリを PostgREST → Drizzle 一本化、AI キーは env 直読み
  - Phase 2 全項目: ダークモード / ノート内 AI Slash / 講義テンプレ Cron / 全文検索 / AI チャット履歴 / 数式・コード・PDF / 添付・Whisper / pgvector / オフライン編集 (Dexie+sync)
  - CLAUDE.md 整備 + vitest 26 件
- **家でやる残作業**:
  - [ ] Supabase SQL Editor で `0002_drop_multitenancy.sql` 適用
  - [ ] 同じく `0003_pgvector.sql` 適用
  - [ ] `.env.local` に `ANTHROPIC_API_KEY` / `GOOGLE_GENERATIVE_AI_API_KEY` / `OPENAI_API_KEY` / `BLOB_READ_WRITE_TOKEN` / `CRON_SECRET` を埋める
  - [ ] Vercel ダッシュボードで Blob 統合を有効化 → `vercel env pull`
  - [ ] `pnpm dev` で動作確認 (オフライン編集 / Slash メニュー / 同期インジケータ)
  - [ ] `vercel --prod` で本番デプロイ更新
- **意思決定記録**: [[2026-05-18_lecture_hub_個人用転換]]
- **関連**: `knowledge/programming/tools/lecture_hub.md`

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
