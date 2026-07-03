---
type: current_state
last_updated: 2026-06-06 (パソコン1台の学校: ★ドメイン接続完了=pc1school.com を お名前.com で取得→Vercel接続(apex+www / NS=Vercel / SSL / HTTPS200確認)。Stripe本番化フォーム回答を全確定(商品説明文/明細書表記/改正割販法セキュリティアンケート)=CC-business/STRIPE-ACTIVATION.md。残=Stripe本番キー sk_live_ + 本名→brand.ts + Resend検証 + Stripe/Vercel 2FA。正本=CC-business/HANDOVER.md §12)
update_frequency: 週1回以上
---

# 進行中プロジェクト一覧

> このファイルはYDの「今アクティブに動いてるもの」のスナップショット
> 完了したものは `archive/` へ、休眠中は `archive/sleeping/` へ移動
>
> ⚠️ **2026-06-14 全件棚卸し済**: 実体ベース(git log/mtime/HANDOVER)の全プロジェクト棚卸しを `~/projects/AUTONOMOUS_SESSION_2026-06-14.md` に作成。本ファイルの各ステータスは5月下旬〜6月上旬時点なので、続ける/殺すの判断はそちらの「即決マトリックス」を参照。要点: ①大半は "YD待ち"(認証/本名/素材) ②CC-businessが最も本番ローンチ間近 ③ai-researcher/morning-briefing は "動くが成果が届かない" 状態。

## 🟢 アクティブ・優先度高

### ★ パソコン1台の学校 (SNS情報商材システム、2026-06-03 着手) ★
- **★ 教材v2全面改訂 始動 (2026-07-03、Fable 5)**: YD「Opus製の中身がぼんやり、価格に見合わない」→ 全86レッスン監査(worthScore3〜4/10・PII破損文9箇所)+Vault発掘7領域(vidkit/AE自動制御/localhost FM/Notion案件管理/easy-share/舞台裏/スイープ)→ **新カリキュラム全10コース(C0〜C9)・約148レッスン確定**。体制=監査/計画/司令はFable5・実装のみSonnet5。ブリーフ2/10コース完成時点でセッション上限到達→引き継ぎ。**正本=`CC-business/v2/HANDOVER-v2.md`**(残り=ブリーフ8コース→Sonnet執筆×Fable検収→サイト統合)。⚠️ v2完成前のGo-live非推奨(現行本文にPII破損文が残存)。
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

### ★ Yitao Film｜案件管理ダッシュボード (Notion、2026-06-24 実案件全件投入完了) ★
- **状況**: 🟢 **chat構築のNotion DBに実案件を全件投入完了 (2026-06-24)**。クライアント7・案件10・タスク84件を作成、ホームに「新規案件テンプレ」トグル追記。3DB(👤クライアント / 🎬案件 / ✅タスク)は relation + 進捗率/タスク数ロールアップで連動。エラー0で完走、案件1/案件3を実機fetchで検証済。
- **ホーム**: https://app.notion.com/p/388cf01b1dc581eb8418d4b6ea8c08ab
- **投入内容**:
  - 動画5: ①ダンサレチャリティーWS(千葉)編集[納6/26] ②長野インタビュー編集[納期未定] ③**ハイミ「蛹」**MV[納6/29] ④平成立祭メイキング&ダイジェスト[納7/12] ⑤大型イベントOP[納7/1・¥70,000・最優先]
  - 写真5: ⑥SANA[納6/25] ⑦aキリア写真/⑦bキリア動画[撮影6/29] ⑧ナナ ⑨のあみ
- **★ 要確認(YD回答待ち、6点)**: ①案件2納品日 ②案件5の撮影要否+主催クライアント名 ③案件7写真/動画を別案件で作成(統合希望なら言う)+納品日 ④写真5人目氏名 ⑤写真勢クライアント区分(暫定アーティスト) ⑥キリア打ち合わせ内容(記入欄だけ作成済)
- **運用**: 新規案件は Claude に「種別・案件名・クライアント・納品日・撮影有無」を伝えれば本文テンプレ+標準タスクごと自動作成。サンプルデータ(YF-1〜3)はYD手動削除予定で未削除(案件IDは YF-4 以降)。本案件群は Yitao Film(Arte Grow / やることDB は対象外)。
- **関連**: [[2026-06-24_Yitao_Film案件Notion投入]]、`knowledge/filmmaking/yitao-film-notion-handoff.md`

