# Vault 操作ログ

> このVaultへの全ての変更・操作を時系列で記録する
> Append-only (削除しない)
> Format: `[YYYY-MM-DD HH:MM] <操作内容>`

---

## 2026-05-18 (Vault誕生日)

[2026-05-18 15:00] Claude Code が Vault 初期構築開始 (Step 1-7 自動実行)
[2026-05-18 16:00] YD が Obsidian 起動 + Vault 認識 (Step 8 完了)
[2026-05-18 16:15] YD がコミュニティプラグイン3つインストール: dataview, templater-obsidian, calendar (Step 9 完了)
[2026-05-18 16:30] Claude Code が Git 初期化 + GitHub Private リポジトリ作成 + push (Step 10 完了)
[2026-05-18 16:35] Claude Code が ~/.zshrc にエイリアス追加: vault, vsync, vstatus, vlog (Step 11 完了)
[2026-05-18 16:40] Claude Code 構築タスク完了 (Step 13/13)
[2026-05-18 16:50] 動作確認テスト成功 — 新しい Claude Code セッションが起動シーケンス通り7ファイル並列読み込み + 敬語で完璧な現状要約
[2026-05-18 17:30] Claude (デスクトップアプリ) が今日の意思決定を decisions/2026-05-18_Obsidian_Vault構築完了.md に記録
[2026-05-18 17:31] Claude (デスクトップアプリ) が運用マニュアルを knowledge/programming/tools/obsidian_vault.md に作成
[2026-05-18 17:32] Claude (デスクトップアプリ) が log.md を再構成 (今後の標準フォーマットを確立)
[2026-05-18 17:45] Claude (デスクトップアプリ) が ~/.claude/CLAUDE.md を作成 (グローバルClaude Code設定、どのディレクトリで起動してもVaultを自動読み込みする仕組み)

## 並行作業: vidkit autocut機能の実装 (別Claude Codeセッション)

[2026-05-18 --:--] vidkit に autocut モード追加 (silence.py + fcpxml.py 新規)
[2026-05-18 --:--] ffmpeg silencedetect + FCPXML 1.13 シリアライザ実装完了
[2026-05-18 --:--] 2プリセット (lecture / vlog) 実装、テストクリップで検証成功
[2026-05-18 --:--] Skill登録: ~/.claude/skills/fcp-autocut/SKILL.md (キーワード自動ロード)
[2026-05-18 --:--] xmllint検証通過、フレーム精度の有理数時刻計算動作確認
[2026-05-18 18:00] Claude (デスクトップアプリ) が vidkit autocut の知見を Vault に保存
  - knowledge/programming/tools/vidkit.md 新規作成
  - decisions/2026-05-18_FCPXML_ラウンドトリップ採用.md 新規作成 (FCP操作方式の意思決定)
[2026-05-18 18:00] YD が Claude Code セッションでの振り返り・記録依頼 — log/mistakes/knowledge/decisions に追記
[2026-05-18 18:00] Claude Code が補足記録: Step 10 で git add 時にプラグインバイナリ (main.js/styles.css 計1.6MB) の追跡可否を AskUserQuestion で確認 → 「除外」決定、.gitignore に追加 (manifest.json と data.json のみ追跡)
[2026-05-18 18:00] Claude Code が補足記録: Step 11 で ~/.zshrc を Edit する際に「File has not been read yet」で1回失敗 → Read 後に再 Edit して成功
[2026-05-18 18:05] Claude Code が mistakes/claude_mistakes.md に A-4「Edit ツール使用前の Read 必須を忘れる」を追記
[2026-05-18 18:05] Claude Code が knowledge/programming/tools/claude_code.md を新規作成 (権限モード比較・プロンプト設計・強み弱み)
[2026-05-18 18:05] Claude Code が decisions/2026-05-18_Obsidian_Vault構築完了.md に補足リンクを追記 ([[claude_code]] への参照)
[2026-05-18 18:10] Claude Code が vsync 相当のコマンドで全変更を GitHub に push
[2026-05-18 18:15] Claude Code (別セッション) が Salamat WBSサイト トップページ2版を実装 — Claude Design カンプを Next.js 16 + Tailwind v4 + Sora/Zen Maru/Noto Sans JP プロジェクトに移植。Hero/Vision/Action/Report/Story/News/Footer 全セクション書き直し、scroll連動の飛行機SVGパスアニメ、GradientText + Typewriter、Tweaks Panel (5パレット/ダーク/Hero/Story/CTA/飛行機 切替+localStorage永続化) 実装。Impact Stats削除、tsc --noEmit + build 通過、dev HTTP 200確認。パス: ~/Downloads/07_開発・アプリ制作/salamat-website-v2
[2026-05-18 18:20] Claude Code が knowledge/salamat/wbs_team.md を新規作成、current_state/active_projects.md の #4 (Salamat WBSサイト) を Wix/8ページ前提 → Next.js/トップ2版完了に更新
[2026-05-18 18:30] YD が新ルール提案: knowledge/decisions の全ファイルに「うまく行ったこと / 詰まったこと / 次回チェックリスト」の必須3セクションを含める
[2026-05-18 18:32] Claude (デスクトップアプリ) が CLAUDE.md に「必須3セクション」ルール追加
[2026-05-18 18:33] Claude (デスクトップアプリ) が templates/knowledge_template.md と decision_template.md に必須3セクションを反映
[2026-05-18 18:35] Claude (デスクトップアプリ) が既存ファイル4つに必須3セクションを遡って追記:
  - knowledge/programming/tools/vidkit.md
  - decisions/2026-05-18_FCPXML_ラウンドトリップ採用.md
  - decisions/2026-05-18_Obsidian_Vault構築完了.md
  - knowledge/programming/tools/obsidian_vault.md (次のステップで対応)

