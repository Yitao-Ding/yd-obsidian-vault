---
type: course_log
course_id: 05
course_title: Claude 101
course_title_ja: Claude 101
category: Foundations
difficulty: Beginner
priority: 高 (基礎)
status: in_progress
recommended_day: Day 2 (2026-05-20)
started_at: null
completed_at: null
cert_url: null
last_updated: 2026-05-20
tags: [anthropic-academy, claude, foundations, beginner, ai-fluency, 4d-framework]
---

# 05. Claude 101 (Claude 入門)

> ⚠️ **ファイル名注意**: ファイル名は `05_claude_code_101.md` だが、本コースは「**Claude 101**」(Claude Code ではない)。
> Claude Code に特化したコースは別途「Claude Code 101」(コースID 06 想定) として存在する。
> 本ファイルは「Claude 101」(基礎コース / Foundations カテゴリ) を扱う。
> 出典: https://anthropic.skilljar.com/claude-101

## 一言サマリ

Claude の Chat / Cowork / Code を初めて触る人向けの土台コース。Projects・Artifacts・Skills・Connectors を最短ルートで覚え、AI Fluency の **4D framework** (Delegation / Description / Discernment / Diligence) で「AIに任せるかどうか」を判断する型を仕込まれる。

## 学ぶ前の前提知識 (3行)

- Claude の Web/デスクトップアプリでチャットしたことがあれば十分。コーディング知識は不要。
- ファイルアップロード、コピー&ペースト、ブラウザ操作ができればOK。
- 4並列 Claude Code をすでに運用している YD にとっては「基礎の言語化」と「公式の語彙合わせ」がメインの収穫。

## このコースで答えが出る問い (5問)

1. **Projects と Artifacts の使い分けは何が公式の推奨か？** (自分の運用との差分を見たい)
2. **Skills と Connectors はどこで線引きするのか？** (Cowork とどう繋がる？)
3. **AI Fluency 4D の「Delegation」基準は具体的に何を見ているのか？** (タスクをClaude に振るかどうかの判断軸)
4. **Enterprise search / Research mode はいつ使うべきか？** (Vault検索との競合関係)
5. **役割別ユースケース (PM / Engineer / Sales / 等) で Claude をどう型化しているのか？** (Salamat WBS のドキュメント生成に転用したい)

## 主要キーコンセプト (5個)

### 1. Projects
プロジェクト単位で「会話履歴 + 共有ドキュメント + カスタムインストラクション」を束ねる入れ物。
ファイルやWebをコンテキストとして固定でき、毎回貼り直す手間が消える。
YD の Vault 運用 (`identity/` `current_state/` を毎回読ませる) は、これを手動でやっているのと同じ。

### 2. Artifacts
コードや長文ドキュメントをチャットから分離して右ペインに表示する仕組み。
反復編集ができ、HTML/React/Mermaid 等は即プレビュー可能。
「動くプロトタイプを会話で作る」ループの主役。Lecture Hub のスライド試作に向く。

### 3. Skills
Claude に「うちのチームの手順」を覚えさせる再利用ユニット (Markdown + 補助ファイル)。
プラグインの最小構成要素。例: 「議事録 → タスク抽出」「PR説明文の社内テンプレ生成」。
Vault の `knowledge/programming/tools/` を Skill 化すれば、Claude Code 4並列が同じ手順で動く。

### 4. Connectors (Enterprise Search)
Google Drive / Notion / Slack / GitHub などの社外データに Claude を接続する公式コネクタ群。
YD 環境では Notion / Gmail / Calendar / Drive / Figma / Canva / Miro / Zoom が接続済み。
「Research mode」と組み合わせると複数ソース横断の調査が一発で出る。

### 5. AI Fluency 4D Framework (Delegation / Description / Discernment / Diligence)
Anthropic + 学術界が作った AI協働の思考フレーム。
- **Delegation**: 任せる/任せない/半々の見極め
- **Description**: 曖昧さを潰したプロンプト記述
- **Discernment**: 出力の良し悪しを評価する目
- **Diligence**: AI を使っても最終責任は人間、というスタンス
本業の意思決定 (Salamat 監修 / Arte Grow ROI判断) は Diligence 側に倒すべきものが多い。

## コース構成 (公式モジュール)

公式: https://anthropic.skilljar.com/claude-101

1. **Meet Claude** — Claude とは / 初対話 / 良い結果の出し方 / Chat・Cowork・Code の3形態
2. **Organizing Your Work and Knowledge** — Projects / Artifacts / Skills
3. **Expanding Claude's Reach** — Connectors / Enterprise Search / Research mode
4. **Putting It All Together** — 役割別ユースケース / 修了証

所要: 約20〜30分 (Class Central / Yahoo Tech 記事より)。修了証あり。

## YDの今やってる仕事との接続点