### ★ JAMBOREE STUDIO BATTLE Lite OP動画 (大型イベントOP、¥70,000・最優先) ★
- **★ v2「THE EYE」BGM完全確定 (2026-07-03)**: 🎵 **`music/LiteOP_BGM_FINAL.wav`(ElevenLabs Music v2生成「Neon Colosseum」116.000秒・無加工)をYD採択**。経緯=Artlist切貼v0→ステム再構成F→Suno手術v3と3系統却下の末、「秒数構造入り作曲発注書で1曲丸ごと生成・一切切らない」が正解(詳細=[[2026-07-03_LiteOP_BGM_AI生成路線確定]]、レシピ=`BGM_Neon系候補/recipe.txt`)。素材18+SFX4検品済み・審査員8名+MC切り抜きv2完了(`素材_審査員_切り出し_v2/`)。**次セッション=「Netflixの最高の監督」ペルソナでAE 4Kスケルトン(D2)から。全TCは`music/LiteOP_BGM_FINAL_structure.md`の実打点(ドロップ36.0/転換59.8/開眼95.87)に吸着**。正本=`HANDOVER_v2_THE_EYE.md`。
- v2初期経緯: 8並列ディスカバリー→5案→4レンズ審査で「THE EYE」優勝(YD FB反映済: 多色カラフル/顔ナチュラル/英語VO/エンドカード情報なし/4K)。スタジオ42/11都県・KEN表記確定済み。Higgsfield課金見送り。
- **状況 (2026-07-02 夜)**: 🟢 **v8 = ロゴ抜き直し+全編品質スイープ+YD実機FB反映 完了**。①ロゴをpdftocairo正攻法で透過抜き直し(colorkeyの欠け根治) ②全編スイープ: 200フレーム→9視点並列レビュー52指摘→Sonnet実測検証→13件修正・4件実証棄却 ③YD iPhone実機FB反映: MC/審査員の顔の白飛び解消・チームロゴチップ1.5倍可読化・B8/B9/B10のFX増量・**B11再構築(巨大だるま降臨→震え縮小→地響きドーン→開眼で瞳が入る)**。体制=Fable5設計/Sonnet5実装・検証。**最新成果物=`書き出し/全体爆上げ_0-90s_v8_YDFB反映.mp4`(720p版YD送付済み・レビュー待ち)**。
- **★ 引き継ぎ書(唯一の真実)**: `/Users/ittou/整理済み/Lite_イベントOP_2024-2026/HANDOVER_映像制作.md` — AE自動制御のハマり所全部+FXライブラリAPI+実装結果と差分(§7b)+B2マップ/ロゴ仕様(§7d)+**スマホリモート操作手順(§10)**。新セッションはこれを読めば続行可。
- **リモート操作**: 新セッションは `start_remote_session.sh`(tmux "liteop")で起動→スマホSSH(Termius)から attach。YD側未完: リモートログインON / Tailscale導入 / Macスリープ無効(§10のチェックリスト)。
- **残りTODO(§7c)**: YDレビューFB反映 → 憲=KEN表記確認 → BGM確定後の音同期 → 最終書き出し。
- **並走**: BGM は別CCセッション担当(`BGM作業指示書_新CC用.md`)。music-first で確定後に映像側の秒を合わせる。
- **方向性(YD明言)**: 派手派手・ガンガン・コテコテOK・遠慮禁止。多色OK(2026-07-02指示)だが緑#13A89B=背骨は死守・和風完全排除・FIFA×パイレーツ。5000人会場のOP、成功すれば次に繋がる大型案件。

## 🟢 アクティブ・優先度中

