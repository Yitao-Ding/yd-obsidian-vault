---
type: current_state
last_updated: 2026-05-18 (vidkit autocut 完成 + FCPXML ラウンドトリップ採用で更新)
update_frequency: 週1回以上
---

# 進行中プロジェクト一覧

> このファイルはYDの「今アクティブに動いてるもの」のスナップショット
> 完了したものは `archive/` へ、休眠中は `archive/sleeping/` へ移動

## 🟢 アクティブ・優先度高

### 1. Obsidian Vault 構築 (今このタスク)
- **状況**: Claude Code に渡す設計書作成中
- **次のアクション**: Claude Codeで `~/ObsidianVault/` を構築
- **完了基準**: 新セッションで「YDの状況を要約して」と聞いて適切に応答できる
- **関連**: `~/Downloads/obsidian-vault-setup/`

### 2. vidkit (動画前処理CLI)
- **状況**:
  - dance モード完成 (TIME Instagram_最終２.mp4 で実機テスト済み)
  - **autocut モード完成 (2026-05-18)** — FCP用無音カット FCPXML 1.13 出力、lecture/vlog 2プリセット、Skill `~/.claude/skills/fcp-autocut/` 登録済み
  - lecture モードは未完成 (pyannote HF_TOKEN 待ち)
- **次のアクション** (優先度順):
  - [ ] **★ FCPXML ラウンドトリップ第一弾 = tighten オペレーション実装** (既存FCPプロジェクトの各クリップ内の残り無音をさらに詰める)
  - [ ] FCPXMLリーダーを汎用モジュールとして実装 (後続オペレーションでも再利用)
  - [ ] lecture モード仕上げ (pyannote HF_TOKEN セットアップ)
  - [ ] tutorial モード設計・実装 (Webサイト制作チュートリアル動画 → 実装)
  - [ ] Obsidian Vault 連携 (`--vault-path` オプション)
- **将来のFCPXMLオペレーション候補**: speaker-filter / marker-batch / beat-snap (蛹用途) / roles-bulk
- **パス**: `/Users/ittou/projects/vidkit`
- **関連**: `knowledge/programming/tools/vidkit.md`, `decisions/2026-05-18_FCPXML_ラウンドトリップ採用.md`

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
- **状況**: ✅ Vercel初回デプロイ完了 (2026-05-18)。トップページ2版 + Tweaks Panel 公開中
- **公開URL**: https://salamat-website-v2.vercel.app (チーム共有用、HTTP 200 確認済)
- **個別デプロイ確認URL**: https://salamat-website-v2-pd68j01x0-yitao-dings-projects.vercel.app (Vercel Deployment Protection で 401)
- **次のアクション**:
  - [ ] Claude Design で追加のカンプを詰める (YD)
  - [ ] 下層ページ (About / Activities / Reports / News / Members / Support / Transparency) の実装
  - [ ] お問い合わせフォーム、CMS連携、i18n
  - [ ] 本番モーション/カーソル演出の作り込み
  - [ ] 独自ドメイン取得 → Vercel に紐付け
- **チーム**: YD (実装) + Riko (内容) + Haruka (構成) + Rena (デザイン)
- **スタック**: Next.js 16 + React 19 + Tailwind v4 + Sora/Zen Maru/Noto Sans JP
- **パス**: `~/Downloads/07_開発・アプリ制作/salamat-website-v2`
- **Vercel scope**: yitao-dings-projects、プロジェクト名 `salamat-website-v2`
- **現行サイト**: `salamat-toyo.web.app` (Firebase、置き換え対象)
- **関連**: `knowledge/salamat/wbs_team.md`, [[vercel]] (Vercel CLI運用知見)

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
