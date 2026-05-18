---
type: current_state
last_updated: 2026-05-18
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
- **状況**: dance モード完成、TIME Instagram_最終２.mp4 で実機テスト済み
- **次のアクション**:
  - [ ] lecture モード仕上げ (pyannote HF_TOKEN セットアップ)
  - [ ] tutorial モード設計・実装 (Webサイト制作チュートリアル動画 → 実装)
  - [ ] Obsidian Vault 連携 (`--vault-path` オプション)
- **パス**: `/Users/ittou/projects/vidkit`
- **関連**: `knowledge/programming/tools/vidkit.md`

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
- **状況**: 5月1日ローンチ予定だったが、現状要確認
- **次のアクション**: 公開状況を確認、未公開なら最終仕上げ
- **チーム**: YD (実装) + Riko (内容) + Haruka (構成) + Rena (デザイン)
- **構成**: 8ページ (Home / About / Activities / Reports / News / Members / Support / Transparency)
- **ツール**: Wix
- **関連**: `knowledge/salamat/wbs_team.md`

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
- **状況**: ✅ 本番デプロイ完了 (2026-05-17)
- **URL**: `https://lecture-hub-yitao-ding-yitao-dings-projects.vercel.app/`
- **パス**: `/Users/ittou/projects/lecture-hub`
- **スタック**: Next.js + Supabase + BlockNote + AI SDK
- **次のアクション**:
  - [ ] Anthropic APIキー登録
  - [ ] PAT 発行 → Claude Code Routine 設定
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