### ★ easy-share (社内 撮影素材 共有・プレビューツール、2026-05-31 着手) ★
- **★ 公開URL(稼働中・正式運用)**: **https://easyshare-fx.vercel.app**(Vercel 本番、Deployment Protection 無効=認証なし公開、YD 認可済「セキュリティ不要・公開OK」)。本物の FX30 実素材(4プロジェクト: 2026-05-30/05-31/sample/写真、flat+4ルックWebGL切替、写真/動画分離)がライブ表示。
- **★ ストレージ = Cloudflare R2(2026-06-02 移行完了・本番稼働)**。バケット `easy-share-media`、公開ベース `https://pub-6cfa3ffdc3ec456790e058cce335c70d.r2.dev`。Vercel 本番 env `NEXT_PUBLIC_ASSET_BASE` が R2 を指す。10GB無料/egress永久無料/3TB$45月。認証は `~/.config/rclone/rclone.conf [r2]` と gitignore config のみ(Vault非保存)。CORSキャッシュ罠は `VideoView` の poster属性削除で解決済(再付与厳禁)。
- **★ Finder 風 閲覧UIに全面刷新 (2026-06-02)**: 旧1枚スクロールギャラリーを置換。4表示モード(アイコン/リスト/カラム/ギャラリー)+ツールバー切替(⌘1-4)、サイドバー(スマートグループ すべて/最近/動画/写真 + プロジェクト別)、ソート/フィルタ/横断検索/サイズ、Quick Look(フォーカストラップ)、URL共有(?view=&g=&p=&asset=)、localStorage永続化、レスポンシブ(モバイル=ドロワー/カラム非表示/タップで開く)、a11y強化。`web/src/components/browser/` 新設+`GalleryApp.tsx`書換。設計workflow→実装→レビューworkflow(41提起→反証18確定→反映)。
- **★ 写真原本のアップロード&サイトDL (2026-06-02)**: `process.sh` の `UPLOAD_ORIGINALS` を 0/photos/all 化(既定 photos=写真原本のみR2、Content-Disposition:attachment付与)。サイトの「元データをDL」は写真のみ表示(動画原本は未対応=404防止)。既存本番写真 **77枚(2.15GB)を R2 にバックフィル済**(公開URLで 200+attachment 確認)。動画原本も要れば config を all + Webの !isVideo ゲート解除で対応。
- **★ git 初コミット (2026-06-02)**: easy-share を独自リポジトリ化(親 `~/projects` 直下に未追跡だった)。2コミット(初コミット=Finder UI+取り込み一式 / 写真原本DL)。**push はまだ**(YD許可待ち)。secrets(config.sh/.env*/rclone)・LUT(*.cube)・大容量メディア(`sample/`=RAW1GB, `public/sample/derivatives/`=250MB)は .gitignore 除外。
- **★ B案(クライアントサイドLUT)**: 動画は flat 1本だけエンコード、4ルックは**ブラウザ WebGL2 で .cube LUT をリアルタイム適用**(`VideoView.tsx`)。LUT は R2 `luts/<id>.cube`、`process.sh` のR2経路でlut自動アップ。
- **★ 「Google Drive 風」全自動・常駐運用 稼働**: **`~/EasyShareDrop/`** に放り込む → `watch.sh`(**launchd `com.easyshare.watch` 常駐**)→ `autosort.sh`(撮影日で自動振り分け、重複は `_duplicates/` 温存)→ `process.sh`(UPLOAD=1/STORAGE=r2)で色変換+R2自動アップ→公開URL反映。サブフォルダ名=プロジェクト名。
- **★ iOS DL 灰色四角バグ解決 (2026-06-17)**: 写真原本(.ARW)を iPhone でDL→アルバム保存すると灰色の四角形になっていた。**原因はファイル形式でなく配信ヘッダ** — 原本の `Content-Disposition` に filename が無く iOS Safari が拡張子を落としていた(手動バックフィルの付け忘れ。ファイル自体は SHA-256 一致・QuickLook 描画可で完璧)。**全77枚の CD に filename 付け直し済**(`ingest/fix-original-headers.sh`、本体再送なしサーバサイドコピー、`--metadata-set`)。`process.sh` は元々正常。**要 YD 実機確認**(DL→アルバム→灰色にならないか)。詳細 [[rclone_r2_metadata]] / [[claude_mistakes]] B-6。
- **状況**: 🟢 **本番稼働中・全機能 実機検証済 (2026-06-02)**。R2移行 + Finder UI刷新 + 写真原本DL まで完了、Playwright で本番URL含めフル動作確認(コンソールエラー0、tsc/lint/build green)。**正本=`~/projects/easy-share/HANDOVER.md`**。2026-06-17 に iOS DL 灰色四角バグを修復(↑、実機確認待ち)。
- **用途**: ダンス×クリエイティブチーム 4 名(全員 Mac+iPhone)が、FX30 S-Log3 動画 + ARW RAW を**その日のうちに色を揃えて軽くプレビュー・共有**。素材は**プロジェクト単位**で振り分け(①/②…)。商用配布ではない社内ツール。
- **確定スタック**: 取り込み = Mac ネイティブ(sips + ffmpeg hevc_videotoolbox + rclone + fswatch) / ストレージ = Cloudflare R2(egress0) / 認証 = Cloudflare Access(4 メール) / 閲覧 = Next.js 16 PWA on Vercel(Spotify 風ダーク)
- **色の核心(検証済 partial)**: 写真は ColorSync で堅牢一致 / 動画は 709 タグ必須(未タグ=Safari が BT.601 誤解の最悪パターン)/ **S-Log3 は必ず Rec.709 LUT を焼く**(タグ不能)/ True Tone・自動輝度・Night Shift は全員オフ運用 / 最終色判断は NLE で(ツールはレビューまで)
- **graded =「ルック切替式」**(YD 提供 FX3 4 ルック Film Tone/Camp Moody/Blue Snow/Pure Night)。`ingest/luts/*.cube` を各ルックとして自動列挙、ビュアーで flat ⇄ 各ルックをトグル切替。proxy はルック分生成(flat+4=5本/クリップ、videotoolbox で軽い)。.cube 出し入れで増減
- **本番認証は単一オリジン必須**(別ホスト Access はサブリソース 302 で素材が壊れる)→ 本番デプロイは all-Cloudflare(OpenNext)推奨に修正(Phase 1 の Vercel から)。手順は `SETUP.md`
- **実機で潰したバグ**: hevc_videotoolbox の色タグ未書き込み → `setparams` 焼き込み / Tailwind v4 `@theme inline` で色未生成 → 非 inline + 再起動(計算済みスタイル検証で発見)
- **残課題(優先度順)**:
  - [ ] B案の色を本物素材で YD が端末確認(iPhone/Mac 実機。良ければB継続)
  - [ ] git push 可否判断(初コミット済・未push)。push するならリモート先決め(GitHub private 等)
  - [ ] r2.dev → カスタムドメイン(本格運用時。egress無料のまま)
  - [ ] R2の古い `look-*.mp4`(旧A案焼き残骸)/ Vercel Blob(1GB満杯)の掃除
  - [ ] 動画原本もDLしたくなったら config `UPLOAD_ORIGINALS=all` + Web の `!isVideo` ゲート解除
  - [ ] easyshare-fx は手動エイリアス運用(`vercel --prod` 後に `vercel alias set <deploy> easyshare-fx.vercel.app`)。本番ドメイン化で自動化も可
