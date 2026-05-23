---
type: current_state
last_updated: 2026-05-23 (ai-researcher: relevance緩和 + プロファイル拡充 + 死亡2ソース修復)
update_frequency: 週1回以上
---

# 進行中プロジェクト一覧

> このファイルはYDの「今アクティブに動いてるもの」のスナップショット
> 完了したものは `archive/` へ、休眠中は `archive/sleeping/` へ移動

## 🟢 アクティブ・優先度高

### ★ AI学習スプリント (2026-05-19 開始、最重要) ★

- **状況**: ✅ 学習基盤構築完了 (セッションC、2026-05-19 03:00) — `learning/` 配下 28ファイル
- **2026-05-19 朝の方針修正**: CCA-F は Anthropic Partner Network 加盟組織限定で個人受験不可と判明 → 代替として AWS Certified AI Practitioner (AIF-C01、個人受験可、$100) を採用。CCA-F は所属確定後に再判断。
- **目標**: 4資格取得 + 朝ブリーフィングで継続的なインプット
  1. Anthropic Academy 全18コース (2026-05-19 → 06-02、2週間スプリント)
  2. **AWS Certified AI Practitioner (AIF-C01)** (65問、5領域、$100、2026-07-15) ← CCA-Fから振替
  3. Google AI Professional Certificate (Coursera、$49/月、Google AI Pro 3ヶ月付き、2026-06-16 → 07-31)
  4. Google Cloud Generative AI Leader ($99、90分、2026-08、余裕あれば)
  ※ Claude Certified Architect Foundations は `pending_partner_access` 状態で保留 ([[../learning/ai_certifications/claude_certified_architect/README]])
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

### 2. vidkit (動画前処理CLI)
- **状況**:
  - dance モード完成 (TIME Instagram_最終２.mp4 で実機テスト済み)
  - **autocut モード完成 (2026-05-18)** — FCP用無音カット FCPXML 1.13 出力、lecture/vlog 2プリセット、Skill `~/.claude/skills/fcp-autocut/` 登録済み
  - **tighten モード完成 (2026-05-19)** — 既存FCPプロジェクトの各クリップ内の残り無音を再カット、合成テストFCPXMLで検証済 (1clip→3clips、3.6s削除、xmllint通過)
  - **tutorial モード完成 (2026-05-19)** — URL/ローカル自動判別、dance パイプライン相乗りで Claude Code が自走実装する PROMPT.md を生成
  - **--vault-path オプション完成 (2026-05-19)** — `<vault>/raw/vidkit/<mode>/` への出力に全モード対応
  - **★ tutorial モード P1-P5 改善完成 + push 済 (2026-05-23 04:00)** — Opus 4.7 サブエージェントで4 commit (`bd1a24b` --lang auto バグ修正 [HTTP 429 回避] / `3f429a0` --source whisper オプション追加 [MLX-Whisper 再文字起こし] / `d5425e9` --prompt-type で 3 種分岐 [implementation/setup/explainer] / `db36ff9` 既存出力検出 [--force] + chapters.md 自動生成)。42 tests pass、実機テスト ("Me at the zoo" 19s) で `--source whisper --whisper-model tiny` 完走。実運用初回 ([[2026-05-23_MCP_9個導入]]) のフィードバックを反映
  - lecture モードは未完成 (pyannote HF_TOKEN 待ち)
- **次のアクション** (優先度順):
  - [ ] **★ lecture モード仕上げ** (pyannote HF_TOKEN セットアップ → YD作業) — HuggingFace で `pyannote/speaker-diarization-community-1` のリクエスト承認 → `.env` に `HF_TOKEN` 設定
  - [ ] 実 FCP プロジェクト (例: 平成たち祭・蛹) を Export XML → tighten 実機検証
  - [x] **tighten/tutorial の Skill 化完了** (2026-05-19 00:52) — `~/.claude/skills/fcp-tighten/SKILL.md` (4.7KB) / `~/.claude/skills/video-tutorial/SKILL.md` (5.2KB)
  - [x] **vidkit を git 初期化 + GitHub Private に push 完了** (2026-05-19 ~01:00) — `177a2f2 Initial commit` → `29deb68 docs: lecture セットアップ + tighten 実機検証手順を追加` (~01:05) → **`40cef7d fix: variable-fps 動画 (Zoom録画/screen capture) を autocut/tighten で扱えるように` (~02:00)**。origin = https://github.com/Yitao-Ding/vidkit.git、現在クリーン