## 並行作業: Lecture Hub 個人用転換 + Phase 2 全完了 (別 Claude Code セッション)

[2026-05-18 --:--] Lecture Hub に「個人専用にする = 認証システム全削除」の方針転換、Phase 2 残全項目を実装する依頼を受領
[2026-05-18 --:--] Phase A — 認証システム全削除: Supabase Auth / Vault / RLS / api_tokens / ai_keys / `(auth)` / `/api/v1` / Upstash を一掃。全クエリを PostgREST → Drizzle 一本化、AI キーは env vars 直読み。migration 0002_drop_multitenancy.sql 作成
[2026-05-18 --:--] Phase B1 — ダークモード: `.dark` クラストグル + globals.css 上書き + FOUC 防止 inline script + BlockNote 連動 (MutationObserver)
[2026-05-18 --:--] Phase B2 — ノート内 AI Slash メニュー: 「要約」「タスク抽出」を BlockNote の SuggestionMenuController に追加、`/api/ai/extract-tasks` (generateObject + zod schema) 新規
[2026-05-18 --:--] Phase B3 — 講義テンプレ Cron: vercel.json で 06:00 JST daily-journal + 07:00 JST 月 weekly-lecture。`日記`/`講義` ルートページを find-or-create
[2026-05-18 --:--] Phase C1 — 全文検索 UI: tsvector + plainto_tsquery + ts_headline、Topbar SearchBox に ⌘K ライブドロップダウン、/search ページ
[2026-05-18 --:--] Phase C2 — AI チャット履歴 UI: ai_threads/ai_messages 永続化 (onFinish コールバック)、/chat ページに @ai-sdk/react の useChat、スレッド一覧サイドペイン
[2026-05-18 --:--] Phase D1 — 数式 (KaTeX) / コードハイライト (Shiki) / PDF 埋め込み: BlockNote の createReactBlockSpec でカスタムブロック、schema.ts で defaultBlockSpecs を拡張
[2026-05-18 --:--] Phase D2 — 添付ファイル + Whisper: /api/upload (Vercel Blob 直アップ) + /api/transcribe (OpenAI whisper-1) + AudioBlock
[2026-05-18 --:--] Phase D3 — pgvector セマンティック検索: migration 0003_pgvector.sql (vector(768) + ivfflat)、Google text-embedding-004、保存時 after() で自動 re-index、/admin/reindex で一括
[2026-05-18 --:--] Phase D4 — オフライン編集 (Dexie + sync): IndexedDB に pages キャッシュ、PageEditor で dirty フラグ管理、online イベント + 60秒間隔で自動 flush、Topbar に SyncIndicator
[2026-05-18 --:--] リファクタ: plainTextFromDocument / plainTextAround (src/lib/blocknote/text.ts)、uiMessageText (src/lib/ai/messages.ts) 抽出 → 3 ヶ所の重複削除
[2026-05-18 --:--] CLAUDE.md (プロジェクトルート) 整備: Auth 復活禁止 / Drizzle 一本化 / ヘルパーの場所 / migrations 適用順 / `pnpm dev` 勝手起動禁止
[2026-05-18 --:--] vitest 導入 + 26 件のユニットテスト (text / embeddings / messages / templates / _auth) — 全パス
[2026-05-18 --:--] サンドボックスから Supabase の Postgres ポート (5432/6543) への TCP がブロックされ migration 適用不可と判明 → ユーザーは家で Supabase SQL Editor から手動適用予定
[2026-05-18 18:30] Lecture Hub の今日の作業を Vault に保存:
  - knowledge/programming/tools/lecture_hub.md 新規作成 (運用マニュアル + 今日の進捗履歴 + 家でやる残作業)
  - decisions/2026-05-18_lecture_hub_個人用転換.md 新規作成 (認証撤去の意思決定)
  - current_state/active_projects.md の #7 (Lecture Hub) を MVP 完成 → Phase 2 全完了に更新
