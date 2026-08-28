---
type: current_state
last_updated: 2026-08-28 (ハタチたち公式HP Phase1完了・新規追加、詳細はアクティブ先頭 / 旧2026-08-08b: プペルOP映像YF-13本制作完了・納品ファイル生成、詳細セクション先頭 / 同日: Arte Grow: 軸を「映像で伝える」に再定義・渡航10/1-2確定・Notion新ページ新設、詳細#5 / 旧2026-08-05注記: CC-business: noteオンリー分売の全7記事完成原稿・YD承認済 / 平成たち祭: 時系列整列TL2本追加。詳細は各セクション参照。※frontmatter 21日放置を外部監査2026-08-05指摘5で是正、旧7/14注記はCC-business本文へ)
update_frequency: 週1回以上
---

# 進行中プロジェクト一覧

> このファイルはYDの「今アクティブに動いてるもの」のスナップショット
> 完了したものは `archive/` へ、休眠中は `archive/sleeping/` へ移動
>
> ⚠️ **2026-06-14 全件棚卸し済**: 実体ベース(git log/mtime/HANDOVER)の全プロジェクト棚卸しを `~/projects/AUTONOMOUS_SESSION_2026-06-14.md` に作成。本ファイルの各ステータスは5月下旬〜6月上旬時点なので、続ける/殺すの判断はそちらの「即決マトリックス」を参照。要点: ①大半は "YD待ち"(認証/本名/素材) ②CC-businessが最も本番ローンチ間近 ③ai-researcher/morning-briefing は "動くが成果が届かない" 状態。

## 🟢 アクティブ・優先度高

### ★ ハタチたち 公式ホームページ (2026-08-28 着手) ★
- **状況**: 🟢 **多観点レビュー反映済 (v5)** — Phase 1 実装後、11エージェント多観点レビュー (46件) + 完全性クリティック (14件) を反映。主要修正: next/font変数を `<html>` へ移動 (明朝フォント全ページ有効化) / 日本語preload 123→1 / YouTube facade実装 / microCMS error shape修正 / ビルド・lint緑・`git commit`完了。HANDOVER.md + docs/3ファイル (microcms-setup.md / assets-list.md / accounts.md) 整備済み
- **パス**: `~/AI projects/hatachi-tachi-website` (※`projects/` でなく `AI projects/` 配下)。**引き継ぎ正本=同ディレクトリの `HANDOVER.md`**
- **草案の実態**: 7セクション中、ヒーロー (赤い手書き風「ハタチたち」+円陣写真) / NEWS (紺の斜めカット帯「ハタチたち5開催決定！」) / ABOUT (珊瑚レッド全面) の3つだけ実コンテンツ済。NOW PLAYING / LOCK BACK (アーカイブ6枠: 初代〜5+平成たち祭) / INSTAGRAM / フッターはプレースホルダーだった
- **YD確定事項**: 更新の仕組み=microCMS無料枠 (毎年二十歳の代に総入れ替えする素人運営が管理画面で更新できることが最優先) / Instagram=手動埋め込み (Meta API・外部ウィジェットは運用が重く不採用) / 公開先=Vercel+最初から独自ドメイン / サイトの役割=①信頼性の担保 (保護者・会場・協賛向け) ②当日の告知・案内、応募フォーム自作は不要
- **設計**: microCMS 3 API (news / archive / site) → 公開時Webhook → Vercel Deploy Hook 再ビルド。SSG + `revalidate=3600` 保険。**microCMS未接続でも `src/content/site.ts` のフォールバックで全ページ成立** (env待ちで実装が止まらない設計、今後も崩さない)
- **★ 品質パス完了 (2026-08-28)**: 11エージェント×2系統 (5観点レビュー+敵対的検証+完全性クリティック / セクション別精密仕様+情報設計リサーチ+デザイン審査) を回し、確定46件+14件を反映。**明朝が全ページで一切適用されていない** (next/fontの変数を`<body>`に付けたため`:root`から解決できずCSS変数チェーンが無効化) / **font preloadが123本** / microCMSの例外を全握りつぶし / ブランド色が草案とズレ / 草案の「写真がレタリングを食う」重なり構図の欠落、を検出して修正。コントラストはレンダリング結果の実測で32要素すべて適合 (ブランド色 #ff5757 は変えず、サイズと配置でAAを満たす方式)。commits `d4498e4`→`bb70920`
- **★ 判明した実データ**: 公式IG [@hatachi_tachi](https://www.instagram.com/hatachi_tachi/) (川崎市・相模原市・福島市協力、2026-08-01にハタチたち5の応募フォーム告知) / 公式YouTube [スタジオメタリ](https://www.youtube.com/@Studio_Metali) / 旧公式サイトはWix (`hatachitachi100.wixsite.com/hatachi-tachi`、初代のみの内容) / 過去作4本のURL確定 (初代=二十歳/LUCCI、2=生きるをする/マカロニえんぴつ、3=かくれんぼ/AliA、4=ピーターパン/優里) → 全部 `src/content/site.ts` に反映済み
- **★ ドメイン (2026-08-28 YD指示)**: `studiometali.com` / `hatachitachi.com` をお名前ID 20382623 のカートに入れ**支払い画面まで到達**。合計1,906円 (各750円+サービス維持調整費203円、Whois情報公開代行0円込み、レンタルサーバー・追加オプション無し)。**カード入力はYD作業**。操作手順は [[chrome_session_cdp]]
- **次のアクション (YD作業)**: ①**お名前.comの支払い画面でカード入力→「申込む」** (Chromeウィンドウを前面に出してある) ②microCMSアカウント+API3本作成 → `docs/microcms-setup.md` (フィールドID1文字違いで無言でフォールバックに落ちる) ③素材提供 → `docs/assets-list.md` ④確認5件 (LOCK BACK表記 / 「二十歳の台」→「代」/ アーカイブ枠数 / 応募導線の優先度 / 赤の濃さ) ⑤`docs/accounts.md` で GitHub・Vercel・レジストラの名義を決める (個人名義だと翌年の代が触れなくなる)
- **残 (Claude作業)**: Phase2 microCMS疎通 → Phase3 実素材差し替え → Phase4 デプロイ+ドメイン+Webhook (push/deployは都度YD確認) → Phase5 非エンジニア向け更新マニュアル
- **関連**: [[2026-08-28_ハタチたちHP_スタック確定]]

### ★ プペルOP映像 YF-13 (紗幕投影35秒、クライアント: はるきさん) ✅✅納品済み (YD口頭確認 2026-08-11)
- **状況**: ✅ **納品ファイル完成** — `ALL WORKs/ぷぺる_2026-08/納品/プペルOP映像_曲フル尺_v1.mp4` (1920×1080 / 曲「image」フル尺5:19.8 / 頭35.4秒に映像→34.95秒=イントロ明けで消滅→以降黒+音のみ) + 確認用頭36秒720p。納期希望8/16に対し8日前倒し
- **素材**: 8/8着「ぷーnumber素材」15本 = 過去公演9回分 (煙の向こう2022〜夢追い人) の画面収録 → `動画/素材_ぷー過去公演_20260808/` にタイトル別リネーム済。全数目視済 (⑧想いの11s以降IG UI映込み=使用禁止等はREADMEに記録)
- **設計**: ラフv10承認モーション継承+実素材15点 (全0.5倍速回想スロー・⑮オーブ新設・素材別hueグロー・終盤火の粉)。パイプライン正本=`動画/本制作_20260808/README.md` (canvas決定論エンジン+puppeteer書き出し、部分再レンダー可)
- **次のアクション**: ①~~LINE送付~~ ✅納品済み (2026-08-11 YD確認) ②FB次第でv2 → **⚠️ 注意: 旧Mac故障で制作パイプライン・納品mp4・素材15本のローカル分が消失 (2026-08-11棚卸し)。v2再レンダーが必要になったらギガファイル素材再DL+再構築が前提** ③請求 (言い値+曲購入費実費) **← ⚠️ 監査指摘 2026-08-25: 納品から2週間経過で未対応。最優先対応**
- **関連**: [[2026-08-08_プペルOP映像_本制作完了]]

### プペル エンドロール動画制作 (クライアント: はるきさん、2026-08-02 受領開始)
- **状況**: 🟢 **v10完成・QC第3パス修正完了・メイキングrecut進行中 (2026-08-28 20:29)** — v9 QC(21エージェント)でblocker1件/should-fix多数検出。blocker根本原因: 「最後の1点を抜けるまで保持」の変更により最後が動画の13枠でクリップが2.97秒で尽きカードが1.5〜2秒黒くなる→`<Loop>`で修正。追加修正: 4点枠の切替スケジュールを後ろ寄せ(完全表示ゾーンに写真を乗せる)・FADE 0.5→0.3(集合写真クロスフェードで顔が二重になる問題)・1-1の素材順序入れ替え(p0の下切れ解消)。**v10=Part A全20枠カード再セレクト + Part Bメイキング5カット32秒**。149MB / 送付用41MB 完成。黒落ち解消を6箇所で輝度計測確認済み。wf_render_qc v10版実行中(QC第3パス)。★ v9以前の状況: v8 QC(21エージェント)でblocker6件/should-fix27件検出。対策: スケジュール点数決め打ち+1-3連写除外+1-4棒立ち→踊り差し替え+2-2/2-6頭出し修正+切り抜き6点+再クロップ2点。**✅ 2-9動画は解決: 2-9ナンバーからの要望で入れる素材とYD確定 (2026-08-28)。差し替え禁止**。**★ Part B メイキング挿入 完了 (2026-08-28)**: 182本全数スクリーニング(star3=48/star2=73)→34本精査→90区間プール→3案審査で構成確定。**5カット32.00秒**=①A021C020 8.5-14.7 現場の引き(音響枠) ②A021C118 28.4-36.9 唯一の踊り8.5秒(照明枠) ③A021C033 59.6-65.1 楽屋メイク(舞台枠) ④A021C005 1.8-7.2 8人が両手を上げる密度のピーク(ぷぺるteam枠) ⑤A021C067 67.0-73.4 赤い布と満面の笑い・素材唯一のgrade5(町長枠)。4Kクリップ=`remotion/public/making/cut00〜04.mp4`、正=`_cc_work/making_20260828/cuts.json`+`cuts_applied.md`。スタッフクレジットの切替をカット境界に一致させる実装済み。**メイキング素材の実在パスは `/Volumes/Yitao HDD/ぷぺる_タイムラプスメイキングなど/ぷぺるコレオ/` (旧記述のExtreme proは誤り)**。**カード表示仕様判明: 1枠あたり約10.2秒・完全表示ゾーン4.6秒 → 実質3〜4点まで(点数ごとにスケジュール決め打ち)**。**YD確定: 動画しかない枠(1-6瀬梨奈・guestみかづきんぱにー)は動画始まりでOK**。素材全数受領完了(2026-08-26)。2-2さえ再アップ受領(jpeg1+mov1)で**全20枠(19演目+guest)欠番ゼロ**。2-4に音源`ogawan number.mp3`同梱(用途要確認)。知見=[[remotion_endroll_card_display]]
- ~~欠番 (残1): 2-2 さえ~~ → ✅ 2026-08-26 解消。1-6 瀬梨奈は8/23再アップ受領 (動画8本)
- **Remotion導入 (2026-08-22 YD指示)**: `remotion/` にv4.0.515セットアップ・4K疎通確認済み (演目データ駆動=`remotion/src/data/programs.json`)。8/5決定のAE素材キット方式 (`_cc_work/ae/`) と並存、用途整理はYD確認待ち
- **★ 背景v2完成 (2026-08-22夜)**: Claude Design (Fable 5 Max) でプペル公式画準拠の背景を再制作 (汎用夜景→差し戻し→YD承認)。AE `_cc_work/ae/プペル_エンドロール_v2.aep` (BG_v2/PartA_v2/PartB_v2、4K59.94p) に移植、**Designとのピクセル照合0.047%で一致を機械証明**。正本=`_cc_work/ae/README_v2.md`、フォントZen Old Mincho導入済み。関連=[[2026-08-22_プペル_エンドロール_Design経由AE_v2背景]]
- **素材の性質**: 完パケ動画ではなくオフショット写真+短尺動画 → スライドショー形式エンドロール想定
- **仕様確定 (2026-08-02)**: 音源=「えんとつ町のプペル」(毎回同じ) / 映像は頭から**2:02**まで(紗幕上がってコレオ実登場→以降音源のみ、音源全体約3:56) / 参照=前回エンドロール(`_参照/前回エンドロール_参照.mp4`、IG reel DYJiK0_z9jB) / 解像度=**4K確定** / 音源=YD購入
- **★ Part B素材確定 (2026-08-02 YD指示)**: ~~`/Volumes/Extreme pro/ぷぺるコレオ/メイキング素材/`~~ → **実際は`/Volumes/Yitao HDD/ぷぺる_タイムラプスメイキングなど/ぷぺるコレオ/メイキング素材/`** (182クリップ/総尺313.6分) + タイムラプス7本。Extreme proのパスは古い (2026-08-28 HANDOVER修正済)。演目フォルダの縦iPhone動画ではない
- **実装**: Remotion採用・デザイン先行・工程ゲート3段 (スタイルフレーム→1演目5秒→全編)。**制作正本=`ALL WORKs/プペル_エンドロール素材/HANDOVER.md`**
- **★ テロップ表記リスト受領 (2026-08-23)**: 全20枠 (演目名/choreographer/guest/assistant/member 計279名) + スタッフ5枠 (舞台/照明/音響/ぷぺるteam/町長)。正=`_テロップ表記リスト_受領20260823.md`、構造化済=`remotion/src/data/programs.json`。演目順もM番号で確定。~~細部要確認: 2-7の☺絵文字 / guestみかづきんぱにー詳細~~ → ✅ **2026-08-28解消**: 2-7の絵文字=「☺︎」(テキスト表示、`programs.json`更新済)。みかづきんぱにーは団体名のみ・個人名は一切不要 (YD確定)。残=2-9のguest,assistant見出し
- **★ 全編サンプルv3 (2026-08-23、YD確定反映)**: 映画式等速ロール (下→上、カット0) + ポラロイド複数枚 (写真+6秒動画クリップ計77点、**枠は傾き固定・写真だけ0.5sクロスフェード**=YD指示) + **背景=右→左スクロール (YD採択。鏡像タイル3840px帯・14px/s。ドリー案は`EndrollDolly`コンプに温存)** + 煙リアル化 (口元の熱揺らめき+パフ14個/本の個体差+乱流ディスプレースメント) + 星瞬き。`remotion/out/endroll_sample_v3_scroll.mp4` (原本119MB) + 送付用30MB、YD送付済み。仮=メディア自動セレクト/PartB背景/2-7絵文字。実装=`remotion/src/Endroll.tsx` (決定論組版、v1静止版は`Endroll_static_v1.bak.tsx.txt`)
- **★ v4窓明かり修正 (2026-08-23 YD指摘対応)**: 窓を建物実面のみに打ち直し (霞バンド・煙突上の浮き窓を根絶)、近景=明・密/中景=中間/遠景=消灯の奥行き差。生成=`_cc_work/design_v2/gen_windows_v2.py` (seed固定、レクト内クランプで機械保証)。AEプレート同期済み。`remotion/out/endroll_sample_v4.mp4`+送付用30MB、YD送付済み
- **★ v5構造物の灯り除去 (2026-08-23 YD指摘対応)**: 煙突シャフト・縞帯・配管・マスト・ドーム付きタンクを除外ゾーン化し窓ごとに当たり判定 (侵入0を機械検証+煙突3本/タンクの拡大目視)。窓351→244。`endroll_sample_v5.mp4`+送付用29MB、YD送付済み。AEプレート同期済み
- **★ v6構成確定 (2026-08-23 YD指示)**: ロール0:00-1:32 (曲調が落ち着く位置で終了) → メイキング枠1:32-2:04+スタッフローワーサード → **2:04サビ頭で消灯暗転**、総尺124秒。メイキングのクリップ選定はYD保留中 (プレースホルダ)。`endroll_sample_v6.mp4`+送付用28MB、YD送付済み
- **★ 2026-08-28 修正 (YD指示)**: ①前半ワープ解消=全クリップを5.5秒以上に切り直し (保持時間を上回ることを数値検証済)。②クレジットレイアウト確定=部門名+最大2行、3行目は2列横並び、1行分下げ、下グラデ強化+文字テキストシャドウ追加 (白ホリ対策)。③QC3修正完了: 1-2控室のアニメモニタをクロップ (左25%除去)・1-1/1-3も修正確認済み (fix_sheet.jpg で輝度・構図確認)。④Part B メイキングrecut: 67候補区間から8〜10カット構成中 (20:29時点進行中)
- **未確定 (残)**: Part B メイキングrecut 完了 / 2-9のguest,assistant見出し / 納品形式(2:02切りか黒含め全長か) / 納期 → 正本=`_案件仕様.md`
- **関連**: [[2026-08-02_プペル_エンドロール素材受領_ギガファイル一括DL]]


### ★ ダンちゃり長野ドキュメンタリー「一匹でも」編集 (2026-07-16 夜間自走ビルド) ★
- **状況**: 🟢 **本編完成** — `動画/2025-07-06_Nagano/編集_1本版/preview/一匹でも_preview_v1.mp4` (7:42、1080p、字幕63キュー焼込、-15.0 LUFS)。Resolveプロジェクト「一匹でも_ダンちゃり長野」/タイムライン「一匹でも_v2」が手直し用の正。検収=`編集_1本版/REPORT_一匹でも_納品検収.md` (指示書チェックリスト9 PASS+ラウドネス1件記録付き逸脱)
- **体制**: Fable司令塔+サブエージェント5系統 (Resolve API+EDL駆動編集、builder非依存の独立監査12系統 — クライマックス音量ミス・匿名化漏れ・黒画面15.5秒を実際に検出→修正)
- **走行中**: TA6注釈テロップ (Q5飼い主補足、文言YD確定済) の配置+再レンダーのみ
- **次タスク (新CC)**: 発言字幕の視認性改善 (白飛び背景と同化する箇所にキュー別の実測ベース処置。**角ゴ固定・明朝禁止**=YD確定ルール)。**引き継ぎ正本=`動画/2025-07-06_Nagano/HANDOVER.md`**
- **試写前の先方確認**: Q5ブロック採否 / TA6採否 / 施設映像公開可否 / 「横山さん」表記 / 金額字幕解禁

### ★ OOO Studio ブランド基盤構築 (Google Workspace + 独自ドメイン、2026-07-08 着手) ★
- **状況**: 🟢 **ドメイン取得・Workspace作成・DNS設定 完了、伝播待ち(最大72時間)**
- **背景**: Notion刷新 → Google連携にはWorkspace必須と判明 → 法人化・Hi,Me:)含むメンバー増を見据えたブランド基盤を新設する方針に発展
- **ブランド構成**: 親=OOO Studio(法人・Workspace・ドメインの器、意味を持たせないプレーン造語) / 子=Yitao Film継続(@yitao_film資産維持) / Hi,Me:)は将来ダンスレーベルとして同傘下に置ける設計
- **確定事項**: ドメイン`ooo-studio.jp`(お名前.com取得済、期限2027/07/31) / Google Workspace Business Standard(2TB・Gemini全アプリ・Meet録画重視、まずYD1人分のみ契約) / 管理者アカウント`yitao-ding@ooo-studio.jp`
- **次のアクション**:
  - [ ] **DNS伝播確認**(数時間〜最大72時間後)→ Google Workspace管理画面で「確認」ボタンを押し所有権証明を完了させる
  - [ ] 所有権証明後、Gmail開通設定(10分)
  - [ ] Whois情報公開代行の設定状況を確認(本名保護、取得時に有効化推奨だった)
  - [ ] お名前.comに残る不要な未申請ドメイン(`ooostudio.com`のプレミアム提案)を放置でOK、購入はしていない
  - [ ] Hi,Me:)の他2名との共有・持ち分・意思決定権の合意(法人化前に詰める必要あり、未着手)