- **将来のFCPXMLオペレーション候補**: speaker-filter / marker-batch / beat-snap (蛹用途) / roles-bulk — `parse_fcpxml` + `write_fcpxml_from_parsed` の汎用モジュールが揃ったので追加コストは低い
- **パス**: `/Users/ittou/projects/vidkit`
- **関連**: `knowledge/programming/projects/vidkit.md`, `decisions/2026-05-18_FCPXML_ラウンドトリップ採用.md`, `decisions/2026-05-19_vidkit_tighten_tutorial_完成.md`

### 3. 平成たち祭 動画制作
- **状況**: ✅ 編集設計書完成 (2026-05-20 深夜) → **朝 DaVinci 着手可能**
- **採用構成**: 案A (シネマティック) + 案C (ドキュメンタリー) ハイブリッド (大川優介スタイル)
- **本編**: 2:00 / YouTube横 + Instagram縦 / 7パート構成 (COLD OPEN〜FINALE)
- **SNS**: ショート5本 (BTS型 / ノスタルジー型 / ダンス型 / インタビュー型 / 来場者型)
- **次のアクション**:
  - [ ] **★ 外付けSSD を接続して撮影素材を確認** (5/6 R5/FX30/GoPro クリップ)
  - [ ] DaVinci プロジェクト初期化 (`knowledge/filmmaking/hatachi_tachi_davinci_workflow.md` の Step 1 から)
  - [ ] BGM 選曲 (Artlist / Epidemic Sound — 本番楽曲は SNS 著作権リスクあり)
  - [ ] LUT ダウンロード (`~/Movies/LUTs/` に Leefilm Japan 等)
  - [ ] インタビュー音声品質確認 (外部マイク使用有無 → Short #4 の判断)
- **設計書**: `knowledge/filmmaking/hatachi_tachi_storyboard.md` / `_assets_list.md` / `_sns_shorts.md` / `_davinci_workflow.md`
- **意思決定**: `decisions/2026-05-20_平成たち祭_編集設計確定.md`
- **完成目標**: 本編 6月末 / SNS ショート 本編完成後2週間以内
- **音源 (受領済)**: `~/Downloads/99_未分類_要確認/gigafile受領物/gigafile-0813-5ba1b0454e7c5cb18caadf8fb375a748/` (Hi,Me / Jyo-Ro / emmmanumber / cocona+伶香 / 友達たち)
- **メモ**: Hi,Me:) 「蛹」DaVinci 復旧の件は 2026-05-19 にアーカイブ (memory 削除済)

## 🟢 アクティブ・優先度中

### 4. Salamat WBSサイト
- **状況**: ✅ **Phase 1 + Phase 2 演出強化 完了 + Vercel 本番デプロイ済 (2026-05-19 夜)** — Phase 1 (Hero ZoomParallax×MeshGradient / Gallery4 / Cobe / LocationTag / Glowing Shadow / List⇄Orbital) + Phase 2 (Magnetic+Fey ボタン + Three.js パーティクル背景 + 旧コード/写真ファイル名 cleanup) を 2 commit (`46e6839` / `20ae3ee`) + 2 回 Vercel 本番デプロイで反映。YD ブラウザ目視 OK。
- **公開URL**: https://salamat-website-v2.vercel.app (HTTP 200、Phase 1+2 反映済)
- **Phase 2 で完了したもの**:
  - [x] Phase 1 を Vercel に再デプロイ (今日 14:00 頃 dpl_G4DhSUZ6uHL41h3753vzAwdk76nB)
  - [x] #07 Magnetic+Fey ボタンを主要 CTA に適用 (Hero × 2 + Vision Value、CtaButton 経由 + Tweaks で切替可、DEFAULT を magnetic-fey に)
  - [x] #01 Three.js パーティクル背景 — Vision/Report/Story/News に per-section マウント (density 900-1400、NormalBlending、寒色5色、size 0.07-0.09、opacity 0.5-0.65、prefers-reduced-motion 尊重、mobile 1/3 密度)
  - [x] 旧コード cleanup — country-hero / country-cards-band / country-scroller / country-card 関連 CSS 削除、.dot-map 関連も全削除
  - [x] 写真ファイル名 cleanup — `ph-feeding/lumbani/selma.jpg`(中身は日本) → `jp-1/2/3.jpg`、`jp-abk/kurodaira/terakoya.jpg`(中身はフィリピン) → `ph-1/2/3.jpg`、コード側の逆転マッピングを素直な形に戻し
  - [x] Phase 2 Vercel 再デプロイ (今日 22:00 頃 dpl_3cZb35DTmMDYeZyLDA4zXdzwDJCh)