[2026-05-18 18:40] CLAUDE.md の新ルール「必須3セクション (うまく行った/詰まった/チェックリスト)」を遡って Lecture Hub 関連 2 ファイルに追記:
  - decisions/2026-05-18_lecture_hub_個人用転換.md (Drizzle 移行・createReactBlockSpec ファクトリ・サンドボックス TCP ブロック等の知見を 3 セクションに整理)
  - knowledge/programming/tools/lecture_hub.md (BlockNote/pgvector/`after()`/同期インジケータの実装知見を 3 セクションに整理)
[2026-05-18 18:50] YD が Claude Code (autocut実装セッション) に「ここで行った内容は Vault に保存されているか確認して、足りない情報があれば追記して」と依頼
[2026-05-18 18:55] Claude Code が Vault 整合性チェックの結果、autocut機能・FCPXMLラウンドトリップ決定の主要記録 (decisions/、knowledge/vidkit.md) は既に保存済みと確認。current_state/ の 3 ファイルが stale だったため追記更新:
  - current_state/active_projects.md #2 vidkit を「autocut完成 (2026-05-18) + tighten が新NEXT」に更新、将来のFCPXMLオペレーション候補も追記
  - current_state/current_focus.md の vidkit次の機能を tutorial モード → **tighten (FCPXMLラウンドトリップ第一弾)** に変更
  - current_state/recent_decisions.md に「FCPXMLラウンドトリップ採用」と「vidkit autocut モード追加」の 2 件を新規ブロックとして追記 ([[2026-05-18_FCPXML_ラウンドトリップ採用]] と [[vidkit]] にリンク)
  - この log.md エントリ追記
[2026-05-18 19:00] Claude (デスクトップアプリ) が今日確立されたワークフロー自体を knowledge/programming/tools/vault_workflow.md として保存 (409行) — 1日の運用フロー、セッション開始の3パターン、保存フロー、並行作業の運用、月次メンテナンス、Phase 1→2→3 自動化フェーズ、必須3セクション付き
[2026-05-18 19:01] YD が vsync で baf3f52..c147ce6 を GitHub に push 完了 (Vault 構築日の作業を全 GitHub 同期)
[2026-05-18 21:30] Claude Code が Salamat WBSサイト (salamat-website-v2) を Vercel に初回デプロイ完了:
  - 公開URL (チーム共有用): https://salamat-website-v2.vercel.app (HTTP 200 公開アクセス可)
  - 個別デプロイURL: https://salamat-website-v2-pd68j01x0-yitao-dings-projects.vercel.app (401 = Vercel Deployment Protection 既定)
  - 経緯: ローカル `next build` 通過 (Turbopack 3.8s, 静的 prerender 2ルート) → `vercel --yes` 初回実行 → 「Preview選択」のつもりが CLI 仕様で Production target になった (新規プロジェクト初回 `vercel` は本番扱い)
  - 既存ブランチ: main (未コミット変更あり、デプロイには git not required で .vercel に紐付け)
  - 知見: knowledge/programming/tools/vercel.md 新規作成、current_state/active_projects.md #4 を「Vercel初回デプロイ完了」に更新