- **パス**: `~/projects/easy-share/`(HANDOVER.md=正本 / DESIGN.md / ingest/ / web/src/components/browser/)
- **関連**: [[2026-05-31_easy-share設計確定]]、[[2026-06-02_easy-share_FinderUI刷新と写真原本DL]]、リサーチ workflow `wafcglinq`

### ★ Project Agent Application (Z 世代向け青春タスクアプリ、2026-05-23 着手) ★
- **状況 (2026-06-06 最新)**: 🟢🟢🟢 **シミュレータ実機で MOCK モード起動成功 + 全画面ツアー着手**(2026-06-06 CC4)。pod 全 update 整合 → `expo run:ios` で native 再ビルド → ホーム(CD「青春の余白」)正常表示。全画面ツアーは **cliclick タップ並走方式を確立**(deep link は dev-client で不可)、主要 8 画面収集 + 一次所見(設定プロフィール「読み込み中」疑い / AssistiveTouch 大ギア全画面被り / 設定・振り返り右上キャラアイコン要確認)。**YD「YD 手動巡回 + 僕が並走」方式に合意 → 次セッションでツアー継続 → 狙い撃ち修正**。起動/ツアー手順 memory = [[sim-launch-and-tour-method]]。正本 = HANDOVER.md 冒頭 2026-06-06 CC4。
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

### 6. ~~Task Hub~~ — **2026-05-23 廃止** ⚫
- **状況**: ⚫ **2026-05-23 廃止決定。** YD「マジで使わない」と明言、代替として **Project Agent Application** (`~/projects/project-agent-application/`) を新規開発。Salamat 内部運用も本アプリで完全置き換え予定。
- **詳細**: [[2026-05-23_TaskHub廃止_ProjectAgentApp移行]] / [[../archive/2026-05_TaskHub]]
- **遺産**: GitHub repo (`Yitao-Ding/salamat-task-hub`) + Firebase Hosting (`salamat-task-hub.web.app`) は当面放置 (削除しない、参考資料)
- **引き継ぎ**: 4 自立型エージェントのハーネス設計 / Discord ロールモデル / PWA 重視 / 寒色系 → 本アプリへ思想継承

### 7. Lecture Hub (個人ナレッジハブ)
- **状況**: 🟢 **Fable 5 全面改修 進行中 (2026-07-03 着手)** — ★ 新セッションはまず `~/projects/lecture-hub/HANDOVER.md` を Read
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