- **次のアクション (Phase 3 持ち越し)**:
  - [ ] **GitHub Private repo 作成 + push** (今日 remote 未設定で push スキップ、別タスクで `gh repo create` → `git remote add origin` → `git push -u`)
  - [ ] モバイル fallback の本格実装 (シェーダー→2D、ZoomParallax 簡略、地球儀静止画、ParticleBg 密度更に削減)
  - [ ] circular ストーリーレイアウトの Gallery 化判断
  - [ ] 下層ページ実装 (About / Activities / Reports / News / Members / Support / Transparency)
  - [ ] お問い合わせフォーム + CMS連携 (microCMS/Notion/Sanity)
  - [ ] 独自ドメイン取得 → Vercel に紐付け
  - [ ] (要すれば) パーティクル密度/opacity の微調整、blending mode の AdditiveBlending 検討
- **チーム**: YD (実装) + Riko (内容) + Haruka (構成) + Rena (デザイン)
- **スタック**: Next.js 16 + React 19 + Tailwind v4 + three + cobe + @paper-design/shaders-react + framer-motion + embla-carousel-react + Sora/Zen Maru/Noto Sans JP
- **パス**: `~/Downloads/07_開発・アプリ制作/salamat-website-v2`
- **Vercel scope**: yitao-dings-projects、プロジェクト名 `salamat-website-v2`
- **現行サイト**: `salamat-toyo.web.app` (Firebase、置き換え対象)
- **関連**: `knowledge/salamat/wbs_team.md`, [[2026-05-19_Salamat_WBS_Phase1実装]], [[2026-05-19_Salamat_WBS_Phase2_演出強化]], [[vercel]], `~/Downloads/07_開発・アプリ制作/salamat-website-v2/design-brief.md`

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