[2026-05-19 朝] Claude Code 権限システム整備 (グローバル `~/.claude/settings.json` に defaultMode:acceptEdits + Push/Deploy/Publish/sudo を ask に、settings.local.json から push/deploy 系 allow を除外、.zshrc に `claude-init` 関数追加、knowledge/programming/tools/claude_code_permissions.md 作成)
[2026-05-19 00:45] Claude Code (監視セッション) が 4プロジェクト並行監視ループを起動 (cron job 36352460, `*/5 * * * *`)。対象: WBS / Lecture Hub / vidkit / Vault。初回tickで検出: vidkit が git 未化、WBS が Initial commit 以降未コミット、Lecture Hub に多数未コミット (削除30+/新規20+)。現在の動き: vidkit が tighten 実装中 (Vault `raw/vidkit/tighten/` に出力)、WBS が design-brief 編集中、Lecture Hub は静止。
[2026-05-19 00:49] Claude Code (vidkit セッション) が vidkit に **tighten / tutorial / --vault-path** を 1 セッションで追加完了:
  - `fcpxml.py` に FCPXMLリーダー (`parse_fcpxml`) + 再シリアライザ (`write_fcpxml_from_parsed`) を追加 (汎用、後続オペレーションでも再利用可)
  - `vidkit/tighten.py` 新規 — クリップ内残り無音を ffmpeg silencedetect で検出 → 各 asset-clip を細分化 → timeline offset 累積再計算
  - `prompts/tutorial.md` 新規 — Claude Code が動画から `TUTORIAL.md` + `code/` を自走実装する 4 ステップ指示
  - `--vault-path` オプション全モード対応 (`<vault>/raw/vidkit/<mode>/` に出力)
  - 検証: 12秒テスト動画 (3発話+2無音) で autocut→tighten ラウンドトリップ成功、ラフ rough.fcpxml (1clip) → tighten で 3clips 分割 (3.6s 削除)、xmllint 通過、offset 累積和・フレーム整列すべてOK
  - 残: lecture モード (HF_TOKEN 取得待ち、YD 作業)、実FCPプロジェクトでの実機検証
  - 関連: [[2026-05-19_vidkit_tighten_tutorial_完成]]、[[vidkit]]
