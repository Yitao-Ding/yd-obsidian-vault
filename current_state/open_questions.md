---
type: current_state
last_updated: 2026-05-18
---

# 未解決の疑問・検討事項

> 議論が中途半端で終わっているもの、後で確認すべきもの
> 解決したら `decisions/` に移動

## 🤔 検討中

### ★Q0. 【矛盾発見 2026-08-13】PAA の7〜8月コミットは GitHub 未push = 旧Macロスト対象だった
- **事実**: `Yitao-Ding/project-agent-application` の GitHub 上の main HEAD = `decd10e` (2026-06-04 実モード全配線時点)。リモートブランチも main のみ (Neo 実機で `git branch -r` / `git log --all` 確認済)
- **矛盾**: 移行棚卸し ([[2026-08-11_MacBookPro故障_MacBookNeo移行_Vault復旧]]) の「未pushは無い (YD確認済み)」と食い違う。実際は **2026-07-13 全面改修 (branch overhaul/2026-07、99ファイル) / 2026-08-08 セキュリティ再監査 (`22d684a`、migration 023含む) / 2026-08-09〜11 白基調リデザイン実装 (16コミット)** が旧Macローカルのみ → ロスト対象
- **生きているもの**: TestFlight build 3 (白基調全部入り、提出済) / 実DBに適用済みのmigration・deploy済みEdge関数 / デザインの正 = claude.ai/design プロジェクト / Vault内の設計・レビュー正本の記述
- **復元ルート調査結果 (2026-08-15 実機検証済)**:
  - ❌ **EASソースtarball**: GraphQL Build型に project archive 系フィールド無し (全フィールド実取得で確認)・expo/fyi ドキュメントにもDL手段の記載無し → **EASからソース回収は不可** (Expoサポートに build 74499b8c のアーカイブ提供を依頼する長打ルートのみ残る)
  - ✅ **EAS build 4 (1.0.0(4), commit `d0eaa19`, 2026-08-11)**: IPA (白基調全部入りのコンパイル済アプリ) がDL可。**残り約25日で失効** → 保険DL推奨。build一覧: expo.dev/accounts/yitao0907/projects/project-agent-application/builds
  - ★★ **claude.ai/code に旧Macの全セッション記録がクラウド同期残存 (実物確認済)**: 8/9〜11の白基調実装セッション「2026年8月のハンドオーバー情報を確認」を開けた (最終コミット `7adaa71` まで記録)。サイドバーに旧Macの他セッション多数 (平成たち祭プロンプト実行と実装 等)。**транскリプト内の編集差分から main `decd10e` ベースに再生 (replay) 復元が可能な見込み** — 7/13全面改修・8/8セキュ監査・8/9-11白基調の各セッションを順に。閲覧にはデバイス確認の再サインインが必要 (elevated auth)
  - ✅ **Supabase本番**: 適用済みmigration (023含む) はDBに、deploy済みEdge関数はサーバに現存 → `supabase db dump` / functions download で回収可
- **次**: ①旧Macディスク救出の可否をYDに確認 (可なら全部一発解決・replay不要) ②保険でIPA DL ③救出不可ならclaude.aiセッションreplay復元をNeoで実行
- **背景**: YouTube のWebサイト制作チュートリアル動画を、Markdown手順書 + Claude Code 実装まで一気通貫させたい
- **未決**: 動画のタイプ別の処理方法、出力先 (既存プロジェクトに組み込むか新規か)、応用版を作るか
- **次のアクション**: 設計書 v3 作成
- **関連**: `current_state/active_projects.md` の vidkit

### Q2. Salamat WBSサイトの公開状況
- **背景**: 5月1日ローンチ予定だった
- **未決**: 現在公開済みか、未完了か
- **次のアクション**: YDに直接確認 or サイトURL を直接見に行く

### Q3. Arte Grow 9月視察の具体プラン
- **背景**: 9月にフィリピン視察予定
- **未決**: 視察期間、訪問先、リサーチ項目、予算
- **次のアクション**: 8月中までにプラン確定

### Q4. Lecture Hub の本格運用
- **背景**: 本番デプロイ完了したが、APIキー登録など最終設定が残っている
- **未決**:
  - Anthropic APIキー登録
  - PAT 発行 → Claude Code Routine 設定
- **次のアクション**: 時間ある時に対応

### Q5. 平成たち祭 動画の構成案決定
- **背景**: リサーチ完了、構成案A/B/C/ハイブリッド の中から選ぶ
- **未決**: どれを採用するか、統一テーマ、冒頭3秒のフック
- **次のアクション**: 短時間で決断、編集着手

## 💭 アイデアレベル (まだ着手しない)

### I1. クリエイターチーム結成
- Taichi と一緒にクリエイターチームを作りたい
- 参考: 1bloom (akiya MOVIE) など
- Manus にリサーチ依頼済み

### I2. 僻地・フロンティア地域コンテンツアプリ
- BeReal的なフロンティア地域コンテンツアプリ構想
- 中央アジアなど情報の非対称性が残る場所がターゲット
- 現状: 構想段階

### I3. MacBook をカバン内でも稼働させる
- Claude Code を電車内でも動かしたい
- 結論: 熱問題で危険、別解 (Tailscale + SSH) を検討中

## 🔍 確認したいこと (YDに聞く)

- [ ] Yitao Film と Studio Metaliana のコラボ進行状況
- [ ] Apple バイトの継続状況・卒業後の予定
- [ ] 卒論のテーマ・進捗
- [ ] 9月フィリピン視察の具体日程

## 📝 解決済 (アーカイブ候補)

解決したものは `decisions/` に移動 + ここから削除

## 📚 関連

- [[active_projects]]
- [[recent_decisions]]
- [[current_focus]]