### 6. Task Hub (タスク管理アプリ) — UI/UX 根本見直しフェーズ突入 (2026-05-21)
- **状況**: 🟢 **UI/UX 根本見直しに着手 (2026-05-21 21:01)** — フロントエンドレベルから仕様を見直す方針 (YD 指示)。現状 UI 把握完了 (8 画面 + globals.css + spec + HANDOVER)、5 大課題特定 (ブランド分裂 #ff6a00 vs #2563EB / デザインシステム不在 / IA 浅い / SaaS テンプレ的 / ロゴ単文字)。**Anthropic Labs ハーネス設計を踏襲した 4 自立型エージェント** を `~/projects/salamat-task-hub/.claude/agents/` に構築完了 (planner / builder / qa-evaluator / design-evaluator★)。次は YD のビジョン (1〜4 行) → planner 起動。
- **過去履歴**: 2026-05-19 21:17 git 整理 + GitHub 連携完了 (untracked 32 + dirty 7 → 5 commit)。**デプロイ実態は Firebase Hosting** (`salamat-task-hub.web.app`) — Vercel ではない
- **パス**: `/Users/ittou/projects/salamat-task-hub`
- **GitHub**: https://github.com/Yitao-Ding/salamat-task-hub (Private、main → origin/main 追跡済)
- **本番 URL**: https://salamat-task-hub.web.app (Firebase Hosting、Spark プラン)
- **スタック**: Next.js 16.2.1 (App Router、静的 export) + React 19.2.4 + TypeScript + Tailwind CSS v4 + Firebase (Auth/Firestore/Storage) + next-pwa
- **コミット履歴 (2026-05-19 push)**:
  - `f876634` chore: prepare Next.js + Firebase build configuration
  - `6d0e341` feat: add Firebase backend configuration
  - `7fa1b65` feat: implement core app (auth, Firestore CRUD, routes, UI)
  - `47b6be5` feat: PWA setup (manifest, service worker, icons)
  - `9dfcd77` docs: add handover document, spec, and admin setup script
- **次のアクション** (UI/UX 根本見直し、優先度順):
  - [ ] **★ YD から 1〜4 行のビジョン取得** → `taskhub-planner` 起動 (SPEC.md + DESIGN_DIRECTION.md + sprint-XX-*.md 生成)
  - [ ] Sprint 01 から: builder 実装 → qa + design 並列評価 → 不合格なら修正イテレーション → 合格で次スプリント
  - [ ] **必須対応**: `globals.css` の orange グラデ (`linear-gradient(#ff6a00 → white)`) を DESIGN_DIRECTION.md のカラーに置換
  - [ ] 既存課題 (メンバー招待フロー / PWA 最終調整 / Firestore rules 本番デプロイ確認 / next-pwa v6 移行) は再設計の中で吸収または別スプリントへ
  - [ ] 商用化検討 (大学サークル向けフリーミアム) は再設計完了後
- **4 エージェント構成**: taskhub-planner / taskhub-builder / taskhub-qa-evaluator / taskhub-design-evaluator (Anthropic Labs ハーネス設計準拠、Design Evaluator は frontend-design / web-design-guidelines / ui-ux-pro-max / vercel:react-best-practices の 4 スキル参照必須 + AIスロップ即不合格ルール内蔵)
- **関連**: [[task_hub]] (運用マニュアル、2026-05-20 作成)、[[2026-05-19_TaskHub_git整理_GitHub連携]] (git 整理の意思決定)、[[2026-05-21_TaskHub_UIUX根本見直し_4エージェントハーネス構築]] (本フェーズの意思決定)

### 7. Lecture Hub (個人ナレッジハブ)
- **状況**: ✅ **本番デプロイ完了 (2026-05-19 21:50)** — BlockNote × Next.js 15.5 の根本不整合 (6試行で確証) を確定し **TipTap v3 に全面移行**、本番動作確認 OK
- **公開URL**: `https://lecture-hub-sable.vercel.app/` (新エイリアス、200 公開アクセス可)
- **個別URL**: `https://lecture-g9pfx9y3z-yitao-dings-projects.vercel.app` (401 Deployment Protection)
- **パス**: `/Users/ittou/projects/lecture-hub`
- **スタック**: Next.js 15.5 + **React 18.3.1** + Supabase Postgres (Auth なし) + Drizzle + **TipTap v3.23** (StarterKit + code-block-lowlight + mathematics + 自作 PdfNode/AudioNode) + AI SDK + pgvector + Dexie
- **2026-05-19 (TipTap 移行) の流れ**:
  - 1. BlockNote 0.51.1 パッチ更新 → ❌
  - 2. BlockNote 0.50 downgrade → ❌
  - 3. dynamic import (ssr:false) → ❌
  - 4. React 19.2 → 18.3.1 ダウングレード → ❌
  - 5. `.next` cache clear → ❌
  - 6. Editor 2行ミニマム化 (`useCreateBlockNote()` + `<BlockNoteView/>` のみ) → ❌
  - 7. **TipTap v3 に移行**: StarterKit / code-block-lowlight / mathematics / 自作 PdfNode/AudioNode / EditorToolbar (useEditorState で active 同期) → ✅
  - 8. `tsc --noEmit` 通過、`next build` 4.6秒通過、`vercel --prod` 成功、実機 OK
- **TipTap 版で実装済み機能**:
  - 基本フォーマット (B/I/S/code/H1-3/list/quote/codeBlock) — Toolbar から
  - コードハイライト (lowlight = highlight.js github-dark)
  - 数式 (KaTeX、`@tiptap/extension-mathematics`)
  - PDF 埋め込み (自作 Node + iframe + Vercel Blob アップロード)
  - 音声 + Whisper 文字起こし (自作 Node + `/api/transcribe`)
  - AI: 要約挿入 (`/api/ai/summarize` → blockquote 挿入)
  - AI: タスク抽出 (`/api/ai/extract-tasks` → `createTasksBulk` で DB insert)
- **Phase 3 残作業 (別日)**:
  - [x] BlockNote 関連 `*.bak` 削除 + `@blocknote/*` パッケージ `pnpm remove` (2026-05-20、commit 591368b/9e45956)
  - [x] `plainTextFromDocument` TipTap 形式対応 + vitest 5件 (2026-05-20、commit 234f8ad、`src/lib/editor/text.ts`)
  - [x] `src/lib/offline/sync.ts` TipTap 形式対応確認 → `document: unknown` で形式非依存、書き換え不要
  - [x] `src/lib/blocknote/` → `src/lib/editor/` リネーム (2026-05-20、commit a158830)
  - [ ] **Slash Menu (`/`) の TipTap 版実装** (Notion 風体験の完成)
  - [ ] Shiki ハイライトへの乗せ替え (NodeView 差し替えで対応可)
  - [ ] 本番で 音声 / AI 要約 / タスク抽出 / 数式 の動作確認 (まだ未確認)
  - [ ] 既存 indexed ドキュメント の再生成 (`/admin/reindex`)
  - [ ] Vercel Preview 環境への env 投入 (ダッシュボード手作業、ペンディングのまま)
- **意思決定記録**: [[2026-05-19_tiptap_migration]]、[[2026-05-18_lecture_hub_個人用転換]]、[[2026-05-20_lecture_hub_notion_design_phase1]]
- **関連**: `knowledge/programming/projects/lecture_hub.md`、[[claude_mistakes]] B-4 (BlockNote × Next 15.5 不整合、本日 6 試行で確証)

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
- **状況**: ✅ **capabilities セクション統合 + GitHub Private + cron 登録完了 (2026-05-20 03:20)** — Vault context (`active_projects.md` + `available_capabilities.md`) を `claude -p` プロンプトに同梱し、当日タスクとスキル/MCP のマッピング候補を 3〜5 件提示する 06 セクションを追加。dry-run で synthesize 完了 (5件 capability 生成: claude-api / vercel:ai-sdk / Google Drive / fcp-autocut / Notion)。初回 (2026-05-19 10:16) は `claude -p` + `say -v Kyoko` でフル動作確認済 (61.9秒、エラー0、PDF 247KB + MP3 3.68MB 生成)
- **GitHub**: `https://github.com/Yitao-Ding/morning-briefing` (Private、2026-05-20 push 済)
- **cron**: `30 7 * * * run.sh` 登録済 (2026-05-20 03:18)、次回実行 = 翌朝 07:30 JST。初回は Drive 認証未完了で upload のみ失敗予定、ローカル PDF/MP3 は生成される。詳細: [[handover_morning]]
- **パス**: `~/projects/morning-briefing/` (Python 3.11 + uv)
- **スタック**: feedparser + weasyprint + jinja2 + google-api-python-client (OAuth2) + crontab
  - **LLM**: `claude -p` ヘッドレス呼び出し → Max 20x 枠内、**API課金なし**
  - **TTS**: macOS 標準 `say -v Kyoko` + ffmpeg → **API課金なし**
  - **anthropic / openai パッケージは依存から削除済**
- **配信形式**: 縦長A4雑誌風PDF + 日本語TTS mp3 → Google Drive `Morning Briefing/2026-MM/` 自動アップ
- **配信時刻**: 毎朝 07:30 JST (`./install_cron.sh` で登録、2026-05-20 03:18 登録済)
- **内容構成**: 今日のハイライト → 業界ニュース3本 (AI/撮影/開発) → ポッドキャストサマリ3本 → 推薦書 → 推薦コース → 締めの一言
- **次のアクション** (YD作業、Drive 認証のみ残):
  - [ ] Google Cloud Console で OAuth クライアント発行 → `credentials/client_secret.json` 配置
  - [ ] `cd ~/projects/morning-briefing && uv run python -m src.uploader.drive --auth` でブラウザ認証 (初回のみ)
  - [ ] `./run.sh` で手動フルテスト (Drive アップロード確認)
  - [x] `./install_cron.sh` で 07:30 JST cron 登録 (2026-05-20 03:18 完了)
  - 詳細: [[handover_morning]]
- **設計判断**: LLM = `claude -p` (Max 20x 枠完結)、TTS = `say -v Kyoko` (macOS 標準、無料)、PDF = WeasyPrint、Drive = OAuth2 scope=drive.file (個人Drive 安全)、cron = crontab
- **コスト**: **完全無料** (Drive APIは無料枠、LLM/TTSは Max 20x 内)
- **計測値**: 全体 61.9 秒 (claude -p 整形 38秒、収集22秒、PDF/TTS 各1秒)
- **方針転換**: [[2026-05-19_API依存撤廃_Max20x完結化]] (朝のYD指摘で初版書き換え)
- **関連**: [[morning_briefing]]、[[2026-05-19_AI学習スプリント開始]]、[[2026-05-19_API依存撤廃_Max20x完結化]]、[[claude_code_permissions]]

### 14. ai-simulator (複数AIペルソナ並列シミュレーター、セッションη)
- **状況**: ✅ **Max 20x 完結化完了 (2026-05-19 夕、D-5 ミス修正)** — anthropic SDK / ANTHROPIC_API_KEY 撤廃、`claude -p` async subprocess + Semaphore (max_concurrency=5) で並列実行制御。**ユニットテスト 19件 全通過**、`ai-simulator list` 疎通OK、**実支払い $0**
- **パス**: `~/projects/ai-simulator/`
- **スタック**: rich / typer / pyyaml / pydantic (uv 管理)、LLM は `claude -p` ヘッドレス経由 (Max 20x枠)
- **モデル**: Sonnet 4.6 (品質優先、`--budget` で Haiku 4.5 切替可)
- **シナリオ**:
  - `salamat_team_chaos` (extreme, 10人質問攻め — 視察4ヶ月前の臨時ミーティング)
  - `apple_sales_rush` (hard, 混雑時5人同時接客 — 怒り客/即決客/シニア/迷い客)
  - `crisis_management` (extreme, 視察3日前トラブル7人)
  - `client_pitch` (hard, Arte Grow 現地パートナー4人商談)
- **ペルソナ**: salamat_member (10 variants) / apple_customer (8) / arte_grow_partner (5) / filmmaker_client (5) / job_interviewer (4)
- **コスト**: **実支払い $0** (Max 20x 枠完結)。`cost_cap_usd` は「枠の使用感」の目安として残してあり、Sonnet 換算で 1 セッション $0.3〜0.7 相当
- **振り返り**: 終了時 Claude が評価ルーブリックに沿って Markdown 生成 → `~/ObsidianVault/learning/simulations/<session_id>.md` に自動保存
- **次のアクション (YD作業)**:
  - [ ] `cd ~/projects/ai-simulator && uv run ai-simulator run client_pitch --budget` で疎通確認 (Haiku、Max 20x枠内)
  - [ ] 本命: `uv run ai-simulator run salamat_team_chaos` (Sonnet、Max 20x枠内)
  - [ ] `logs/<session_id>.md` と Vault `learning/simulations/<session_id>.md` を確認
- **関連**: [[ai_simulator]] (運用マニュアル、必須3セクション付き)、[[2026-05-19_API依存撤廃_Max20x完結化]]、[[claude_mistakes]] D-5

### 13. ai-researcher (24時間 AI 研究員エージェント、セッションθ)
- **状況**: ✅ **relevance 緩和 + プロファイル拡充 + 死亡2ソース修復 完了 (2026-05-23 早朝)** — 「集めてるのに Vault に残らない」(kept ほぼ0) を解消。threshold 3.0→1.5 + collect `--max-articles 8` (plist + launchd reload)、interests.yaml に撮影/Web/国際協力語 (high16 + medium24) 追加、papers_with_code→HF daily_papers 差し替え (name=hf_papers)、github_trending を topic検索→キーワード+pushed+stars に修正。**dry-run で relevant 0→46/run、7ソース全稼働**を確認。詳細: [[2026-05-23_ai-researcher_relevance緩和とソース修復]]
- **過去の状況**: ✅ **Max 20x 完結化 完了 (セッションθ、2026-05-19 10:22) → slug パス区切りバグ修正 (2026-05-19 21:10)** — 朝の YD 指摘 ([[2026-05-19_API依存撤廃_Max20x完結化]]) を受け、Anthropic SDK + ANTHROPIC_API_KEY 依存を撤廃し `claude -p` (Claude Code ヘッドレス) に書き換え。月課金 $0、Max 20x プラン枠で完結。**夜の slug バグ**: google_research の RSS guid が URL 形式 → `Article.slug()` が `source_id` を素通し → pathlib で `/` が path 区切り扱いになり 10:03/11:03 の collect が kept=0 で空回り → `src/utils/models.py:29-33` を 3 行修正 (`sid = slugify(self.source_id, max_length=24)`) で全 source の事故を予防 → `collect` 再走で過去失敗分 5 件全復旧 ([[claude_mistakes]] A-10 + [[ai_researcher]] 必須3セクション更新済)
- **パス**: `~/projects/ai-researcher/` (Python 3.11 + uv)
- **スタック**: **anthropic SDK 削除済** / arxiv + feedparser + bs4 + typer + tenacity + SQLite (重複・呼び出し履歴)
- **LLM 経路**: `claude -p --output-format json --json-schema ... --system-prompt ... --no-session-persistence --disable-slash-commands --permission-mode bypassPermissions --model claude-haiku-4-5` (stdin プロンプト)
- **ベンチ**: 1 記事 25-35 秒、21 件 / run で約 14 分 (pace_seconds=6 込み)
- **dry-run 実績 (2回目)**: raw 45 → dedup 43 → relevant 21 (threshold 3.0)
- **launchd**: 3 本 (`collect` 毎時 HH:03 / `weekly` 月06:00 / `archive` 月初04:00) — 書き換えで影響なし、`launchctl list | grep ai-researcher` で確認可
- **次のアクション** (残課題、別タスク):
  - [ ] 新ジャンルのソース追加 (撮影系/Web開発系/国際協力系 RSS) — これが無いと 2026-05-23 で足した撮影/フィリピン/法律語が「ソースに記事が流れてこない」ので発火しない。**ソース選定は YD と方向性を握ってから**
  - [ ] github_trending を `stars:50..5000` の上限でトレンド性向上 (現状は transformers 16万star 等の定番大型が上位独占、緊急性は低)
  - [ ] (任意) `.env` の `GITHUB_TOKEN` 設定で rate limit 10/分→5000/時 (PAT 発行は YD 作業)
  - [ ] 数日後 `uv run ai-researcher status` で kept 件数の増加 + ソース別バランスを確認
- **将来案**: 朝ブリーフィングとの `briefing-json` 接続をセッションβ側で有効化、embedding 類似検索 (Lecture Hub の pgvector 同居)。(papers_with_code/github_trending の死亡は 2026-05-23 に修復済み)
- **関連**: [[ai_researcher]] (knowledge/programming/projects/)、[[morning_briefing]] (連携先、`raw/research/` 共有)、[[2026-05-19_API依存撤廃_Max20x完結化]]、[[2026-05-23_ai-researcher_relevance緩和とソース修復]] (relevance緩和+ソース修復の意思決定)、`learning/research_interests.yaml` (興味プロファイル)

### 15. parallel-claude (並列 Claude Code 監視基盤)
- **状況**: ✅ **初回運用完了 (2026-05-20 03:15→04:08、約53分)** — 5並列 parallel-claude + 12並列 business-plan-sprint を `--dangerously-skip-permissions` + OAuth (Max 20x 完結) で並走、`CronCreate(2-59/5 * * * *)` で 5分ごと監視、12 iter で全終息。**実支払い $0**。
- **パス**: `~/projects/parallel-claude/`
- **スタック**: `claude --print --output-format stream-json --verbose` (ヘッドレス) + nohup + `~/projects/parallel-claude/scripts/monitor.sh` + CronCreate
- **構成**: `state/sessions.json`, `tasks/`, `logs/`, `scripts/{launch_session,launch_all,monitor,status_report}.sh`
- **5並列 parallel-claude 成果**:
  - ✅ Vault整合性チェック → `current_state/vault_improvement_proposals.md` 23.8KB
  - ⚠️ textbook 第2号 (Vercel/Next.js 入門) Markdown 18.6KB だが **PDF未出力** (build.sh 失敗要因要確認)
  - ✅ Salamat WBS 下層ページ → `src/app/{about,activities,reports}/page.tsx` (4.2-6.5KB)
  - ✅ Arte Grow フィリピン視察リサーチ → `raw/research/2026-05-20_arte_grow_philippines_visit.md` 24.5KB
  - ✅ AI学習 Day2 自習ノート → `learning/.../{05_claude_code_101, 04_introduction_to_claude_cowork}.md` 計346行
- **次のアクション**:
  - [ ] textbook 第2号 PDF 出力 (build.sh エラー原因確認 → 再実行)
  - [ ] monitor.sh A-12 修正 (ps 検出プロセスも log_size > 0 で completed 判定)
  - [ ] tasks/ にテンプレを足して、定常運用しやすい状態に
- **関連**: [[parallel_claude]] (運用マニュアル、必須3セクション付き)、[[2026-05-20_parallel-claude_監視基盤構築]] (意思決定)、[[claude_mistakes]] A-11 (UTF-8 decode) / A-12 (状態判定片肺)

### 16. business-plan-sprint-2026-05-19 (12並列ビジネスプラン発散→統合)
- **状況**: ✅ **初回ラン完了 (2026-05-20 03:13→04:06、約53分)** — 15 prompts (発散×7 / 統合×3 / 雑務×2 + 後追い3) を `claude -p` で並列実行、Synthesis が `FINAL_REPORT.md` 生成。**実支払い $0** (Max 20x 完結)。
- **パス**: `~/projects/business-plan-sprint-2026-05-19/`
- **成果物**:
  - `FINAL_REPORT.md` 29KB (3案ピッチ完成)
    - 🥇 **LegalTrio** (日中英 法律文書翻訳 AI SaaS、統合スコア 60/70、粗利85%/LTV240万/年商2.9億)
    - 🥈 日本職人 × 中国越境EC ブランド (D2C、年商3-5億)
    - 🥉 相続専門税理士向けバーティカルSaaS (税理士法人「ともに」インターン経験が参入障壁)
  - `candidates/` 50本
  - `critiques/` 61本 + `_meta_review.md` 11KB
    - 「規制空白=チャンス」誤読が 15-20 候補に共通 (最大の認知バイアス)
    - 最も死亡確率低い候補: `06_06_philippines_japanese_education_saas` (62-64点推定)
  - `sessions/` 各セッション中間素材
- **死亡セッション**:
  - 旧 02_divergent_b (90310): API socket error (リトライ可能)
  - 旧 10_synthesis (92908): ScheduleWakeup を `--print` モードで誤用、即死亡 (設計矛盾)
  - 新 14_hatachi_video (95895): YD への確認質問待ちで死亡 (`~/Downloads/hatachi_tachi_video_research_report.md` 不在 / 本編尺 2分 vs 3-5分)
  - 旧 8本 + 新 5本 = 計15本死亡 (大半は完了後の自然終了、`died` ステータスは monitor.sh A-12 のため誤分類)
- **二重起動**: 03:13:44 起動の旧12本に対し、何者かが 03:23:24 に再起動した形跡あり (`_pids.txt` 上書き)。両系統とも完走。
- **次のアクション**:
  - [ ] YD が `FINAL_REPORT.md` を読み、3案のどれを採るか判断 (LegalTrio が最有力)
  - [ ] 14_hatachi_video の質問 (本編尺 / リサーチレポートのありか) に YD が回答 → 再起動
  - [ ] Red Team の `_meta_review.md` 「規制空白=チャンス」誤読を踏まえて候補再評価
  - [ ] `06_06_philippines_japanese_education_saas` 候補 (Red Team が最有力と判定) を読む
- **関連**: [[parallel_claude]] (parallel-claude 側に外部 `_pids.txt` 連携の実装)、[[2026-05-20_parallel-claude_監視基盤構築]]

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