[2026-05-19 00:52] Claude Code (vidkit セッション) が tighten / tutorial を Skill 化: `~/.claude/skills/fcp-tighten/SKILL.md` (4.7KB) と `~/.claude/skills/video-tutorial/SKILL.md` (5.2KB) を作成。fcp-autocut と同じパターンでキーワードトリガー自動ロード可能に。
[2026-05-19 00:57] Claude Code (監視セッション) が tick 4 で Skill 2件の作成を検出、Vault 未反映のため log.md と active_projects.md (#2 vidkit の Skill 化チェックを [x] 完了マーク) を補正。
[2026-05-19 01:00 前後] Claude Code (vidkit セッション) が vidkit を **git 初期化 + GitHub Private に push 完了** — `git init` → 初期 commit `177a2f2` (Initial commit: 動画前処理 CLI (dance / lecture / autocut / tighten / tutorial)) → GitHub Private リポ `Yitao-Ding/vidkit` 作成 → push。これで監視ループ初期に検出した CRITICAL 問題「vidkit が git 未化」は完全解消。Untracked は `docs/lecture-setup.md` のみ (lecture モード仕上げで追加されたっぽいファイル、commit はまだ)。
[2026-05-19 01:02] Claude Code (監視セッション) が tick 5 で vidkit git 化を検出、active_projects.md #2 の「git 初期化 + GitHub Private push」チェックを [x] 完了マークに補正。
[2026-05-19 01:05 前後] Claude Code (vidkit セッション) が docs/ を 2 ファイル追加して 2 commit 目 `29deb68 docs: lecture セットアップ + tighten 実機検証手順を追加` を作成、push 済。同時に knowledge/programming/tools/vidkit.md を更新 (tighten/tutorial/--vault-path セクション + 必須3セクションを完全反映、ただし fcp-tighten / video-tutorial の Skill 登録情報は未追記、提案として保留中)。
[2026-05-19 01:08] Claude Code (監視セッション) が tick 6 を完了、YD 指示で監視ループ (cron 36352460) を一時停止。最終状態: vidkit ✅ git 化 + 2 commits + docs 2 件、Lecture Hub ✅ クリーン (.env.local だけ編集)、WBS ⚠️ 6 tick 連続 commit ゼロ (design-brief.md 編集のみ継続)、Vault は M 3 件 + 未 add の decisions/2026-05-19_vidkit_tighten_tutorial_完成.md 1 件で git push 未実行。

## 監視一時停止中 (01:08 → 02:13) の出来事

[2026-05-19 ~02:00] Claude Code (Lecture Hub セッション) が `2aca0d4 Fix array type mismatch in 0002 migration` を commit。家作業で `0002_drop_multitenancy.sql` を Supabase SQL Editor 適用試行中に発生した array 型の不一致を修正したと推察。

## Salamat WBS v3 — Phase 1 一気実装 (別 Claude Code セッション)

[2026-05-19 朝〜夕方] Claude Code (Salamat WBS セッション) が design-brief 収集 → 優先度付け → Phase 1 自走実装を1日で完了:
  - YD が 21st.dev 風コンポーネントを8個投入 (#01〜#08) → 都度 design-brief.md に構造化記録 (参考URL / YDコメント原文 / Claude抽出 / 詰めポイント / 判定)
  - Claude が「演出過密 / メンテコスト2倍 / 依存肥大化 / 情報伝達と装飾のバランス」を率直に懸念表明 → YD が「最初から完成形を目指す」哲学で Phase 2 持ち越し推奨だった #04 #05 を Phase 1 に繰り上げ判断
  - YD から「ここからの作業は全て Claude 判断、最後にレビュー」の委譲を受領
  - Phase 1 実装: #08 Hero (ZoomParallax × MeshGradient 融合, 300vh) / #06 Gallery4 構造で全カード統一 / #02 Cobe 地球儀 + #03 LocationTag で Action 再構築 / #04 Glowing Shadow + MeshGradient 簡略版でカード装飾 / #05 Radial Orbital Timeline で List⇄Orbital ビューモード切替
  - 新規 10ファイル + 書き換え 7ファイル、依存追加: three / cobe / @paper-design/shaders-react (既存に framer-motion / embla-carousel-react / lucide-react / clsx / tailwind-merge / class-variance-authority あり)
  - tsc / build / dev HTTP 200 通過、YD 目視レビュー「ちゃんと変わってます、大丈夫」
  - 詰まったこと: paper-design/shaders-react 0.0.76 の API がプロンプト記載と差異 (`backgroundColor` 廃止、`spotsPerColor`→`spots`、`frame` 廃止) → `node_modules` の .d.ts を直読みして対応
  - 写真ファイル名 vs 中身の不一致発覚: `ph-*.jpg` の中身が日本、`jp-*.jpg` の中身がフィリピン。YD指摘で Read ツール6枚並列確認 → コード上のマッピングを入れ替え (ファイル名はそのまま、Phase 2 cleanup)
  - decisions/2026-05-19_Salamat_WBS_Phase1実装.md 新規作成
  - mistakes/claude_mistakes.md に2件追記 (A-5: ライブラリ API バージョン差異、A-6: 画像ファイル名を信用しない)
  - knowledge/salamat/wbs_team.md に Phase 1 セクション追記
  - current_state/active_projects.md の #4 を Phase 1 完了 + Phase 2 残タスクに更新
[2026-05-19 ~02:00] Claude Code (vidkit セッション) が `40cef7d fix: variable-fps 動画 (Zoom録画/screen capture) を autocut/tighten で扱えるように` を commit。可変フレームレート動画 (Zoom録画・screen capture 系) で autocut/tighten が壊れていたのを修正。
[2026-05-19 02:13] Claude Code (監視セッション) が YD 指示で監視ループ再開 (cron 884eaa28、5分間隔)。Lecture Hub と vidkit の新 commit を補完追記、active_projects.md #2 vidkit に variable-fps 対応 commit を追記。Vault は依然 push 未実行 (M 3 + 未 add 1)。WBS は引き続き 1時間以上 commit ゼロのまま design-brief 編集続行。
[2026-05-19 02:36] Claude Code (監視セッション) が YD 指示で `~/.claude/settings.json` の `permissions.allow` に **`Bash(*)`** を追加。監視 tick の複合 Bash (`P="..."; printf ...; git -C ...; find ...`) が毎回 ask されていた問題を解消。既存 ask (push/deploy/publish/sudo 系) は `deny > ask > allow` 優先順により維持。`knowledge/programming/tools/claude_code_permissions.md` の更新履歴にも反映。
[2026-05-19 02:32] (commit `10f0dc8` で記録) Claude Code (vidkit セッション dive 2) が Vault に vidkit セッションの追加成果を保存: decisions/2026-05-19_vidkit_tighten_tutorial_完成.md 拡張 (実機47分動画検証 / GitHub Private / Skill 2件 / docs 2件 / variable-fps fix を必須3セクションに反映)、knowledge/programming/tools/vidkit.md 拡充 (5モード一覧 / variable-fps 注意点 / GitHub URL / Skill 3件 / docs リンク)、log.md 28行追記。active_projects.md は監視セッション側との競合を避けて除外 (連携OK)。
[2026-05-19 ~02:40] YD or Claude デスクトップアプリが Vault に **大方針転換** を記録: `decisions/2026-05-19_AI学習スプリント開始.md` 新規 (AI学習スプリント + 朝ブリーフィング + 教科書システム + 4並列セッション割り当て A/B/C/D)、`identity/profile.md` 更新 (就活終了 + メアド 2系統 yitao0907@gmail.com / save.yitao@gmail.com + 進路状況)、`current_state/active_projects.md` の last_updated を「AI学習スプリント開始 + 朝ブリーフィング + 教科書システム追加、就活終了」に更新。自分 (監視セッション) は割り当て上「セッションD: 既存監視ループ継続」担当として明示された。
[2026-05-19 02:41] Claude Code (監視セッション) が tick で WBSサイトの本格実装開始を検出: 直近6分で 11 ファイル変更 (新規 `cobe-globe.tsx` / `mesh-gradient-shader.tsx` / `gallery-carousel.tsx` / `location-tag.tsx` + Hero/Story/Report セクション更新)、design-brief 編集フェーズから本格コンポーネント実装フェーズへ移行。ただし依然 commit ゼロ (8 tick 連続)。
[2026-05-19 02:55] Claude Code (監視セッション) が Vault の全 dirty 変更を 1 commit にまとめて push 完了 (`6646176 AI学習スプリント開始 + 監視ループ補正 + 権限拡張 (Bash(*))`、25 files / 1589+ / 10-)。`10f0dc8..6646176 main -> main` で GitHub `Yitao-Ding/yd-obsidian-vault` と完全同期。
[2026-05-19 02:58] Claude Code (監視セッション) が YD 指示で監視対象を **4→8 に拡張**: 既存 (WBS / Lecture Hub / vidkit / Vault) + 新規 (textbook-engine / morning-briefing / learning/ / textbook/)。新 cron job で再起動。新規4対象の現状: `~/projects/textbook-engine` 未存在、`~/projects/morning-briefing` 未存在、`~/ObsidianVault/textbook` 未存在 (セッション A・B 起動待ち)、`~/ObsidianVault/learning` は anthropic_academy 17コーステンプレ + books/ + podcasts/ 構造で存在 (セッション C 進行中)。
[2026-05-19 03:00 前後] **セッション A・B・C 3つ同時起動を検出**:
  - セッションA (教科書PDFパイプライン): `~/projects/textbook-engine/` 出現 (`requirements.txt` 1 ファイルで初期化開始)、`~/ObsidianVault/textbook/README.md` 作成 (Vault 側ハブ立ち上げ)
  - セッションB (朝ブリーフィング): `~/projects/morning-briefing/` 出現、**32 ファイル構造完成**: pyproject.toml / config.yaml / README.md / .env.example / .gitignore / .python-version、src/ 配下 4 モジュール分割 (`collectors/` / `renderer/` / `synthesizer/` / `uploader/`)、output / logs / credentials の `.gitkeep`。git init 済みだが commit はまだ (DIRTY=9 全 untracked)
  - セッションC (AI学習ログ): `~/ObsidianVault/learning/` を大幅拡張 — `learning/README.md` 新規、`books/_template.md` + `books/README.md` 新規、`podcasts/_template.md` + `podcasts/README.md` 新規、`ai_certifications/` に新 3 サブフォルダ (claude_certified_architect / google_ai_professional / google_cloud_gen_ai_leader) と各 README.md、anthropic_academy 配下 17 コーステンプレも更新進行中
  - セッションD (監視継続) = 自分
[2026-05-19 03:00 前後] Vault DIRTY=8 で push 後の追加分が溜まり中 (主に learning/ と textbook/ の新規ファイル)。commit + push は YD 指示で随時。
[2026-05-19 03:06] tick 進捗 — セッションA: `textbook-engine` 8ファイルへ拡大 (`setup.sh` / `build.sh` / `requirements.txt` / `README.md` / `templates/style.css` / `templates/template.html` / `src/build.py` / `src/__init__.py` — PDFパイプライン骨格)。セッションB: `morning-briefing` で uv 環境構築完了 (`.venv/` 作成、`uv.lock` 追加、`bin/morning-briefing` 実行ファイル生成)。Lecture Hub も動き出した — `src/components/editor/schema.ts` と `ai-slash-items.tsx` 編集 (Phase 2 完成後の調整か継続実装、要 watch)。WBS は 13 tick 連続 commit ゼロのまま静止。
[2026-05-19 03:11] tick 進捗 — **★ セッションB が試作 PDF 生成達成** (`morning-briefing/output/2026-05-19_morning_briefing.pdf` + `.html` 出力、`templates/briefing.html.j2` + `briefing.css` テンプレ作成、`src/renderer/pdf.py` + `src/renderer/tts.py` + `src/synthesizer/briefing.py` 実装)。**★ WBS担当 CC が Vault に decisions 追加**: `decisions/2026-05-19_Salamat_WBS_Phase1実装.md` 新規 + `knowledge/salamat/wbs_team.md` 更新 (WBSの本格コンポーネント実装フェーズの記録)。セッションA: `textbook/_template/textbook_template.md` 追加 (Vault 側に教科書テンプレ)。Lecture Hub: `src/components/editor/Editor.tsx` 編集続行 (DIRTY=5)。WBS は 14 tick 連続 commit ゼロのまま。
[2026-05-19 01:00] Claude Code (vidkit セッション) が dive 2 を完走:
  - **variable-fps fix** (commit 40cef7d): Zoom録画 / screen capture / オンライン講義 系の `r_frame_rate=600/1` 誤検出を `_pick_fps(r, avg)` で吸収。`_pick_frame_duration` 許容を 0.1 → 0.5 に緩めて 30.302 → 30 スナップ可
  - **実機ラウンドトリップ検証**: `オンデマンド経営学５-3.mp4` (852x480 / 30.302fps / 47m31s) で autocut → 479 keeps / 196.6s 削除 (2.25s 完走) → tighten で +0.8s (ほぼ冪等) → rough(1clip)→tighten で 479 clips / 196.9s 削除 (autocut と整合)、xmllint 通過
  - **GitHub Private 化**: https://github.com/Yitao-Ding/vidkit (Initial commit + docs commit + variable-fps fix commit の 3 commits)
  - **Skill 2 件登録**: `~/.claude/skills/fcp-tighten/SKILL.md`、`~/.claude/skills/video-tutorial/SKILL.md` (新セッションでキーワード自動ロード)
  - **ユーザーガイド** (vidkit プロジェクト内): `docs/lecture-setup.md` (HF_TOKEN 取得 5 ステップ)、`docs/tighten-howto.md` (FCP 実プロジェクト往復手順)
  - Vault 反映 (このコミット): [[2026-05-19_vidkit_tighten_tutorial_完成]] に検証(E) / 配布・運用 / 必須3セクションを追記、[[vidkit]] に variable-fps 注意点 / Skill / docs / GitHub リンク追記

## AI学習スプリント Day 0 (基盤構築) — セッションC

[2026-05-19 03:00] Claude Code (セッションC) が **AI学習スプリント管理基盤を構築完了**:
- `~/ObsidianVault/learning/` 新規作成、配下に **28ファイル** 生成
- 構造:
  - `learning/README.md` (3軸ハブ: 資格 / 書籍 / ポッドキャスト)
  - `learning/ai_certifications/README.md` (4資格進捗ダッシュボード)
  - `learning/ai_certifications/anthropic_academy/README.md` + 個別コーステンプレ **18枚** (公式ページに18コース公開を確認、decisions文書の17コースから1増)
  - `learning/ai_certifications/{claude_certified_architect, google_ai_professional, google_cloud_gen_ai_leader}/README.md` (各試験概要 + 準備状況 + 受験予定)
  - `learning/books/{README.md, _template.md}` (朝ブリーフィング推薦本ログ用)
  - `learning/podcasts/{README.md, _template.md}` (購読9番組: 英Tim Ferriss/Lex/Acquired/MFM/All-In + 日COTEN/Off Topic/西野/a scope)
- 各個別コースファイルは frontmatter で `status`/`started_at`/`completed_at`/`cert_url` を機械可読管理、CLAUDE.md (Vault憲法) の必須3セクション (うまく行った/詰まった/チェックリスト) 含む
- 推奨受講順序 (2週間プラン): Day 1=基礎2本 / Day 2=製品 / Day 3=Claude Code / Day 4-6=API+MCP / Day 7=Skills+Subagents / Week 2=Cloud+業界別Fluency
- 「YDへの引き寄せ」セクション全コースに記入: Salamat (260名NPO) / Arte Grow (SMB) / vidkit / Lecture Hub / Task Hub / 撮影業 への接続を1-3行でメモ
- 試験情報を Web で最新確認: CCA-F (60問/120分/Early Access 5000人無料以降$99/5領域=Agentic 27%, Claude Code 20%, Prompt 20%, MCP 18%, Context 15%)、Google AI Pro (7モジュール+capstone/$49月/Google AI Pro 3ヶ月特典)、Google Cloud Gen AI Leader (50-60問/90分/$99/4領域)
- active_projects.md に「★ AI学習スプリント ★」を最重要エントリとして挿入、既存番号は維持
- 自走モード遵守: 設計判断 (進捗管理フォーマット / コース番号付け / カテゴリ分類 / 推奨受講順) は自分で決めて進行、YD確認は不要範囲

## 教科書システム構築 — セッションA

[2026-05-19 03:00 前後] Claude Code (セッションA) が **YD専用教科書システムを構築完了**:
- `~/ObsidianVault/textbook/` 新規作成: README.md (目次・進捗管理) + 5領域ディレクトリ (00_basics / 01_web_development / 02_video_processing / 03_ai_engineering / 04_tools) + `_template/` (統一テンプレート) + `_output_pdf/` (PDF出力) + `_assets/mermaid_cache/` (Mermaid PNG キャッシュ)
- `~/projects/textbook-engine/` 新規作成 (ローカルのみ、git未): Python製 Markdown → 縦長PDF 変換ツール。スタック = WeasyPrint 62 + markdown + pymdown-extensions + Pygments (one-dark) + Jinja2 + Mermaid CLI (npx mmdc)
- `setup.sh` 一発でシステム依存 (brew: pango / cairo / gdk-pixbuf / libffi) + Python venv + Pythonパッケージ全部入る。Apple Silicon の dylib 不可視問題は `build.sh` で `DYLD_FALLBACK_LIBRARY_PATH=/opt/homebrew/lib` を通して解決
- `templates/template.html` + `templates/style.css` で**雑誌風レイアウト**を実装: ネイビーグラデーション + アクセント円 + Kicker badge のカバーページ / h1 採番 / h2 縦線アクセント / コードブロックは ダーク背景 + 左ボーダー / Mermaid 図は薄背景カード化 / ページ番号は `N / total` 表記 / 末尾に奥付ページ
- `_template/textbook_template.md` 統一テンプレート策定: 7セクション (1.何が起きた / 2.図解 / 3.キーコンセプト / 4.コード解説 / 5.チェックリスト / 6.関連リンク / 7.用語集)、Mermaid と用語集は必須
- 第1号 `textbook/03_ai_engineering/01_claude_code_parallel.md` 執筆完了: 「Claude Code 4並列で何が起きてるか」、Mermaid 2枚 (flowchart + sequence)、コードブロック 8個、用語集 17項目、最終 **A4 12ページ / 753KB** PDF として `_output_pdf/01_claude_code_parallel.pdf` に生成
- ビルドコマンドは 1コマンド: `cd ~/projects/textbook-engine && ./build.sh <markdown_path>` で `textbook/_output_pdf/` に自動出力 (textbook ディレクトリを自動検出)
- 設計判断 (自走モード遵守): PDFライブラリ = WeasyPrint 採用 (reportlab は低レベル過ぎ、Playwright はchromium重い) / フォント = macOS 標準ヒラギノ角ゴ ProN (Noto Sans CJK は追加依存) / Mermaid = mmdc で PNG レンダリング + SHA1 ハッシュキャッシュ / シンタックスハイライト = Pygments one-dark
- 詰まった点 (3回未満で解決): (1) WeasyPrint の libgobject 不可視 → `DYLD_FALLBACK_LIBRARY_PATH` で解決 / (2) CSS変数 `var(--navy-deep)` を radial-gradient 内で使うと WeasyPrint がコケる → ハードコード `#051728` で解決
- Vault 反映 (このコミット): `decisions/2026-05-19_AI学習スプリント開始.md` の「4並列セッション割り当て」に セッションA 完了マーク、`current_state/active_projects.md` に教科書システム追加、`knowledge/programming/tools/textbook_engine.md` を新規作成 (必須3セクション付き)