- **関連**: [[2026-07-08_OOO-Studio_ブランド基盤構築]]、`identity/profile.md`(本名「丁一韜」表記追記済み)

### ★ パソコン1台の学校 (SNS情報商材システム、2026-06-03 着手) ★
- **★ 0→100全面リバンプ+文体改稿 ✅完了 (2026-07-11→07-13、Fable司令塔/Opus実装/Sonnetゲート)**: YD指示「フロント/バック/UI/UXを根本から全部見直して全部修正」+「文章のAIっぽさを排除」→ ①全方位監査143件(反証検証付き・誤検知0) ②8WP実装(特商法プレースホルダ根治/価格単一情報源化=¥15,000リテラル全廃/コードコピー766箇所/モバイル可読性/進捗・続きから/JS無しLP可視/全AAコントラスト/git管理化) ③E2E全通過・リグレッション0 ④教材149本+LPの文体全面改稿(「正直」71→17・オープナー149本全ユニーク・リンク353/フェンス384/図32無傷を機械証明)。commits `5d2b1cb`→`c361c33`(ローカルのみ、push未)。文体正本=`CC-business/REVAMP-2026-07-11/VOICE-RULEBOOK.md`(今後の新規コピーにも適用)。**残=YD作業: 本名→brand.ts / Stripe本番キー+店舗名 / RESEND_FROM / 残席実数表示の判断 / テストURL再デプロイ可否**。詳細=[[2026-07-13_CC-business_全面リバンプと文体改稿完了]] / HANDOVER.md §14
- **★ 教材v2全面改訂 ✅完了 (2026-07-03→07-05、Fable5設計/Sonnet5実装)**: 全86レッスン監査(worth3〜4/10・PII破損9箇所)+Vault発掘7領域 → **全10コース(C0〜C9)・40章・149レッスン・37.1万字**(旧6コース86本7.9万字の4.7倍)に全面改訂。新設=C0準備の学校(インストール/費用全開示)・C7クリエイティブ(vidkit/AE案件/AI音楽/共有サイト)・C8タスク案件管理(Notion84タスク実録)・C9舞台裏(この商品自体の制作記録)。図解32・コピペブロック384。検収=Fable全数、機械スイープ(PII/リンク/異常文字0)、build緑、Playwright実機確認済。本文正本=`content-src/lessons/`(md→bodies.ts生成)。**残=YD判断: ①テストURLへのv2デプロイ ②localhost FM実名化可否 ③外注額の絶対額表記可否 ④~/.claude.jsonのAPIキー平文→ローテーション推奨**。詳細=CC-business/HANDOVER.md §13。
- **状況**: 🟢 **テストモードで本公開済・購入フロー全検証済(2026-06-04) / 本番化はYD入力待ち(正本=CC-business/HANDOVER.md)**。テストURL=https://cc-business.vercel.app (noindex/Protection無効/Stripeテスト鍵)。Playwrightで 購入→決済→/unlock→/learn 全6解放→レッスン本文 まで end-to-end 確認済。SNS(Instagram+Threads)で売る情報商材一式、中身の主役=Claude Code(裏方)。真の目的=YD が情報商材の稼ぎ方を実験的に見てみる。
- **パス**: `/Users/ittou/projects/CC-business`(`web/` が Next.js 16 + Tailwind v4)。**正本引き継ぎ=`CC-business/HANDOVER.md`**
- **構成**: LP(Claude Design 移植の編集デザイン)→ Stripe Checkout(テストモード)→ 署名JWTクッキーで購入者だけ `/learn` 閲覧。教材=6コース25章**86レッスン**(Vault実録から生成)。法務4ページ実装済。SNS素材=Instagramポスター6 + Threads原稿10 + OGP(`marketing/`)。
- **価格(2026-06-03 変更)**: ★ワンプランのみ。お試し¥1,000は廃止。定価¥29,800 / 発売記念¥15,000・先着100名(以降は定価)。先着判定はStripe実購入数で自動(100名で定価復帰)。景表法対策で¥29,800は「以降の定価(将来価格)」表示。購入で全6コース解放(単一ティア)。価格の置き場=`web/src/lib/brand.ts` の `product`。
- **品質の仕組み**: 3人の架空人物(主婦みどり/学生リョウ/元編集者田中)による**自走批評ループ**をコピー(4R)・デザイン(2R)で運用。
- **★ PII方針**: 団体名・大学名・勤務先名は出さない(一般化済)。年齢/身分/吃音/「販売で全国トップ」「260名団体代表」等の基本情報は残す。
- **デザイン**: YD が Claude Design(claude.ai/design)で生成したHTML/CSSバンドルを忠実移植(`app/editorial.css` を `.ed` スコープで隔離)。
- **★ GTM計画確定 (2026-06-03)**: 媒体=Instagram+Threads(役割分担=Threads信頼構築/物語・オーガニック専念、IG認知+リール広告+着地)。広告=少額テスト(トラフィック目的・リール面・¥500×3日=¥1,500単位・A1正体+B1ビフォーアフター同時→探索7日→寄せ7日)。リール=ハイブリッド(faceless基本+要所で声/手元)。運用=半自動(Claude週1一括生成→Meta Business Suite予約→毎日は返信だけ)。進め方=助走7日(ノーセル)→Go-live→本格オファー2週間→100名到達/定価復帰後継続。成果物=`marketing/launch/` 8ファイル(戦略/日割りカレンダー/Threads30本/IGカルーセル/リール絵コンテ8本/広告設計/返信15テンプレ/景表法CLEAN)。詳細=[[2026-06-03_パソコン1台の学校_GTM計画確定]]、索引=`marketing/launch/README.md`。
- **★ 販売=独自サイト継続 + セキュリティ硬化 + テスト本公開 (2026-06-04)**: note=情報商材規約NG, Tips/Brain比較の末「本名公開OK」判断で独自サイト採用(手数料3.6%・先着自動値上げネイティブ)。詳細=[[2026-06-04_CC-business_独自サイト継続_セキュリティ硬化_テスト公開]] / `marketing/PLATFORM-decision.md` / `FUNNEL-design.md`(LINEファネルは棚上げ)。セキュリティ=監査 critical0/high1(restore本人確認=R-1)、Phase A硬化(JWTアルゴ固定/unlock metadata検証/ヘッダ5種/レート制限/cookie統一)→`SECURITY/`宝箱6ファイル+IPA・IBM学習ノート。テスト本公開=cc-business.vercel.app で購入→決済→解放→本文→restore まで実機検証済。**R-1(restore本人確認)=マジックリンク化済**(Resend、`lib/email.ts`+`api/restore/verify`、`RESEND_API_KEY`投入・配信実証済、R-2列挙オラクルも一律応答で解消)。**実弾ペンテスト(JWT偽造/RSC本文窃取/XFFレート制限バイパス/Hostオープンリダイレクト/情報露出/パストラバーサル)=新規脆弱性0**(`SECURITY/PENTEST-2026-06-04.md`)。確定high/medium全解消。**残(YD作業のみ)=①特商法operator実情報(本名+メール)→brand.ts ②Stripe本番キー(sk_live_) ③Resend独自ドメイン検証+RESEND_FROM(restore実顧客配信用・現状本人yitao0907@gmail.comのみ) ④本公開(noindex env削除+本番キー→vercel --prod)**。
- **★ 本番化着手 (2026-06-05)**: YD「順番に全部やる」で開始。確定=独自ドメイン**`pc1school.com`**(Vercel新規取得・whois空き確認済、候補pasokon1dai/pasokon1/pasokondai比較の末選択)/特商法方針=**本名表示+住所電話は「請求あれば開示」**/メール=**`support@pc1school.com`**(特商法operator+Resend RESEND_FROM共用)。Go-live 6ステップ依存順=①[YD]pc1school.com購入 ②[CC]brand.ts実値化(本名待ち・購入を待たず実行可) ③[CC]ドメイン接続+DNS+SITE_URL ④[CC/YD]Resend検証→RESEND_FROM ⑤[YD→CC]Stripe本番有効化→sk_live_(チャット禁止・env直接) ⑥[CC]NOINDEX削除→vercel --prod→再検証。**いまYD待ち=①ドメイン購入/②Stripe有効化申請/③本名**。コード変更なし(決定のみ)。詳細=[[2026-06-05_CC-business_本番化着手_ドメイン特商法決定]] / CC-business/HANDOVER.md §11。
- **★ Go-live前 総合監査 (2026-06-03、`EVAL_prelaunch_audit_2026-06-03.md`)**: 反証検証で確定25件。コア(決済/JWT/dev-unlock本番無効/ゲート)は健全。**blocker3件**=①特商法の運営者情報ダミー(`brand.ts:17-20`)②プライバシー開示請求メールもダミー(同`brand.ts`)③死蔵ファイル(.playwright-mcp/*.yml・docs/mining-result.json)にPII残存+ルート未gitignore=次の`git add .`+pushで履歴混入リスク(※ライブweb/srcはスクラブ済)。should-fix=S-1価格ハードコード(100名到達時に表示¥15,000/決済¥29,800乖離リスク、初日sold=0で一致)/S-2 site-copy.ts旧仕様dead削除/S-3 layout.tsxの廃止価格meta=**修正済**/S-4 lintエラー。
- **次のアクション (YD作業)**: ①SNSアカウント開設(IG/Threads同名)→プロフ文を`00-strategy.md`§4からコピペ ②Phase A 助走投稿開始(`01-calendar.md`、まだ売らない) ③Go-live準備(Stripe本番キー+`AUTH_SECRET`+`NEXT_PUBLIC_SITE_URL`+`ENABLE_DEV_UNLOCK`空+`brand.ts`特商法実情報[blocker①②]+Vercelデプロイ)。残: PII死蔵ファイル掃除(B-3、rm伴う=要GO)、100名到達時の価格出し分け(S-1)。手順=`CC-business/README.md` / `marketing/launch/README.md`。
- **★ note移行へ方針転換 + 合言葉ゲート実装 (2026-07-27決定 / 2026-07-28実装)**: 特商法の本名非公開を優先し、販売をnoteへ移行(B案)。上の2026-06-04「独自サイト継続・本名公開OK」判断を**覆した**。教材本体は自サイトに残し、noteは「レジと看板」に徹する。設計=`CC-business/note-migration/migration-design.md`(方式=(c)ローテ付き共通パスワード)、残件=同`REMAINING.md`。**CC実装済(B-1)**=`/access`+`/api/access`(合言葉→既存signAccessでJWT発行、`ACCESS_PASSPHRASES`カンマ区切りで世代ローテ、SHA-256定数時間比較、8回/10分)、`brand.ts`に販売フェーズ`phase`追加で「Stripeキー無し=恒久¥15,000表示」事故を封鎖。branch `note-migration` にローカルコミットのみ(push/deployなし)。**残ブロッカー(YD作業)**=①B-2 屋号メール窓口の実在化(特商法の氏名省略要件) ②B-3 `whois pc1school.com`で本名が出ないか確認(最優先・2分) ③合言葉決定→`vercel env`投入 ④note記事公開。**残CC作業**=設計書§2のCTA差し替え・Stripe/restore導線撤去・法務4ページ改訂。→ **2026-08-05に下記のnoteオンリー転換で上書きされ廃止**
- **★★ noteオンリー分売へ再転換 (2026-08-05 YD決定)**: サイト完全廃止、note完結で分売。B-1合言葉ゲートは死にコード化。①リサーチ3本 (競合16本実測/売れ筋45本API精読/公式30万件分析+規約2026-05-26逐語) → `CC-business/note-publish/research/` ②統合プレイブック=`note-publish/PLAYBOOK.md` (正本) ③**商品構成確定 (YD承認済)**: C0無料+有料6本 (C1=¥1,480→1,980 / C2, C3+C8, C5+C6, C7, C4+C9=各¥2,980) +全部入りマガジン¥9,800→完結時12,800、週1順次公開 ④変換パイプライン (`scripts/build-drafts.mjs` 表→箇条書き/SVG→PNG32枚/内部リンク置換) で全10コース本文ドラフト済 ⑤C0+記事1の完成原稿=`articles/00-c0-free.md`+`01-c1.md` (YD検収待ち)。**規約上の絶対NG**: 売上・部数の公開煽り (10.1(3)) / 自演購入 (13.1(9)=没収+違約金、別アカも同一管理者判定で該当→テスト購入は実在の第三者に依頼)。**⑥全7記事の完成原稿完了 (2026-08-05深夜、YD承認済み)**: `articles/00〜06` (計35.4万字)。C9「noteはやめとけ」矛盾は c9-m1-l3 に「結論は二度ひっくり返った」追記+手数料実測値修正で解消。パイプラインは記事単位 (合体記事の章連番化・記事間リンク解決)。**残**: 図解32枚の挿入位置つき投稿手順書 / 要レビュー表30個の画像化判断 / noteアカウント開設・プロフィール文 (YD作業、2FA・プレミアム月500円推奨) / E2E購入確認は実在の第三者に依頼 (自演購入は規約13.1(9)で厳禁)。commits `2b625dc`〜`21aa414` (branch note-migration、push未)
- **⑦脱AI臭・全面書き換え完了 (2026-08-07)**: YD指示「AIぽさ完全排除・構成も・リサーチから監視まで」→ リサーチ2本→企画書(REWRITE-PLAN)→機械層根治(変換器バグ/LMS座標廃止)→パーツ層書き分け→articles/凍結→7エージェント並列書き換え→**敵対的リーダー2系統の検収** (記事3不合格→R2で全解消、事実破綻・三人称混入・二重表記まで摘出) →全記事最終合格。ブランド=「深夜2時｜パソコン1台の学校」(ID: pc1school、夜仕様トプ画/ヘッダー、バイブコーダーペルソナ=属性を名乗らない)。詳細=[[2026-08-07_CC-business_脱AI臭全面書き換え]]。**残: YDのnoteアカウント開設のみ (原稿・画像54枚・手順書・BIO全部準備済)**。commits 〜`0a69dcd`
- **関連**: [[2026-06-03_パソコン1台の学校_情報商材システム構築]]、[[claude_design]]、`CC-business/README.md`

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

### localhost FM (コーディングBGM YouTubeチャンネル、2026-06-05 第1弾完成)
- **★★ 2026-07-28 深夜第2セッション: ep-02・ep-03 公開完了 (チャンネル3本体制)**: ①ep-02 4K版公開 https://youtu.be/bRxVwqLaOI8 (旧1080pドラフト削除→新規アップ、著作権チェック問題なし) ②ep-03をSuno回収残2曲→ミックス34:04→4K映像 (ヒーロー語「ship it」)→サムネ→メタデータ→公開 https://youtu.be/lMTL7f5ZuyY まで一気通貫 ③**300MB動画の完全自動アップロード方式を確立** (チャンク注入、YD許可で `javascript_tool` 許可ルール追加、手順=HANDOVER正本) ④Sunoクレジット2,330残 (今夜消費0)。残=YD任意: YouTube Data API審査 / Midnight Syntax 2曲の扱い / チャンネル2段階認証
- **★ 2026-07-28 深夜自走 (新CC)**: ①ep-01は実は**6/5に公開済みだった** ②ep-02完成=4K版 `episodes/ep-02/out/localhost-fm-mix-02-4k.mp4`(34:08、Claude Designエディトリアル映像+コーラルカーソル点滅、YD承認の新ブランド路線)、Studioに旧1080pドラフトあり→削除して4K版アップが最初のタスク ③ep-03素材8/13回収済 ④自動化スイート+スキル`localhost-fm-episode`導入 ⑤Sunoクレジット2,330残。**正本=`~/AI projects/youtube bgm/HANDOVER.md`冒頭** (※リポジトリは projects/ でなく AI projects/ 配下)
- **状況**: 🟢 **第1弾動画 完成・公開直前**(YDアップロード待ち)。@productivityonyt 系「コーディングが捗るBGM」チャンネル。名前=localhost FM。
- **パス**: `/Users/ittou/projects/youtube bgm`(正本=`HANDOVER.md`、公開メタ=`PUBLISH.md`)
- **第1弾**: `localhost-fm-mix-01.mp4`(1920×1080 / 53.39分 / 13曲ミックス)。音源=Suno(Pro・WAV)、-14 LUFS正規化、6秒クロスフェード、右下に点滅 `localhost█` 焼込。背景=深夜デスク(var2-lockin)。検証=統合-14.0 LUFS / TP-1.4dBFS / クリップ無し。
- **制作方式**: 1曲ループでなく短い曲13本を連結。並び順は **BPM/エネルギー/明るさの実データ**で決定したエナジーアーク(静→123bpmフロー→143.6bpmピーク[ハイプ最高潮]→Terminal Drift着地)。`-2`版は全ペア非隣接。
- **★ プロンプト量産ライブラリ確立 (2026-06-06)**: 同曲問題(1プロンプト=似た曲しか出ない)の解として、別BPM/キー/楽器の Style を **57本**在庫化。`suno-prompts.md`(A〜E、E章=amapiano)+`suno-prompt-library.md`(F〜K 50本=afro-latin/dnb/citypop/uk-garage/afro拡張/melodic-club)+差し替えパーツ表。Burna Boy "Don't Let Me Drown"(映画F1/amapiano)起点。Suno知見=StyleInfluence(ジャンルの枠)/Weirdness(2曲を違える)/タグ法で展開/extended mixで3〜4分/2曲は片方Trash。詳細=[[localhost_fm]]。
- **パイプライン(`scripts/`)**: analyze-tracks.py→normalize-order.py→make-mix.sh→gen-chapters.py→render-final.sh(再生成コマンドは PUBLISH.md 末尾)。
- **ブランド素材(`assets/`)**: icon-final(暗闇ネオン)/ banner-lockups(4ロックアップ横並び)/ thumb-desk-ship(サムネ)/ brand-kit.html。
- **次のアクション(YD)**: YouTubeアップロード(カテゴリ=音楽・著作権チェック必須・自曲はContent ID登録しない)→ 公開後URLで02本目構成 or 9:16 Shorts抜粋。
- **関連**: [[../decisions/2026-06-05_localhost-FM始動]]、[[../knowledge/programming/tools/localhost_fm]]、`youtube bgm/HANDOVER.md`

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
- **★ 2026-08-12 撮影素材フル復元 (MacBook Neo移行後)**: Google Drive「平成たち祭り」→ `/Volumes/Yitao HDD/平成たち祭り/` へrcloneで559.553 GiB/226ファイル (Sony/100CANON/Insta360X3/CLIP/100GOPRO) を完全DL。バイト数・オブジェクト数がDrive側と完全一致、転送エラー0、全ファイルMD5検証済み。※旧Macロストで消えたのは素材ではなく `_cc_work2/` 作業資材とResolveタイムライン (shotlist等の設計はVault知見ファイルに現存、再構築可)
- **★ 2026-08-05 時系列整列版TL 2本追加 (YD指摘「クリップの時系列がバラバラ」対応)**: メイキング+ダイジェストv3を、章構成・カット選定・in/outはそのままに**章内のカット順だけ実撮影時刻順に整列** (ffprobe creation_time+inオフセット基準)。新TL=**04_MAKING_chrono** (34カット/227.45s) + **04_2min_v3_chrono** (57カット/123.83s)、既存4TL無傷を機械検証済み。補正3点: ①GoPro3本はカメラ時計未設定(04/03表示)のため現行位置ピン ②making OP#1のフラッシュフォワードは演出として先頭維持 ③最終カット(掛け声/さくら木満開)は末尾ピン。発見: v3「2章開場」のC108/C111は実は終演後21時台の撮影。字幕/BGM工程の正= `_cc_work2/chrono/shotlist_making_chrono.md` / `shotlist_v3_chrono.md`、時刻根拠= `chrono/chrono_report.md`。注意: 初回mtg A013/A014の2カメ時計差は未検証(1限目の順序が入れ替わった根拠)
- **★ 2026-07-29 メイキング 企画〜カット編集完了 (Fable監督版)**: 構成=「時間割 / 言ったことが、ぜんぶ本当になっていく」(会議で企画誕生→公園で図工→当日実現→終演後の声、掛け声「ヘイ!セイ!たち!サーイ!」が縦糸)。**34カット 3:47、TL「03_MAKING」を take2 にビルド済み** (60fps・マーカー: 緑=幕頭8/紫=縦素材2/青=最終カット)。全カットFable本人が2fpsタイル目視、ダイジェストv1/v2の94カットと重複0。会話22カットはwhisper引用付き=字幕工程に直結。構成宣言=`_cc_work2/making/構成案_final.md`、shotlist=`_cc_work2/making/shotlist_making.md`、**報告=`CC_報告_making_2026-07-29.md` (YD Top3: 通し再生 / C067のブレ判断 / 縦素材2カットの処理)**。BGM・字幕・カラーは次工程
- **★ 2026-07-29 ダイジェストv2完全リビルド (目視必須版) 完了**: 対象170本を全数目視 (Fable系統12エージェント、selects.csv 170行・重複0)→2fps精査74窓→librosaビート/whisper解析→**shotlist 49カット118.95秒**→Resolve「**平成たち祭_REPLAYGROUND_take2**」ビルド済み (4K/60fps/ビン5系統検証済/Comments全170書込/Retimeマーカー3/候補TL2本)。v1(音声のみ選定)の資材は `_archive_v1_音声のみ版_20260728/`。**Haruhiソロ=C081/C084で成立 (本人確認はYD)**・さくら木ビフォー(C101)/アフター(2731)確保・観客の涙は170本に存在せず代替採用。**YD次アクション=CC_報告_take2_2026-07-29.md のTop3** (playbackFrameRate確認/Retime50%×3/候補TL判断2件)。作業資材=`_cc_work2/`
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

### ★ Yitao Film｜案件管理ダッシュボード (Notion、2026-07-09 新アカウント再構築完了) ★
- **状況**: 🟢 **Notion新アカウント (yitao-ding@ooo-studio.jp、workspace「DingYitaoさんのスペース」) に全面再構築完了 (2026-07-09)**。クライアント8・案件10・タスク84件、エラー0・実機fetch検証済。ホームは見やすさ優先で刷新: 進行中案件board / 直近タスクlist / 納品カレンダー / アーカイブ分離、3DBベタ置き廃止。旧アカウント (toyo.jp) 側の残骸ページはYDが手動削除予定。
- **ホーム**: https://app.notion.com/p/398d8d9d19c281c59d71d986e0a6b63a
- **★ 2026-07-14 B案移植+ページ内タスクビュー 完了 (YD全要望実装済み)**: YD採択のB案を正規ホームへ移植 (callout→3DB本体→要確認トグル→🎬パイプラインboard→📅タスク締切カレンダー→✅タスク全体[締切順table+工程別board]→💰お金→👤クライアント→📦アーカイブ→🧩テンプレ、verifier=Sonnet全ビュー実測パス)。**全10案件ページ末尾に「✅ このPJのタスク」ビュー設置** — 行追加=タスク作成・案件リレーション自動・締切/工程/メモ直接編集可、タスクはホームのカレンダー/横断ビューに即反映。**制約発覚: Notion MCP の view DSL は relation 値フィルタ非対応** → ビュー本体はMCP、フィルタ10個は Chrome (claude-in-chrome) でUI操作代行 (各ページ件数 5/8/7/13/9/6/8/12/8/8 =期待値と全件一致検証済)。試作3案ページ (399d8d9d…d52a) は役目済み=YD確認後に削除可 (配下に退避した空ページYF-11/12あり、一緒にゴミ箱へ)。**残: ①ステータス鮮度の実態確認 (YF-1/3/6 が納品日超過のままレビュー表示) ②完了タスクをPJ内ビューから隠すか (要望あればフィルタ追加はMCP一括で可)**。知見= knowledge/programming/tools/notion_mcp_dashboard.md
- **投入内容** (6/24時点データ + 進捗反映):
  - 動画5: ①ダンスでチャリティーWS(千葉)編集[納6/26] ②長野インタビュー編集[**✅2026-07-15 v3編集完成・YD検収待ち**: 本編「91匹の家」1本9:40 4K (全字幕方式・Bロール18本・DaVinci API自動構築)+手ミックス用editタイムライン、成果物=`2025-07-06_Nagano/render/honpen_4K_v1.mp4`、詳細=[[2026-07-14_長野ドキュメンタリー_全字幕方式完成]]] ③**ハイミ「蛹」**MV[納6/29] ④平成たち祭メイキング&ダイジェスト[納7/12] ⑤JAMBOREE STUDIO BATTLE Lite OP『THE EYE / 開眼』[📦納品済7/5・¥70,000]
  - 写真5: ⑥SANA[納6/25] ⑦aキリア写真/⑦bキリア動画[撮影6/29] ⑧ナナ[🎬制作中・セレクト済209枚・クライアント選定待ち] ⑨のあみ
- **★ 要確認(YD回答待ち、5点・ホーム冒頭トグルにも掲載)**: ①案件2納品日 ②案件7写真/動画の統合可否+納品日 ③写真5人目氏名 ④写真勢クライアント区分(暫定アーティスト) ⑤キリア打ち合わせ内容(記入欄だけ作成済)
- **運用**: 新規案件は Claude に「種別・案件名・クライアント・納品日・撮影有無」を伝えれば本文テンプレ+標準タスクごと自動作成。案件IDは YF-1〜10 (サンプルデータなし、クリーン)。本案件群は Yitao Film(Arte Grow / やることDB は対象外)。名称の正式表記: **ダンスでチャリティー**(ダンちゃり)・**平成たち祭**。
- **関連**: [[2026-07-09_Notion新アカウント移行_ダッシュボード再構築]]、[[2026-06-24_Yitao_Film案件Notion投入]]、`knowledge/filmmaking/yitao-film-notion-handoff.md` (正本、ID更新済)

### ★ JAMBOREE STUDIO BATTLE Lite OP動画 (大型イベントOP、¥70,000) ★ ✅✅完成・R13提出済み・YD終了宣言
- **★★ 状況 (2026-07-06): ✅✅✅ R13完成・YD「本プロジェクト終了」宣言・終了処理完了 ★★**。**最新提出物=`書き出し/LiteOP_v2.mp4`**(YDリネーム後、4K/AAC320k/faststart/116s、審査員紹介順=SHIKI→憧→TOMOKA→MANA→HAL→KIDO→HISAYO→MAA)。R12版=`書き出し/LiteOP.mp4`(旧順・比較用)。R13=クライアントFBで紹介順のみ変更(JUDGES配列=スロット時刻固定・属性が移動→音声完全無変更/カードは旧並び維持/L/R偶奇反転)。QA0件+監督25フレーム検分合格。**終了処理**: 書き出しスリム化(4.0G→976M、中間41件ゴミ箱)+Vault/HANDOVER同期。⚠️「旧提出物消失」は誤認=YDリネーム(byte一致確認)。⚠️YDのAME自主書き出し(~/Downloads/THE_EYE_master/)は音声BGMのみ=VO/SFX欠落。詳細=HANDOVER§0a-12。**→ 次セッションでVault archiveセクションへ移動可**。
- **反復ログ**: R10(字幕半減/B3をANSWERED式統一/ARE YOU READY再設計/S4声低く再生成/審査員全員p99≤240/SFX9発)→R11(AYRゴースト根治・ITS NAME IS 3s前倒し・S4「Jamboree」のみ化・VO1.4倍)→R12(VO1.5倍・ラストJP字幕削除で英語のみ・EN題字ど真ん中・ITS NAME—削除)→提出(L7削除・QAラベル除去)。体制=Fable5監督(全フレーム直接検分)+Sonnet5実装便1本。**正本引き継ぎ=`HANDOVER_v2_THE_EYE.md`(§0a-11=提出版、§3=罠13項)**。ミックス正本=`mix_final.sh`。
- **★ v2 BGM確定 (2026-07-03)**: 🎵 `music/LiteOP_BGM_FINAL.wav`(ElevenLabs「Neon Colosseum」116.000s無加工)YD採択。TC正=`music/final_tc_lock.json`(開眼スラム=f2886/96.200s)。詳細=[[2026-07-03_LiteOP_BGM_AI生成路線確定]]。
- v2初期経緯: 8並列ディスカバリー→5案→4レンズ審査で「THE EYE」優勝。スタジオ42/11都県。審査員1人目の表記は**「憧」**(2026-07-04 YD変更、旧KEN)。
- **残TODO**: **R10(FB6項目、HANDOVER§1)** / D3残(CAMERA_SHAKE移植・まばたき磨き) / D5本ミックス(VOダッキング+SFX)→D6書き出し(QAラベル除去+720p版) / 素材課題2件YD待ち(p7_stars高解像度ロゴ・p10_movement完全版ロゴ)。
- **状況 (2026-07-02 夜)**: 🟢 **v8 = ロゴ抜き直し+全編品質スイープ+YD実機FB反映 完了**。①ロゴをpdftocairo正攻法で透過抜き直し(colorkeyの欠け根治) ②全編スイープ: 200フレーム→9視点並列レビュー52指摘→Sonnet実測検証→13件修正・4件実証棄却 ③YD iPhone実機FB反映: MC/審査員の顔の白飛び解消・チームロゴチップ1.5倍可読化・B8/B9/B10のFX増量・**B11再構築(巨大だるま降臨→震え縮小→地響きドーン→開眼で瞳が入る)**。体制=Fable5設計/Sonnet5実装・検証。**最新成果物=`書き出し/全体爆上げ_0-90s_v8_YDFB反映.mp4`(720p版YD送付済み・レビュー待ち)**。
- **★ 引き継ぎ書(唯一の真実)**: `/Users/ittou/整理済み/Lite_イベントOP_2024-2026/HANDOVER_映像制作.md` — AE自動制御のハマり所全部+FXライブラリAPI+実装結果と差分(§7b)+B2マップ/ロゴ仕様(§7d)+**スマホリモート操作手順(§10)**。新セッションはこれを読めば続行可。
- **リモート操作**: 新セッションは `start_remote_session.sh`(tmux "liteop")で起動→スマホSSH(Termius)から attach。YD側未完: リモートログインON / Tailscale導入 / Macスリープ無効(§10のチェックリスト)。
- **残りTODO(§7c)**: YDレビューFB反映 → 憲=KEN表記確認 → BGM確定後の音同期 → 最終書き出し。
- **並走**: BGM は別CCセッション担当(`BGM作業指示書_新CC用.md`)。music-first で確定後に映像側の秒を合わせる。
- **方向性(YD明言)**: 派手派手・ガンガン・コテコテOK・遠慮禁止。多色OK(2026-07-02指示)だが緑#13A89B=背骨は死守・和風完全排除・FIFA×パイレーツ。5000人会場のOP、成功すれば次に繋がる大型案件。

## 🟢 アクティブ・優先度中

### ★ PCデータ整理 規格v3 (Notion連携、2026-07-11 Phase A完了) ★
- **状況**: 🟢 **規格v3確定 + Phase A完了・検収済み** (Fable計画/Sonnet実装)。規格= 3クリック到達 / 1案件1フォルダ完結(アプリDB例外) / 先頭記号禁止 / ALL WORKs集約 / フォルダ名合体で階層抑制 / ~/素材は例外。記号接頭辞6件撤去、あやか_2026-05・つんた_2026-07収容、Premiere→/Applications。**正本= `~/ALL WORKs/MAP.md`** (Notion YF-1〜10対応表入り)、ジャーナル= `~/MediaCatalog/reorg_phaseA_2026-07-11.tsv`
- 4フォルダ (俺のリール128G/ドローンアーカイブ/ばば会社/_予備_HiMe) はYDが**Extreme pro**へ手動移動済み → マウント時に実在検証+リネーム+MAP_両ドライブ再生成
- **次**: ① Phase C = 平成たち祭 (YF-4) 納品7/12完了後に450G規格化+SANA移設+FCPライブラリ同居+Pictures振り分け ② 対話仕分け167G (YDと一緒に) ③ ゴミ候補削除GO ④ Notion MCPは `mcp__claude_ai_Notion__*` で接続済み (案件DB= collection://c8b35e29-...)
- **関連**: [[2026-07-11_データ整理規格v3_PhaseA完了]]

### ★ LightRig (スタジオ照明セッティング記録アプリ、2026-07-11 v1→同日v1.1) ★
- **状況**: ✅ **v1.1完成・YD iPhone実機で使用中** (実機フィードバック7点を同日反映済)。スタジオで組んだ照明 (機材/位置/出力%/色温度K/ジェルsRGB/モディファイア) を3Dで記録し、撮影写真と紐付けて見返す個人アプリ。iPhone/iPad/Mac 対応、iCloud (CloudKit) 自動同期。
- **★ v1.1 (2026-07-11 実機FB対応)**: 1タップ=選択 / **2タップ=インスペクタ** / ドラッグは選択不要で直接移動。首振り自由化 (aimAtSubject 既定OFF、壁当て可)。**レフ板** (StudioProp: 白/黒/カスタム色・サイズ自由・ドラッグ可)。インスペクタ刷新 (セクション+大型値表示+全体ダーク)。背景紙を壁全幅化。部屋プリセット (小/標準/大)。視点プリセットボタン (正面/45°/俯瞰/リセット)。**カメラ半球ドーム化** (注視点=被写体固定・床が下限・2本指パン廃止、YD発案)。スキーマ変更は追加のみで既存データ/CloudKit互換。
- **パス**: `~/AI projects/Lighting` (正本=`HANDOVER.md` §0a、設計=`DESIGN.md` §8=v1.1契約)。git ローカルのみ (リモート未設定・push未)
- **スタック**: SwiftUI + SceneKit + SwiftData/CloudKit + XcodeGen。機材カタログ49機種 (Profoto/Godox/Aputure/Amaran/Nanlite/Generic、`fixtures.json` に1行追加で増やせる)。チームID=FC2V887B8C (証明書OU由来、[[claude_mistakes]] B-7)
- **品質**: iOS/macOS ビルド green・テスト25件 pass。v1.1 も iPhone 17 Pro シミュレータ実描画で Fable 検分済 (レフ板/壁全幅/ダークUI/視点ボタン/新インスペクタ)
- **残TODO (YD)**: ① 実機に ▶ で v1.1 反映 (iPhone/iPad/Mac 各1回) ② **2台間で写真付きセッティングの iCloud 同期を実機確認** (externalStorage→CKAsset 同期は Apple 公式に明記なし、唯一の未実証点。NGなら同期方式を組み替える) ③ 操作感 (ドラッグ感度・プリセット角度) の微調整FBあれば随時 ④ 年1回 Xcode から再 Run (証明書 約1年)
- **関連**: [[lightrig]] (knowledge/programming/tools/)

### ★ easy-share (社内 撮影素材 共有・プレビューツール、2026-05-31 着手) ★
- **★ 2026-07-11 Fable司令塔で0→100全面見直し完了**: YD指示で Fable=司令塔(計画/監視/検分)・実装は Opus/Sonnetサブエージェント委譲の体制。7視点全域監査→統合バックログ**64件**(P0=2/P1=10/P2=29/P3=23、反証検証で棄却0)→Wave1-4で修正実装(main へ8コミット、各Waveで敵対的レビュー)→**本番は Wave3 まで反映済み**(Wave4リファクタは未デプロイ)。あわせて launchd watcher 復旧(プロジェクトディレクトリ移動でplistパスがずれ7,547回クラッシュループ→取り込み1ヶ月停止していたのを修正)、R2孤児ファイル1.18GiB掃除(総量2.70GiB化)、Vercel env(NEXT_PUBLIC_ASSET_BASE)をPreview/Developmentにも追加。詳細・全記録は正本 `HANDOVER.md` と `docs/overhaul-2026-07-11/`。
- **★ 公開URL(稼働中・正式運用)**: **https://easyshare-fx.vercel.app**(Vercel 本番、Deployment Protection 無効=認証なし公開、YD 認可済「セキュリティ不要・公開OK」)。本物の FX30 実素材(写真77枚、flat+4ルックWebGL切替、写真/動画分離、mid派生完備)がライブ表示。
- **★ ストレージ = Cloudflare R2(本番稼働)**。バケット `easy-share-media`、公開ベース `https://pub-6cfa3ffdc3ec456790e058cce335c70d.r2.dev`。Vercel env `NEXT_PUBLIC_ASSET_BASE` が R2 を指す(本番+Preview+Development)。10GB無料/egress永久無料/3TB$45月、現在総量2.70GiB。認証は `~/.config/rclone/rclone.conf [r2]` と gitignore config のみ(Vault非保存)。CORSキャッシュ罠は `VideoView` の poster属性(直接付与しない、`<img>`オーバーレイ方式)で解決済。
- **★ B案(クライアントサイドLUT)**: 動画は flat 1本だけエンコード、4ルックは**ブラウザ WebGL2 で .cube LUT をリアルタイム適用**(`VideoView.tsx`、WebGL資源解放+LUTキャッシュ対応済)。LUT は R2 `luts/<id>.cube`、`process.sh` のR2経路でlut自動アップ。
- **★ 「Google Drive 風」全自動・常駐運用 稼働**: **`~/EasyShareDrop/`** に放り込む → `watch.sh`(**launchd `com.easyshare.watch` 常駐**、ログは `~/Library/Logs/easyshare.watch.log`)→ `autosort.sh`(撮影日で自動振り分け、重複は `_duplicates/` 温存)→ `process.sh`(UPLOAD=1/STORAGE=r2、mid派生生成、失敗時manifest非更新で堅牢化済み)で色変換+R2自動アップ→公開URL反映。サブフォルダ名=プロジェクト名。
- **状況**: 🟢 **本番稼働中(Wave3相当)**。iOS DL 灰色四角バグ(2026-06-17)修復済み。**正本=`~/AI projects/easy-share/HANDOVER.md`**。
- **用途**: ダンス×クリエイティブチーム 4 名(全員 Mac+iPhone)が、FX30 S-Log3 動画 + ARW RAW を**その日のうちに色を揃えて軽くプレビュー・共有**。素材は**プロジェクト単位**で振り分け(①/②…)。商用配布ではない社内ツール。
- **確定スタック**: 取り込み = Mac ネイティブ(sips + ffmpeg hevc_videotoolbox + rclone + fswatch) / ストレージ = Cloudflare R2(egress0) / 認証なし(YD認可) / 閲覧 = Next.js 16 PWA on Vercel(Spotify 風ダーク + Finder風レイアウト、アクセントはコバルトブルー)
- **色の核心(検証済 partial)**: 写真は ColorSync で堅牢一致 / 動画は 709 タグ必須(未タグ=Safari が BT.601 誤解の最悪パターン)/ **S-Log3 は必ず Rec.709 LUT を焼く**(タグ不能)/ True Tone・自動輝度・Night Shift は全員オフ運用 / 最終色判断は NLE で(ツールはレビューまで)
- **graded =「ルック切替式」**(YD 提供 FX3 4 ルック Film Tone/Camp Moody/Blue Snow/Pure Night)。`ingest/luts/*.cube` を各ルックとして自動列挙、ビュアーで flat ⇄ 各ルックをトグル切替。proxy はルック分生成(flat+4=5本/クリップ、videotoolbox で軽い)。.cube 出し入れで増減
- **実機で潰したバグ**: hevc_videotoolbox の色タグ未書き込み → `setparams` 焼き込み / Tailwind v4 `@theme inline` で色未生成 → 非 inline + 再起動 / Tailwind v4 トークン名衝突(`--color-base`が`.text-base`を潰す)→`canvas`に改名
- **残タスク(次セッション、優先度順、詳細はHANDOVER.md)**:
  - [ ] 最終 0→100 再監査(7/11はセッション上限で未実施)
  - [ ] Wave4 のデプロイ(プレビュー→YD確認→本番+alias)
  - [ ] ルートdocs(README/SETUP/DESIGN)の実態同期
  - [ ] git履歴 squash → push 可否確認(旧コミットにR2アカウントID残存)
  - [ ] F3: 動画原本バックアップ(Time Machine/外付けSSD)の手順明文化
  - [ ] F6: easyshare-fx を Vercel Project Domain化(手動alias運用を解消)
  - [ ] Vercel Blob ストア本体の削除(データはR2移行済み)
  - [ ] 非ブロッキング残(stem衝突/StatusBar端数表示/横断グループ重複表示/グリッドsrcset/mid再試行なし 等)
- **パス**: `~/AI projects/easy-share/`(HANDOVER.md=正本 / DESIGN.md / ingest/ / web/src/components/browser/ / docs/overhaul-2026-07-11/=2026-07-11監査・Wave記録)
- **関連**: [[2026-05-31_easy-share設計確定]]、[[2026-06-02_easy-share_FinderUI刷新と写真原本DL]]、リサーチ workflow `wafcglinq`

### ★ Project Agent Application (Z 世代向け青春タスクアプリ、2026-05-23 着手) ★
- **★★★ 2026-08-24 キャラ再設計Round2完成 + push完了 ★★★**: Round1 (小鳥コーラル化/丸型/中間の3案) はYD「全部えぐいぐらいダサい」で却下。敗因=既存の鳥に引っ張られた。Round2は文法を厳格化して再指示: 横長の丸い塊・ベタ塗り単色・縦長の点目・口なし・短い足スタブ3〜4本・付属要素 (冠羽/翼/嘴/ほっぺ) 全面禁止。**6体 (2a標準比脚3 / 2b丸め脚4 / 2c丸め+耳突起脚3 / 2d ローワイド脚4 / 2e縦丸+耳突起脚4 / 2f ワイド+低い耳脚3) 完成、YD選択待ち**。Design タブ「マスコット再設計 方向出し」で確認可。選択後→第3弾でmood6種+サイズ検証→商標チェック→確定の流れ。**push完了** (gh auth loginをChrome自動化で突破、overhaul/2026-07を origin へ計21コミット push済み)。**YD判断待ち残: ハル吹き出し語調 / ハル体色 / キャラ案2a-2f選択**
- **★★★ 2026-08-23 GOバッチ適用 + キャラ再設計始動 ★★★**: YD GO (旧判断待ち1・2・5) を全適用。①**migration 023+024 実DB適用済み** (適用前後を実測検証、advisors ERROR 0。**実DBがpause状態と発覚→Management APIでrestore。無料枠は無活動~7日で再pause、実モード公開前に有料化orキープアライブ判断**。022は意図的に据え置き=ホーム長押し完了の直UPDATE経路のため) ②**Edge 12関数deploy済み** (旧語調13文言をmessages.ts正本と同期 `4b9d7ee`) ③仕様判断2件実装: 団体作成の学校情報選択 (tms.school_id+create_tm 7引数化+トグルUI `a27fa89`) / オンボ4を授業・課題軸コピーへ (`f0eb6c1`、planner向け提案書 design/school-academic-features-PROPOSAL.md 付き) ④**キャラ再設計始動**: YD確定=コーラル系・名前から再考 (「ハル」は仮のまま)・今回はキャラ本体確定まで。ブリーフ= `design/character-redesign-2026-08/`。ゲート tsc0/eslint0/jest34。引き継ぎ正本= HANDOVER.md 冒頭「2026-08-23」
- **★★★ 2026-08-09〜11 白基調の実装 + YDレビュー反映 + TestFlight build 3 提出 ★★★**: ①白基調基盤実装 (`49cc2d5`: tokens反転でクリーム完全撤廃・keyCard文法・不可視化24箇所修正、alias構造で164箇所自動追従) ②YD が Claude Design 画面集1〜3をレビュー → **★横断ルール確定「青帯はメイン画面だけ、サブ画面は白メイン+普通のバー、灰色の枠組みは全ページで要不要を再検証 (LINE風)」** → 3バッチ実装 (`22e69f4` SubScreenBar+通知メタ行+フルブリード+語調 / `7dc01ef` 通知文言25文 / `c23e1a9` 4-6相当14画面監査適用)。レビュー正本= `design/redesign-2026-08-White/YD-REVIEW-2026-08-09.md`。**画面集4〜6のYDレビュー継続中** ③App Store風プロモスクショ6枚 (1290×2796) をClaude Designで生成→Drive「Project Agent マーケスクショ 2026-08」+ ~/Downloads/PAA-shots ④**TestFlightデモbuild 3 提出完了** (白基調全部入り。EASのpod install失敗=AppCheckCore新依存RecaptchaInteropをexpo-build-properties extraPodsで根治 `d0eaa19`。ios/はgitignore=リモートprebuildなのでネイティブ変更はconfig plugin経由が正)。全ゲートPass (tsc0/eslint0/jest34)。**YD判断待ち**: migration023 GO/022扱い/Edge deploy GO(+旧語調文言4本の同期)/仕様2件(オンボ4学校メリット・団体作成の学校情報)/ハル吹き出し語調/push(未push16コミット)。引き継ぎ正本= HANDOVER.md 冒頭「2026-08-09〜11」
- **★★ 2026-08-08〜09 白基調リデザイン: 全41画面のデザイン確定 (実装はこれから) ★★**: YD「フロント/全ページをデザイン面から作り直す」+「背景を白に統一」→ claude.ai/design (**Fable 5 / Effort Max**) で全41画面ぶん生成完了。**視覚の正 = https://claude.ai/design/p/2235e024-afb8-4600-80dc-afc6fd6dca0c** (プロトタイプ遷移あり5画面+状態Tweaks / 探索3案比較 / 画面集1〜6)。**確定文法 = 1b×1c統合** (YDが3案から選択): 地は白#FFFFFF・沈み面#F5F6F8で**クリーム#FFF8DC完全撤廃**、青#025291の帯が「現在地」と「まず何をするか」を宣言、**押せるものだけが沈んだ面から1段浮いた白いキー**、橙#FF8B07は主アクションと締切のみ。ダーク例外は振り返り一覧/年次Wrapped/お宝箱撮影の3つだけ(ダイジェスト詳細は白)。**YD指示で確定した仕様**: ①タスク詳細=概要→提出→コメント→担当/チームの順、期限は締切チップ1本に統合、提出欄を主役化し「完了にする」を内包 ②ホーム見出しは「今日やること」のみ(件数は小メタへ) ③**コピーの語調=大学生・社会人向けの普通の日本語**(「小学生に対する言葉遣い」はNG、幼い語尾・絵文字・演出過剰を排除、ハルも対等トーン)。**次=実装(未着手)**: tokens.ts反転で164箇所が自動追従するが、**トークン反転と「カードの型」適用は同一コミット必須**(分割すると地もカードも白で階層崩壊)。白地で不可視化するカード=パターンB(影のみ16箇所)+C(枠線も影もなし8箇所)。正本= `design/redesign-2026-08-White/{BRIEF,MIGRATION-SURVEY,DESIGN-OUTPUT}.md`。commit `e3cd990` `e8cabec`。詳細 [[2026-08-08_PAA_白基調リデザイン確定]]
- **★★ 2026-08-08 セキュリティ総合再監査 (Edge/DB/クライアント/設定 の4系統独立監査) ★★**: YD「バックグラウンドとセキュリティ周りを総合チェック、抜け漏れがあれば修正」→ **60+所見**。過去2回の監査で「修正済み」とされた項目も自己申告を信じず全件再検証したのが奏功。**P1**: ①SSRF ブロックリストが IPv4-mapped IPv6 (`[::ffff:10.0.0.5]`) で回避可能 (修正後バイパス27種を node で実行検証) ②`reminders-leader-nudge` が assigneeIds を実アサイニーと照合せず「被害者本人のキャラ名義」で任意ユーザーへ Push 送信可能 ③招待の二重消化レース ④PKCE 運用なのに implicit 経路が残りセッション注入可能 ⑤**Play サービスアカウント鍵が .gitignore に無く、GitHub リモート実在のため鍵作成の瞬間に公開される地雷**。**★最重要: migration 022 は適用保留** — 「呼び出し元ゼロの死にコード」という 022 の前提は 2026-07-13 時点のもので、その後 Wave 2 でホーム長押し完了に配線済み。単独適用すると実モードのタスク完了が全ユーザーで RLS 拒否 (DB監査とクライアント監査が独立に同一結論)。commit `22d684a` (43ファイル +1565/-240、tsc0/eslint0/jest34/シミュレータでMOCKデモ保全確認)。**YD判断待ち**: ①migration 023 の実DB適用GO ②022 の扱い (完了経路を submissions INSERT に付け替えるか) ③Edge deploy GO ④公開ポートフォリオの anon 全件列挙の是非。詳細 [[2026-08-08_PAA_セキュリティ総合再監査]]
- **★★ 状況 (2026-07-13 最新、Fable 司令塔で 0→100 全面改修 完了) ★★**: 🟢🟢🟢🟢 YD「フロント/バック/UI/UX が気に入らない、全部見直して」→ Fable=監査/計画/検収 + 実装 Opus・Sonnet の分業で全面改修。**監査 10次元→105所見** → **3 Wave 実装・全便 Fable 実体検収 (tsc/jest/eslint 再現+P0/P1直読+MOCK保全)**。ブランチ `overhaul/2026-07`(未push・main未マージ)に 4 コミット (13335b7 W1 / 4e4dd3d W2 / 1fc0bd5 W3 / 238254f docs)、99ファイル +15427/-8022。**★ P0×3 の MOCK デモ破壊 (コメント/タスク完了/PJ完了クライマックスが戻る) を根治** + HANDOVER 最優先「設定プロフィール読み込み中固定」根治 + バックエンド認可穴封鎖 + 画面の状態機械整合 + デザイン磨き込み。**YD 判断待ち4点**: ①バックエンド実DB適用GO(migration022+Edge7本deploy、GOでFable代行・未適用でも無変更) ②お宝箱の新演出画面(新機能) ③審査/安全系(アカウント削除・UGC通報・Push・Sentry) ④MOCK cold-restart永続化。**正本 = `~/AI projects/project-agent-application/OVERHAUL_2026-07.md` + HANDOVER.md 冒頭 2026-07-13**。走行中BG便なし。YD側=実機確認+push可否。詳細 [[2026-07-13_PAA全面改修]]。
- **状況 (2026-06-06、改修前)**: 🟢🟢🟢 **シミュレータ実機で MOCK モード起動成功 + 全画面ツアー着手**(2026-06-06 CC4)。pod 全 update 整合 → `expo run:ios` で native 再ビルド → ホーム(CD「青春の余白」)正常表示。全画面ツアーは **cliclick タップ並走方式を確立**(deep link は dev-client で不可)、主要 8 画面収集 + 一次所見(設定プロフィール「読み込み中」疑い / AssistiveTouch 大ギア全画面被り / 設定・振り返り右上キャラアイコン要確認)。**YD「YD 手動巡回 + 僕が並走」方式に合意 → 次セッションでツアー継続 → 狙い撃ち修正**。起動/ツアー手順 memory = [[sim-launch-and-tour-method]]。正本 = HANDOVER.md 冒頭 2026-06-06 CC4。
  - (以下 2026-06-04/05) 🟢🟢 **実モード(本番DB)を mock スタブ状態から「複数人フルフロー」まで全配線完了 + 本番E2E実証**。1セッション15コミット: ① データ層7ファイル実配線(機能監査🔴8解消)+ 画面guard17件(🟠17)② DB migration 015–021 を実DB `lkrmziwygyyyijyabtzp` に適用(get_member_profiles RPC=users self-only越境の唯一点・PII非返却 / handle_new_user / create_tm・project・team / task_assignees.status整合 / avatars バケット)③ 作成系UI新規(PJ/Team/メンバー管理)④ Edge関数12個 deploy + Gemini鍵設定 + 招待join slugバグ修正。**★ 本番DBで E2E: バックエンド31/0 + アバター9/0 PASS(残留0で掃除済)** = 登録→TM/PJ/Team作成→招待join→タスク→提出→AI診断→振り返り→アバター写真 が全部繋がり実証済。MOCK友達デモ(FORCE_MOCK・TestFlight)は終始無傷。**残= YD実機UIツアー / design磨き込み / Google・LINE点灯(コンソール待ち)。コード的ブロッカーはほぼ打ち止め。** ★ **正本は `~/projects/project-agent-application/HANDOVER.md` の冒頭(2026-06-04 CC3続き)**。
  - (以下 2026-05-26 時点の履歴) 🟢 Phase 2 大方針再定義版 第 7 回 Spec Review Pass 確定 (2026-05-26) — planner ⇄ spec-reviewer 自立ループ 7 ラウンドで「致命 0 / 中 0 / 軽 1 (Section G 重複の Phase 2 持ち越し)」のクリーン仕様書完成。designer Sprint 01 + 07 第 1-2 弾 + 08-10 全件完了 (18+ ファイル、約 734KB)。builder Sprint 01 修正版完了 (33 ファイル、tsc/lint/expo export web/直書き grep 全部 Pass、Expo SDK 56 採用 + Tamagui 不採用)。qa/design-evaluator BG 走り中 (自立ループ初運用)
- **★ 大方針再定義 (2026-05-26、パートナー雑談議事録より) ★**: 「Teams ライクなタスク管理アプリ」→ **「チームで頑張る過程を青春にして、頑張りを可視化する Z 世代向けハイブリッドアプリ」**。タスク管理は手段、目的は青春創出 + 頑張りの可視化 + 100% やりきれるチーム体験
- **5 キラー機能 (MVP 必須)**:
  1. **お宝箱 (仮称、Sprint 08)** — タイムカプセル、ミーティング中通知 → 写真撮影 → PJ 完了まで非公開 → リーダー完了で 1 本のムービー自動生成。YD「これだけで勝ち」発言
  2. **振り返りダイジェスト (Sprint 09)** — PJ/月/半年/年 の 4 粒度自動生成、Spotify Wrapped 風。素材はお宝箱から
  3. **ポートフォリオ共有 (Sprint 10)** — プロフィール (達成 PJ 数 + 総合ポイント) を共有可能リンク化、インスタ自己紹介で「意識高い」感醸成
  4. **AI Recognition (Sprint 10)** — Gemini API で「縁の下の力持ち」風の褒める一言診断 (Apple Recognition 文化インスパイア)
  5. **寄せ書きコーナー (Sprint 05 統合)** — PJ 完了でタスク管理ページが寄せ書きに変化
- **+ タスクお掃除コンセプト (Sprint 07 統合)**: キャラ足元のゴミがタスク完了で減る視覚演出
- **ペルソナ 5 人体制**: カナさん / ハルキくん (現 SPEC 維持) + **ノアミ** (病みやすい努力家、精神サポート + 寄り添い) + **タイチ** (能力高い、自分も周りも厳しい、効率化) + **ガブちゃん** (自己肯定感低い、経歴で自信、スキル蓄積)
- **技術スタック (確定、変更不可)**: React Native + **Expo SDK 56** (planner 設計時 53+ 想定 → Sprint 01 builder で 56.0.4 採用、`code/IMPLEMENTATION_NOTES.md` 1.1) + Expo Router + **NativeWind v4 + Tailwind v3 単体構成** (Tamagui は SDK 56 + React 19 + Tailwind v3 衝突懸念で Sprint 01 不採用、Sprint 07 で再検討、`IMPLEMENTATION_NOTES.md` 1.2) + Reanimated 3/4 + Supabase (Postgres + Auth + Storage + Edge Functions) + Expo AuthSession (Google/Apple/LINE/Magic Link) + Expo Notifications (APNs+FCM) + **Gemini API (Free 1500 RPD → 有料 Gemini 2.5 Flash $0.075/M tokens)** + EAS Build + App Store + Google Play
  - ★ **Anthropic API は採用しない** (商用配布で Max 20x 個人プラン不可、API key は Supabase Edge Function で隠蔽)
  - ★ **招待制は採用しない** (誰でも DL 可能、議事録のレア感言及は誤読)
  - ★ **メール送信全 Phase 排除** (Magic Link 認証メールのみ Supabase Auth 標準として OK)
- **車輪の再発明禁止原則** (★ YD 強調): Discord/Spotify/Instagram/BeReal/Finch/Duolingo + 新規 BeReal/Setlog (お宝箱) + Spotify Wrapped (振り返り) + LinkedIn (ポートフォリオ) + Apple Recognition (AI 診断) のベンチマーク参照
- **マーケティング戦略**: インスタストーリー導線 (PJ 開始 / 完了画面 9:16) + 意識高い大学生にニッチ刺し (自然な口コミ) + 先行体験 = Salamat 内 PJ + ハタチタチ + 長期 100 億バイアウト目標
- **スプリント分割 (MVP 10)**:
  - sprint-01: 基盤 (Expo + 認証 4 種 + Onboarding **5 ステップ** + キャラ名カスタマイズ基盤 = `DEFAULT_CHARACTER_NAME = 'ハル'` + `useFinchName()` フック先行配置)
  - sprint-02: 5 階層モデル (User/TM/PJ/Team/Task + RLS + scope トリガー)
  - sprint-03: タスク詳細 + 提出 (ファイル/Drive/写真)
  - sprint-04: リマインダー + 通知 (Expo Notifications + in-app バッジ)
  - sprint-05: **軽量チャット + 寄せ書き** (タスクコメント + メンション + 寄せ書きコーナー)
  - sprint-06: DB 自動登録 (学校 800 校 + サークル OGP)
  - sprint-07: **キャラ + ホーム + お掃除コンセプト** (デフォルト名「ハル」+ Adaptive UI + ゴミ減る演出 + 設定画面キャラ名変更 + キャラ `proud` 表情 SVG 追加)
  - ★ **sprint-08: お宝箱 (仮称)** (タイムカプセル管理 + 写真投稿 + 完了まで非公開)
  - ★ **sprint-09: 振り返りダイジェスト** (PJ/月/半年/年 4 粒度、素材は 08 から、mp4 60 秒)
  - ★ **sprint-10: ポートフォリオ + AI Recognition + 仕上げ** (共有リンク + Gemini 診断 + ストア配布)
  - ※ ファイル名は実体名 (sprint-XX-旧名.md) のまま、冒頭注記で「新分割」明記
- **マネタイズ (Phase 2 持ち越し)**: 推奨 D = 個人 Pro 480 円/月 + 団体 Pro 1480 円/月 (両方契約可)。新方針では「初期赤字 OK、Gemini Free 枠で MVP コスト 0」、MVP 完成後に判断。形態 = 個人事業主開業届 ★ 確定
- **★ 2026-05-26 朝〜昼の最終状況 (フルスプリント完了 + Vault 全件監査済)**:
  - ✅ designer Sprint 01 + 07 第 1-2 弾 + 08-10 完了 (18+ ファイル、約 734KB、Snack 互換)
  - ✅ builder Sprint 01 初版完了 (Expo SDK 56、Web preview 動作確認可)
  - ✅ builder Sprint 01 **修正版完了** (33 ファイル、tsc/lint/expo export web 24 ルート/直書き grep 全部 Pass、キャラ名カスタマイズ反映)
  - ✅ 自立ループ planner ⇄ spec-reviewer **第 7 回 Pass 確定** (致命 0/中 0/軽 1 = Section G 重複 Phase 2 持ち越し可)
  - ✅ キャラ名カスタマイズ仕様反映 (DEFAULT_CHARACTER_NAME 「ハル」+ useFinchName フック + Onboarding step 5 + 設定画面 Sprint 07)
  - ✅ 商標調査 ([[2026-05-26_kohaku_trademark_research]] 「コハル」NG = ポケモン + Dr.Stone/犬夜叉 + Ethereum 三重衝突) + Z 世代 5 アプリリサーチ ([[2026-05-26_z_gen_app_research]] 応用候補 10 個 + NG 4 件) 完了
  - ✅ Web 対応バグ修正 (secureStorage ラッパー、AuthContext + useUiMode)
  - ✅ **★ Vault 全件監査完了 (2026-05-26、メイン Claude ⇄ サブエージェント 二重チェックループ Pass)** — 差分分析 `/tmp/project_agent_app_audit_diff.md`、Round 1 報告 `/tmp/audit_round1_report.md` (致命 0/中 7/軽 5、すべて運用マニュアル + active_projects + claude_mistakes に反映済)
  - ✅ **Sprint 01 完全 Pass (2026-05-26 11:35、log.md 参照)** — 自立ループ初運用 loop 2 で達成、qa-evaluator **Pass 38/0** + design-evaluator v2 **Pass 9/0** (AI スロップ匂い度 0)。次セッションで Sprint 02 (5 階層モデル) 着手可能
  - ★ **YD 指示 (最重要)**: builder/designer ⇄ evaluator 自立ループ運用ルール確立 + 「キリのいいところで終了」モード明文化 ([[2026-05-26_セッション引継ぎ_自立ループ強化指示]])
- **次のアクション (新セッション以降、優先度順)**:
  - [ ] **★ 新セッション再開**: `~/projects/project-agent-application/HANDOVER.md` を Read (Sprint 01 Pass 確定済、Sprint 02 着手可能)
  - [ ] **★ designer Sprint 02 起動** (5 階層モデル UI、Discord サーバーサイドバー風 + チャンネルリスト風) → design-evaluator 自立ループ
  - [ ] designer Sprint 02-06 起動 (機能ロジック中心) → design-evaluator 自立ループ
  - [ ] builder Sprint 02-10 順次着手 → qa + design-evaluator 自立ループ (Pass まで)
  - [ ] お宝箱の正式名確定 (planner 推奨「タイムカプセル」、MVP 後 OK)
  - [ ] アプリ名 + お掃除コンセプト名確定 (MVP 後 OK)
  - [ ] マネタイズ Q13 確定 (MVP 完成後判断)
  - [ ] DESIGN_DIRECTION.md Section G→H リネーム (Phase 2 軽微、`DESIGN_DIRECTION.md:685`)
  - [ ] ファイル名リネーム (sprint-XX 内容に合わせて、関連リンク既に実体名で参照済なので互換)
  - [ ] **Sprint 02 着手前 YD 作業 (8 項目)**: Supabase プロジェクト作成 / Apple Developer 登録 ($99/年) / Google OAuth クライアント ID / LINE channel 発行 / Supabase Auth Provider 登録 / Supabase DDL apply (`supabase/migrations/2026-05-26_001_initial.sql`) / アプリアイコン (Sprint 07 着手前) / EAS Build アカウント (Sprint 10)
- **パス**: `~/projects/project-agent-application/`
- **生成済ファイル**:
  - `.claude/sprints/`: SPEC.md (41.7KB) / DESIGN_DIRECTION.md (40.3KB) / sprint-01〜10.md (175KB) / REVIEW_REPORT.md (29.8KB、★ **第 7 回 Pass 確定**)
  - `.claude/agents/`: planner / spec-reviewer / designer / builder / qa-evaluator / design-evaluator (6 件、各 8〜11KB)
  - `design/sprint-01-基盤/`: mockup-login.tsx + mockup-onboarding.tsx + tokens.md + components.md + flow.md + snack-url.txt
  - `design/sprint-07-home/`: mockup-home.tsx (第 1 弾) + mockup-home-v2.tsx (第 2 弾) + tokens.md + components.md + flow.md + snack-url.txt
  - `design/sprint-08-お宝箱/`: mockup-treasure-trigger.tsx + mockup-treasure-capture.tsx + mockup-treasure-vault.tsx + tokens.md + components.md + flow.md
  - `design/sprint-09-振り返り/`: mockup-digest-list.tsx + mockup-digest-pj.tsx + mockup-digest-yearly.tsx + tokens.md + components.md + flow.md
  - `design/sprint-10-ポートフォリオ/`: mockup-portfolio.tsx + mockup-portfolio-public.tsx + mockup-recognition.tsx + tokens.md + components.md + flow.md
  - `design/q11-exploration/case-a〜d/`: Q11 試作 4 案 (Case D 採用済)
  - `VISION.md` (27.5KB): 末尾「2026-05-26 大方針再定義」セクション追記済
  - **`HANDOVER.md` (14.8KB)**: 次セッション引継ぎ用、走り中 BG Agent ID + 未完了タスク一覧 + 最短手順
  - **`code/`**: Sprint 01 builder 修正版実装 (33 ファイル、Expo SDK 56)
  - **`code/IMPLEMENTATION_NOTES.md` (27.9KB)**: SDK 56 採用 + Tamagui 不採用 + NativeWind v4 + Tailwind v3 + ESLint immutability off + Apple ボタン #000 例外 + Sprint 02 申し送り + YD 作業申し送り (8 項目) + dev サーバー起動方法
  - **`code/AGENTS.md` + `code/CLAUDE.md`**: Expo SDK 56 公式ドキュメント参照 (https://docs.expo.dev/versions/v56.0.0/)
- **Snack URL** (Sprint 07 ホーム第 1 弾、`design/sprint-07-home/snack-url.txt` 参照): Secret Gist + files.url 参照方式、Gist ID `9a9597e7370ad6babcc1e8db24488ae1`
- **議事録**: `~/ObsidianVault/raw/meetings/2026-05-26_新アプリ議事録_yd_partner_{生ログ,gemini整理}.md`
- **関連**: 
  - [[project_agent_application]] (運用マニュアル、2026-05-26 全件監査反映済 = SDK 56 / Tamagui 不採用 / NG 31 / 第 7 回 Pass / Sprint 01-10 完了状況 / IMPLEMENTATION_NOTES 要約 / YD 作業 8 項目 / Z 世代リサーチ反映状況 等)
  - [[2026-05-23_Project_Agent_Application_着手]] (Phase 0、5 エージェント体制構想)
  - [[2026-05-23_TaskHub廃止_ProjectAgentApp移行]] (旧方針起点)
  - [[2026-05-26_ProjectAgentApp_セッション保存]] (旧方針 Phase 2 完了スナップショット)
  - [[2026-05-26_新アプリ大方針再定義]] ★ 本セッション最重要意思決定 (議事録ベース)
  - [[2026-05-26_セッション引継ぎ_自立ループ強化指示]] ★ 自立ループ + 終了モード明文化
  - [[2026-05-26_kohaku_trademark_research]] (商標調査結果、「コハル」NG → 「ハル」推奨)
  - [[2026-05-26_z_gen_app_research]] (Z 世代刺し 5 アプリリサーチ、応用候補 10 個 + NG 4 件)
  - [[claude_mistakes]] A-14/A-15/B-5 (Edit 誤検知 / gh gist ブロック / planner Bash 不持) + D-6 (評価ステップ省略) + E-4 (ポートフォリオ vs 年末振り返り) + D-7 (Anthropic Max 20x 商用提案) + D-8 (招待制誤読)
  - **議事録**: `~/ObsidianVault/raw/meetings/2026-05-26_新アプリ議事録_yd_partner_{生ログ,gemini整理}.md`

### 4. Salamat WBSサイト
- **状況**: ✅ **Phase 1 + Phase 2 演出強化 完了 + Vercel 本番デプロイ済 (2026-05-19 夜)** — Phase 1 (Hero ZoomParallax×MeshGradient / Gallery4 / Cobe / LocationTag / Glowing Shadow / List⇄Orbital) + Phase 2 (Magnetic+Fey ボタン + Three.js パーティクル背景 + 旧コード/写真ファイル名 cleanup) を 2 commit (`46e6839` / `20ae3ee`) + 2 回 Vercel 本番デプロイで反映。YD ブラウザ目視 OK。
- **公開URL**: **https://toyo-salamat.com** (独自ドメイン、2026-05-25 切替完了) / https://salamat-website-v2.vercel.app (Vercel デフォルト URL、引き続き有効)
- **2026-06-01 検証・一括修正 (ブランチ `feat/audit-fixes` commit `47ff6d2`、main 未マージ)**: 本番ビルド実機検証 + 7次元並列解析(確定59件)。致命=モバイルナビ欠落→ハンバーガー新設。Tweaks/オービタルを本番非表示(dev限定ゲート)。WebGL6個を画面外停止/reduced-motion をJSにも/カード文字コントラスト/未使用CSS約170行削除/metadataBase=toyo-salamat.com+個別OG+robots/sitemap。本番build・TS・ESLint・コンソール全クリーン。⚠ dev サーバは Turbopack panic(node_modules の npm/pnpm 混在 + 不正な pnpm-workspace.yaml)で不安定=本番は正常。コンテンツ事実不整合(Selma説明矛盾等)・ダミーリンクは未変更で `AUDIT-FINDINGS.md` に列挙(YD対応待ち)。詳細: [[2026-06-01_Salamat_サイト検証修正]]
- ⚠ **本ノートの実態ズレ**: 下層ページ(About/Activities/Reports)は実装済 (リポジトリに存在)。作業パスは実際には `~/projects/salamat-website-v2` (git管理) を使用。
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
  - [x] **独自ドメイン取得 → Vercel に紐付け (2026-05-25 完了)** — `toyo-salamat.com` Wix 取得済を Vercel に紐付け、Apex メイン + www→Apex 308 リダイレクト、SSL 発行済、Google Workspace MX 保護。旧 `salamat-toyo.web.app` は放置で並行稼働。詳細: [[2026-05-25_Salamat_WBS_独自ドメイン化]]
  - [ ] (要すれば) パーティクル密度/opacity の微調整、blending mode の AdditiveBlending 検討
- **チーム**: YD (実装) + Riko (内容) + Haruka (構成) + Rena (デザイン)
- **スタック**: Next.js 16 + React 19 + Tailwind v4 + three + cobe + @paper-design/shaders-react + framer-motion + embla-carousel-react + Sora/Zen Maru/Noto Sans JP
- **パス**: `~/Downloads/07_開発・アプリ制作/salamat-website-v2`
- **Vercel scope**: yitao-dings-projects、プロジェクト名 `salamat-website-v2`
- **現行サイト**: `salamat-toyo.web.app` (Firebase、置き換え対象)
- **関連**: `knowledge/salamat/wbs_team.md`, [[2026-05-19_Salamat_WBS_Phase1実装]], [[2026-05-19_Salamat_WBS_Phase2_演出強化]], [[vercel]], `~/Downloads/07_開発・アプリ制作/salamat-website-v2/design-brief.md`

### 5. Arte Grow (社会起業 → ★軸=映像で伝える)
- **★ 2026-08-08 軸再定義**: 支援団体でも物販会社でもなく「普通じゃない環境に入って撮って伝える」が軸。収益構想=①メディア ②コマース ③制作受託。渡航は「視察」でなく「パイロット撮影+アクセス開拓」
- **渡航確定: 2026-09-21〜10-03** (セブ滞在9/26〜10/3、CFAI訪問10/1-2。メンバー: YD/Rina/Taichi/Hina。詳細=`knowledge/arte_grow/2026-09_渡航情報.md`)。Manuel Stumpf (CFAI = Christ for Asia International / Feeding & Family Center、ダンちゃり経由) に訪問打診Messenger送信済(8/8)。返信後に映像無償制作オファー
- **CFAI宿泊 10/1-3 予約承認済み・条件確認中** (4名) → `knowledge/arte_grow/cfai_guesthouse_2026-10.md`。9/26〜10/1は **Hotel Asia Cebu** (Capitol Site) 泊予定
- **管理正本 = Notion「Arte Grow｜フィリピンプロジェクト」** (新アカウント。タスク12件/論点5件/アイデア6件/現地コンタクト/参考事例のDB5つ)
- **次のアクション**:
  - [ ] Taichiとビジョン擦り合わせ (期限8/15、最優先)
  - [ ] 「なぜ搾取じゃないのか」の一行化 / 発信の主戦場・名義の決定
  - [ ] ゲートキーパー候補リスト(8/22) / ANTHILL商談アポ(8/25)
- **モデル**: Pride, Not Dependency
- **共同創業**: Taichi (19歳)
- **詳細**: [[2026-08-08_ArteGrow_軸再定義_渡航確定]]
- **関連**: `knowledge/arte_grow/`

## 🟡 完成済み・運用フェーズ

### 6. ~~Task Hub~~ — **2026-05-23 廃止** ⚫
- **2026-07-11 ローカル実体を `~/AI projects/_archive/salamat-task-hub` へ移動** (GitHub/Firebase遺産は方針通り温存)。taskhubエージェント定義4本は project-agent-application/.claude/agents/ へ退避コピー済み
- **状況**: ⚫ **2026-05-23 廃止決定。** YD「マジで使わない」と明言、代替として **Project Agent Application** (`~/projects/project-agent-application/`) を新規開発。Salamat 内部運用も本アプリで完全置き換え予定。
- **詳細**: [[2026-05-23_TaskHub廃止_ProjectAgentApp移行]] / [[../archive/2026-05_TaskHub]]
- **遺産**: GitHub repo (`Yitao-Ding/salamat-task-hub`) + Firebase Hosting (`salamat-task-hub.web.app`) は当面放置 (削除しない、参考資料)
- **引き継ぎ**: 4 自立型エージェントのハーネス設計 / Discord ロールモデル / PWA 重視 / 寒色系 → 本アプリへ思想継承

### 7. Lecture Hub (個人ナレッジハブ)
- **状況**: ✅ **Fable 5 全面改修 実装完了 (2026-07-05)** — 残り = YD 作業のみ: ①migration 0004 を SQL Editor で適用 ②/admin/reindex 1 回実行 ③実機確認 ④push 許可
- **★ セキュリティ監査+硬化 (2026-07-06、commit 74df2ac)**: 多角監査(生存35件)。**最大リスク=認証ゼロ設計のため公開デプロイで全API/Server Actionが無認証到達可能**(過去 lecture-hub-sable は Deployment Protection 無し実測)。対応=①境界ゲート(GATE_PASSWORD env時のみ有効な合言葉middleware、owner_id不使用で認証復活に非該当)②PdfNode/AudioNodeの格納型XSS(javascript: URL)修正③AIコスト上限(maxOutputTokens+入力長)④transcribe SSRF/upload硬化⑤CSP/HSTS/X-Frame-Options⑥cron ?secret=廃止+定数時間比較。**YD要対応=本番で Vercel Deployment Protection を ON**(ゲートは多層/代替)。
- **改修サマリ (2026-07-05 完了、commits 6910401〜6c90321 の 7 本、ローカル未 push)**: 監査 79 件 + レビュー 13 件を全修正。P0 = Cron テンプレ TipTap 化 / エディタ CSS 自前実装 (.rich-text) / チャットのスレッド乱造・履歴不表示 / タスク偽 UUID・期限 UTC 9時間ズレ / 検索の生 JSON 索引→plain_text 化 (0004) / デバウンス保存消失→即時ローカル保存 / 親削除の子孤児化→再帰 soft-delete + undo トースト / ページ遷移で内容混線 (key 欠落) / モバイル drawer 追加。詳細 = lecture-hub/HANDOVER.md 冒頭
- **★ 2026-07-03 Fable 5 全面改修 (YD 指示「根本から見直して全部修正」)**:
  - ultracode 監査ワークフロー (71 agents) 完了 → **確定 79 件** (仮説 19 + レンズ発見 60、敵対的検証済み・棄却 0)。結果=`lecture-hub/handover/*.json`
  - 主要 P0: ①Cron テンプレが旧 BlockNote 形式で日記/講義ページが空表示 (編集すると本文恒久消失) ②prose クラスが typography プラグイン不在で無効=エディタ本文ほぼ無スタイル ③新規チャットで毎メッセージ新スレッド乱造 ④タスク楽観更新が偽 UUID で操作が DB に効かない ⑤tsv が生 JSON 索引 ⑥デバウンス保存がページ遷移で消失 ⑦タスク期限が本番 UTC で 9 時間ズレ
  - **完了**: 依存整理 (TaskList/Placeholder/react-markdown 追加、shiki 等 7 件削除) + globals.css 全面拡張 (.rich-text 自前タイポ / hljs 変数化 / color-scheme / 不足トークン) — commit 6910401, 5159f21
  - **残り**: HANDOVER.md の実装プラン B〜N (エディタ/オフライン同期堅牢化/検索 migration 0004/Cron テンプレ TipTap 化/チャット 2 層化/タスク/AppShell モバイル対応/API 硬化/docs)。完了後に YD 作業 = migration 0004 手動適用 + /admin/reindex + 実機確認
- **公開URL**: `https://lecture-hub-sable.vercel.app/` (新エイリアス、200 公開アクセス可)
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
- **⚫ 2026-07-11 停止・アーカイブ**: 要約44/44失敗・Drive0/45で一度も届かず、ai-researcher週次ダイジェストに一本化。実体は `~/AI projects/_archive/morning-briefing`。cron行はYDが `crontab -r` で削除予定
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
- **⚫ 2026-07-11 アーカイブ済み** (`~/AI projects/_archive/ai-simulator`、移動前にgitスナップショット済み)
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
- **🟢 2026-07-11 復旧**: launchd 3本を新パス (`~/AI projects/ai-researcher`) で再登録、6/14の未コミット193行をコミットし GitHub private (Yitao-Ding/ai-researcher) へpush
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
- **⚫ 2026-07-11 アーカイブ済み** (`~/AI projects/_archive/business-plan-sprint-2026-05-19`)。FINAL_REPORT 3案 (LegalTrio/職人EC/相続SaaS) の意思決定は未完のまま — 本エントリの要約が正本
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
