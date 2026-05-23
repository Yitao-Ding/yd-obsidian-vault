# 2026-05-23 ai-researcher: relevance 緩和 + プロファイル拡充 + 死亡2ソース修復

> 種別: 意思決定 + 技術ノウハウ
> 関連: [[ai_researcher]] / [[active_projects]] / [[2026-05-19_API依存撤廃_Max20x完結化]]

## 背景

YD が「ai-researcher は問題なく情報を集められているか」と確認。稼働状況を調べたところ、
**収集パイプライン自体は正常稼働（launchd 3本生存、課金0、エラーログ0）だが、
集めた記事がほぼ Vault に残っていない**ことが判明（直近 collect で raw=76 → kept=0〜1 が連発）。

原因は3層に分かれていた:

1. **relevance threshold が厳しすぎる**: スコアは `score_one()` のキーワード重み合計で、
   high語=3.0 / medium語=1.5。threshold=3.0 は実質「high語1つ必須」の壁。
   直近キューで通過 0/36、threshold 直下（1.5〜2.9）に19件が滞留していた。
2. **interests.yaml が AI技術語に偏重**: high_priority が `claude` `agent` `mcp` `quantization` 等
   AI/LLM 用語ばかりで、YD の中核興味（撮影・映像、Web開発、フィリピン/国際協力、法律）の語が皆無。
3. **7ソース中2つが死亡**:
   - `papers_with_code`: paperswithcode.com が廃止され `huggingface.co/papers/trending` に
     リダイレクト（HTML を返すため `Expecting value` で JSON パース失敗）。
   - `github_trending`: クエリが0件を返し続けていた（後述の GitHub Search 2大制約）。

## 選択肢と決定

3層すべてに対処。スコープは「死亡2ソース修復まで（C）。新ジャンルのソース追加は別タスク」で握った。

### A. threshold 緩和 + 1回あたり上限
- `config.yaml`: `min_relevance_score: 3.0 → 1.5`（medium語1つで通過可能に）
- `com.yd.ai-researcher.collect.plist`: `collect --max-articles 8` を追加 → `launchctl unload/load` で反映
- 効果: 直下の19件が拾えるようになり、cap 8 で量と Max 20x 枠の消費を制御

### B. interests.yaml 拡充
- high_priority に16語追加: `davinci` `cinematography` `カラーグレーディング` `filmmaking` `映像制作`
  `next.js` `nextjs` `vercel` `jica` `フィリピン` `philippines` `social business` `社会起業` `国際協力` 等
- medium_priority に24語追加: `ffmpeg` `prores` `gimbal` `react` `firebase` `supabase` `tailwind`
  `typescript` `frontend` `design` `startup` `起業` `法学` `legal tech` `frontier` `辺境` 等
- **注意**: `ui`/`ux`/`ec`/`ngo` 等の短い語は部分一致事故（"build" に "ui" がマッチ等）を起こすため意図的に除外

### C. 死亡2ソース修復
- **papers_with_code → hf_papers**: `src/sources/papers_with_code.py` の中身を
  HF daily_papers API（`https://huggingface.co/api/daily_papers?limit=N`、クリーンな JSON、arXiv裏付き）に差し替え。
  `name` を `hf_papers` に変更。`config.yaml` の `collection` キーと `interests.yaml` の `source_boost` も `hf_papers` に更新。
  （ファイル名は git 履歴のため papers_with_code.py のまま、ヘッダコメントで明記）
- **github_trending**: topic 検索を廃止し、キーワード + `pushed:>=` + `stars:>=50` 検索に変更。

## 結果（Before / After）

| 指標 | Before | After |
|------|--------|-------|
| 稼働ソース | 7中5（hf/github死亡） | **7中7** |
| relevant / run | **0〜1** | **46**（dry-run実測、cap 8 で kept は最大8） |
| github_trending | 0件 | 20件（langchain, transformers, anthropics/skills 等） |
| hf_papers | 死亡 | 15件（スコア10点が上位独占） |

次回 collect（HH:03 JST）から新設定で稼働。

## ✅ うまく行ったこと
- **段階的な切り分けが奏功**: 「collect は動いてるが kept が0」→ ログの `raw/dedup/relevant` 内訳で
  dedup（正常）と relevance（犯人）を分離 → スコア分布で「threshold直下に19件滞留」を定量化 → 原因確定。
- HF daily_papers API が paperswithcode の素直な後継だった（arXiv ID ベース、JSON、認証不要）。差し替えがスムーズ。
- github_trending は **rate limit ではなかった**（remaining=10 でも0件）。生クエリの total_count 比較で
  「topic + 日付 = 0」「OR 7個 = 422」の2つの GitHub Search 制約を切り分けられた。

## ❌ 詰まったこと
- **早合点**: 当初「google_research の RSS が止まっている疑い」と発言 → 実際は15件取得できており、
  threshold で全部落ちていただけだった（会話内で即訂正）。`raw/research/` のディレクトリが疎らなのを見て
  「取得できていない」と推測したのが誤り。**kept された記事しかファイル化されない**ので、ディレクトリの
  疎らさ=取得失敗ではない。
- **GitHub Search の2大制約でハマった**:
  1. `topic:llm` 単体は 68,046 件あるのに `topic:llm created:>=<date>` は 0 件
     → topic フィルタは created/pushed の日付 qualifier と組み合わせると効かない。
  2. キーワードを OR で8個繋いだら 422 HTTPError → **AND/OR/NOT は最大5個**（qualifier は対象外）。
  どちらも tenacity の RetryError に包まれて出るため、生クエリの status_code を直接見るまで原因が見えなかった。
- 自分のデバッグ用クエリ連打で search rate limit（無トークン10/分）を一時枯渇させ、切り分けを混乱させた。

## 📋 次回同じことをするときのチェックリスト
- ai-researcher が「集めてるのに残らない」時は、まず `logs/<date>.log` を
  `grep "raw=\|dedup\|relevance: kept"` で内訳確認 → dedup と relevance のどちらで落ちてるか切り分け。
- スコア分布を見たい時は `dry-run` コマンド（top10のみ）か、`score_one()` を全 deduped に当てる小スクリプト。
- ソースが0件の時は **fetch 内の例外は握り潰される**ので、`requests` で生 URL を直接叩いて
  `status_code` / `content-type` / `final_url`（リダイレクト）/ `body[:200]` を見る。
- GitHub Search を使う時の鉄則: ①AND/OR/NOT は最大5個 ②topic フィルタと日付 qualifier は併用しない
  ③無トークンは 10リクエスト/分（デバッグ連打に注意）。
- threshold を変えたら必ず `dry-run` で relevant 件数を確認してから本番に乗せる。
- 外部 API が死んでいたら、まず生で叩いてリダイレクト先を確認（後継サービスのヒントになる）。

## 残課題（別タスク C+）
- `github_trending` の `stars:>=50` は定番大型リポジトリ（transformers 16万star等）が毎回上位に来る。
  トレンド性を上げるなら `stars:50..5000` の上限を検討（初回以降は dedup で除外されるので緊急性は低い）。
- 新ジャンルのソース追加（撮影系・Web開発系・国際協力系の RSS）。これが無いと B で足した
  撮影/フィリピン/法律の語は「ソースに記事が流れてこない」ので発火しない。**ソース選定は YD と方向性を握ってから**。
- `.env` の `GITHUB_TOKEN` 設定（無くても10/分で足りるが、設定すれば5000/時で余裕）。PAT 発行は YD 作業。