| プロジェクト | 接続点 | 期待効果 |
|------|--------|---------|
| **Salamat WBSサイト** | Projects に「Salamat 公式ドキュメント + 監修ルール」を固定 → 毎回貼り直し不要 | 監修ループの工数1/3 |
| **Arte Grow** | Research mode で「ヤク高地観光トレンド + 競合スクール」横断調査 | 提案資料の下調べ時間短縮 |
| **Lecture Hub** | Artifacts で講義スライド試作 (Mermaid 図 + HTML プレビュー) | ライブ作業中に即見せる |
| **Task Hub** | Skills 化して「日次ログ→翌日タスク抽出」を再利用ユニットに | Vault Phase 2 移行の足場 |
| **vidkit** | Skills として「dance/lecture/autocut モード別の運用手順」を登録 | チームメンバ追加時のオンボ短縮 |
| **4並列 Claude Code 運用** | Projects + Skills + Connectors の組合せが、自分の「Vault憲法 + 起動時必須シーケンス」の公式版 | 語彙統一で textbook/ vol.2 が書きやすくなる |

## コース後にやる小さな練習問題 (3問、答え付き)

### Q1. Projects と「ファイルをチャットに毎回添付する運用」の最大の違いを2つ挙げる
**A**:
- (1) 会話履歴がプロジェクト内で連続する (チャット毎の文脈分断がない)
- (2) コンテキストファイルが永続化され、Claude 側のキャッシュ/最適化が効く

### Q2. Artifact が一番効くタスク／一番効かないタスクを1つずつ挙げる
**A**:
- **効く**: HTML/React/Mermaid のプロトタイプ ─ プレビューしながら反復編集できる
- **効かない**: 4並列セッションの状態管理のような「複数会話にまたがる継続状態」 ─ Artifact は1チャット内の生成物に閉じる。これは Projects または外部 Vault の仕事

### Q3. AI Fluency 4D で「Delegation を強める」場面と「Diligence を強める」場面を YD の仕事から1つずつ
**A**:
- **Delegation 強め**: vidkit の dance モードの代表フレーム選定 ─ 主観評価が不要、判定基準が明確、AIに任せて人間は最終承認だけ
- **Diligence 強め**: Salamat の監修判断 ─ 文化背景・現地関係性が絡み、AIの提案を鵜呑みにすると関係を毀損するリスク

## 受講ログ

```
受講開始: 
受講完了: 
合計所要時間: 
```

### セクションごとのメモ

### 💡 受講後に埋めるキーコンセプト
(動画後に自分の言葉で書き直す)

### 🛠 Claude Code で試したこと
(コース後に試した具体例)

## ✅ うまく行ったこと
(動画前は空欄。受講後埋める)

## ❌ 詰まったこと
(動画前は空欄。受講後埋める)

## 📋 次回同じことをするときのチェックリスト

事前準備 (動画見る前):
- [ ] Claude デスクトップアプリにログイン状態を確認
- [ ] 既存 Connectors の接続状況を一覧で把握 (Notion / Gmail / Calendar / Drive / Figma / Canva / Miro / Zoom)
- [ ] 「自分が現在使っている Projects」を1つ用意 (例: Salamat WBS)
- [ ] 受講中に止めて手を動かすための作業フォルダを開いておく

動画中に手を動かす:
- [ ] Module 2 で実際に Project を1つ作る → Vault の `identity/` を投げ込む
- [ ] Module 2 で Artifact を1つ生成 → HTML or Mermaid で
- [ ] Module 3 で Connector を1つ叩いて Research mode を試す (Notion 検索が早そう)
- [ ] Module 4 で「自分の役割に近いユースケース」を見て差分メモ

落とし穴・先回り:
- ⚠️ Skills と Cowork の Skills は「同じ Skills」── コース04 (Cowork入門) と同じ概念が出る。重複学習しない
- ⚠️ Enterprise Search は個人プランで使えない機能がある可能性 → 自分のプラン上限を最初に確認
- ⚠️ 4D framework は別コース「AI Fluency: Framework & Foundations」で深掘り。Claude 101 では概要止まり

## 🔗 関連

- 公式コース: https://anthropic.skilljar.com/claude-101
- AI Fluency 別コース (深掘り用): https://anthropic.skilljar.com/ai-fluency-framework-foundations
- AI Fluency Framework PDF: https://www-cdn.anthropic.com/334975cdec18f744b4fa511dc8518bd8d119d29d.pdf
- 全体マップ: [[README]]
- 同日ペア (Day 2): [[04_introduction_to_claude_cowork]]
- 次の進路: [[06_claude_code_101]] (Claude Code に特化) / [[10_introduction_to_agent_skills]] / [[11_introduction_to_subagents]]
- YDの本業接続: [[active_projects]] / [[tools_available]] / [[claude_mistakes]]

## 📚 情報源

- [Claude 101 公式 (Anthropic Academy)](https://anthropic.skilljar.com/claude-101)
- [Class Central: Free Course Claude 101](https://www.classcentral.com/course/anthropic-academy-claude-101-536157)
- [Anthropic Academy 13 courses ranked 2026 (Spectrum AI Lab)](https://spectrumailab.com/blog/anthropic-academy-13-free-courses-ranked-2026)
- [AI Fluency: Delegation (Anthropic)](https://www.anthropic.com/ai-fluency/ai-fluency-delegation)
- [4D Framework redefines AI Fluency (AI for Leaders)](https://aiforleaders.com/how-anthropics-4d-framework-redefines-ai-fluency/)

> ⚠️ 推測・捏造ガード: モジュール構成・所要時間・4D framework の定義は上記の公式/二次情報を根拠にしている。具体的なクイズ問題や講師名は本ノートには含めていない (公式コース内で確認)。受講後の修正歓迎。
