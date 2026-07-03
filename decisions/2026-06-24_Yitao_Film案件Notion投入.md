# 2026-06-24 Yitao Film 案件管理Notion 実案件全件投入

作成: 2026-06-24 / 実行: Claude Code
引き継ぎ書: `knowledge/filmmaking/yitao-film-notion-handoff.md`
ホーム: https://app.notion.com/p/388cf01b1dc581eb8418d4b6ea8c08ab

## 状況
chat側で構築済みの Notion「Yitao Film｜案件管理ダッシュボード」(3DB: 👤クライアント / 🎬案件 / ✅タスク) に、YDの実案件を全件投入するタスク。引き継ぎ書の手順に従い Notion MCP (notion-create-pages / notion-update-page / notion-fetch) で実行。

## やったこと
- クライアント**7件** → 案件**10件** → タスク**84件** を「クライアント→案件→タスク」の順で作成、毎回URLを引き継いで relation リンク。
- 動画5案件(案件1〜5) + 写真5案件(案件6/7a/7b/8/9)。
- 案件3の案件名は YD指定どおり **「ハイミ「蛹」」** で確定。
- 案件5(大型イベントOP)は主催クライアント名未確定のためクライアント未リンク・報酬¥70,000で投入。
- ホーム末尾に「🧩 新規案件テンプレ」トグルを追記。

## 要確認(YDへ、6点)
1. 案件2(長野インタビュー)の納品日 — 未設定。確定でタスク締切を逆算。
2. 案件5(大型イベントOP) — (a)撮影要否(不要なら④撮影タスク削除) (b)主催クライアント名(作成しリンク)。
3. 案件7(キリア) — 写真(7a)/動画(7b)を別案件で作成。統合希望なら統合。納品希望日も未定。
4. 写真5人目 — 氏名未提供で保留。
5. 各クライアント区分 — 写真勢は暫定「アーティスト」。
6. キリアの打ち合わせ内容 — 本文に記入欄だけ作成済。

## ✅ うまく行ったこと
- 引き継ぎ書がID・スキーマ・入力フォーマット・実データ全部入れだったため、Claude Code 単体で迷わず完走できた(エラー0)。
- 投入前に `notion-fetch` で3DBのライブ schema(プロパティ名・select選択肢の完全一致)を実機照合 → 入力フォーマットのハマりどころ(日付の展開キー `date:納品日:start`、チェックボックス `__YES__`/`__NO__`、relation の JSON配列文字列)を事前確定できた。
- 1案件=1回の notion-create-pages にタスクを `pages` 配列でまとめ、案件単位の並列呼び出し(5本/メッセージ)で高速化。relation は同一バッチ内で同じ案件URLを共有させると安全。
- 引き継ぎ書 §7 のトグルテンプレは閉じタグが typo(`</summary>` 二重)だったが、`notion://docs/enhanced-markdown-spec` を読んで正しい `<details><summary>…</summary>…</details>` に直して投入 → 正常にトグル描画を fetch で確認。

## ❌ 詰まったこと
- 進捗率/タスク数ロールアップは notion-fetch の出力では `<omitted />` 表示になり、一見「未計算?」と紛らわしい。実際は relation が通っていれば Notion UI 側で自動計算される(fetchツールの表示仕様)。判断材料はロールアップ値ではなく「タスクrelationが案件に紐付いているか」。
- 案件IDは auto_increment で、サンプルデータ(YF-1〜3)の続きから採番された(案件1=YF-4 …)。サンプル未削除なので番号がズレて見えるが正常。YD手動削除後も既存IDは詰め直されない。

## 📋 次回同じことをするときのチェックリスト
1. 先に `notion-fetch` で対象DB(data_source)の **ライブ schema を実機照合**(プロパティ名・select選択肢は絵文字+スペースまで完全一致が必須)。
2. 入力フォーマットの3点を最初に確定: **日付=展開キー** `date:<名>:start` + `:is_datetime`、**チェックボックス=`__YES__`/`__NO__`**、**relation=対象ページURLのJSON配列文字列** `"[\"https://app.notion.com/p/<id>\"]"`。
3. 作成順は **クライアント→案件→タスク**。各ステップで返却URLを控え、次のrelationに渡す。
4. タスクは案件単位で `pages` 配列にまとめて1コール。複数案件は並列コールで速いが、各コール内は同一案件で揃える。
5. Notionのトグルは `<details><summary>タイトル</summary>本文</details>`(summaryは1つ)。迷ったら `notion://docs/enhanced-markdown-spec` を読む。
6. ロールアップ検証は値そのものでなく relation 紐付けで確認(fetchは `<omitted />`)。
7. 本案件群は Yitao Film。Arte Grow / やることDB への追加は対象外。

## 関連
- `knowledge/filmmaking/yitao-film-notion-handoff.md` (引き継ぎ書・正本)
- [[active_projects]] (Yitao Film｜案件管理ダッシュボード セクション)
