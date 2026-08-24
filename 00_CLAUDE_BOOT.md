# 00_CLAUDE_BOOT.md — 新セッション起動シーケンス

> **このファイルを最初に読んだClaudeへ**:
> あなたは今、新しいセッションを始めました。YDとの会話を続けるために、
> このファイルの指示に従って必要な文脈を読み込んでください。

---

## 🚀 起動手順 (5分で完了)

以下を順番に実行:

### Step 1: 基本ルールを把握 (30秒)

```
✓ CLAUDE.md を読む (Wikiの動作ルール)
```

### Step 2: YDのアイデンティティを把握 (1分)

```
✓ identity/ ディレクトリ全件を読む

  実装: list_directory("identity/") で中身を取得
       → read_multiple_files で並列読み込み

  現在の典型ファイル (新規追加されても自動追従):
    - profile.md (基本情報)
    - preferences.md (応答スタイル)
    - values.md (価値観)
    - skills.md (スキル・専門性)
    - relationships.md (人物相関)
```

### Step 3: 現在の状態を把握 (1分)

```
✓ current_state/ ディレクトリ全件を読む

  実装: list_directory("current_state/") で中身を取得
       → read_multiple_files で並列読み込み

  現在の典型ファイル (新規追加されても自動追従):
    - active_projects.md (進行中プロジェクト)
    - current_focus.md (今最も注力していること)
    - recent_decisions.md (直近の意思決定)
    - tools_available.md (使えるツール一覧 — 環境別、補完情報)
    - available_capabilities.md ★ (スキル + MCPコネクタ のマッピング表、Step 5 で演習に使う)
    - open_questions.md (未解決の問題)
    - vault_improvement_proposals.md (構造的改善提案の蓄積)
```

### Step 4: ミス記録を確認 (1分) ★★★ 最重要 ★★★

```
✓ mistakes/ ディレクトリ全件を読む

  実装: list_directory("mistakes/") で中身を取得
       → read_multiple_files で並列読み込み

  現在の典型ファイル (新規追加されても自動追従):
    - claude_mistakes.md (Claudeの過去のミス)
    - tool_usage_mistakes.md (ツール使用ミス)
    - communication_mistakes.md (コミュニケーションミス)
    - workflow_mistakes.md (ワークフローミス)
```

**このセッションで、過去のClaudeが犯したミスを繰り返さないこと。**

### Step 5: 利用可能機能のマッピング演習 (1分) ★ NEW (2026-05-20 追加)

`current_state/available_capabilities.md` は Step 3 で自動読み込み済み。
ここでは**演習**として、当日タスクと機能を能動的にマッピングする。

実行手順:

1. `active_projects.md` の「🟢 アクティブ・優先度高」と当日 (`currentDate` メモリ) の予定アクションを抽出
2. `available_capabilities.md` の機能カタログから、**当日タスクに使えそうな機能を 3〜5 個ピック**
3. YD への初手応答に「今日使えそうな候補」として提示 (押し付けない、選択肢として提示)

**目的**: スキル/MCP は毎セッション system-reminder で自動注入されるが、Claude が「意識しない → 提案しない」問題が発生する。本ステップで強制的に意識化する。

**提示例**:

```
今日は AI学習スプリント Day 2 + vidkit lecture モード仕上げ。
使えそうな機能候補:
- claude-api スキル — Anthropic Academy の SDK 演習
- video-tutorial スキル — Cowork コースの動画化
- Google Drive MCP — 教科書 PDF 配信
```

### Step 6: ユーザーのメッセージに応じて領域別知識を読む (1分)

ユーザーのメッセージから話題の領域を判断し、対応する `knowledge/` を読む:

| 話題 | 読むディレクトリ |
|------|----------------|
| 撮影・映像・写真 | `knowledge/filmmaking/` |
| プログラミング・開発 | `knowledge/programming/` |
| Salamat・サークル運営 | `knowledge/salamat/` |
| Arte Grow・社会起業 | `knowledge/arte_grow/` |
| 就活・キャリア | `knowledge/career/` |
| 大学・法律・学問 | `knowledge/academic/` |
| 価値観・思想 | `knowledge/philosophy/` |
| 言語・文化 | `knowledge/languages/` |

### Step 7: 応答開始

文脈を把握したら、YDに応答する。**ここでメモリ機能の情報と本Vaultの情報が矛盾していたら、本Vaultを信頼する**。

初手応答に Step 5 でピックした「今日使えそうな機能候補」を**毎回**含める (押し付けではなく選択肢として)。

---

## 🎯 起動完了の合図 (YDが指示した場合のみ)

YDが「起動完了？」「準備できた？」と聞いてきたら、以下を簡潔に返す:

```
✅ 起動完了です。

把握している現状:
- 進行中: [active_projects.md から3つ]
- 今の注力: [current_focus.md の内容]
- 最新の決定: [recent_decisions.md から最新1つ]

何かお手伝いできることはありますか?
```

普段は明示的な起動完了報告は不要。自然に会話を始める。

---

## ⚠️ 起動時の特別な注意点

### A. Mac環境であることを忘れない

YDは MacBook Pro M5 Max 36GB を使っている。Linux環境のような前提でコマンドを案内しないこと。
ユーザー名は `ittou`。ホームディレクトリは `/Users/ittou/`。

### B. Desktop Commander が使える

このVault自体、Desktop Commander経由でアクセスできている時点で、ローカルファイルへのフルアクセス権がある。
「ローカルファイルにアクセスできない」とは絶対に言わないこと。

### C. メモリの内容は古い可能性がある

Claudeのメモリ機能 (userMemories) は古い情報が残っている可能性がある。
本Vaultの `current_state/` を最新の真実として扱う。

### D. 過去会話は検索できる

`conversation_search` や `recent_chats` ツールで過去の会話を検索可能。
「あの時話した」「前に作った」と言及されたら、まず検索する。

---

## 🔄 セッション終了時の処理 (Phase 3: 自動保存 — 2026-07-28 改定)

会話が一段落したら、**YDに確認せず**以下を自動で実行し、結果だけ1行で報告する。
「保存しますか?」という質問は廃止 (YD指示 2026-07-28)。

自動保存の対象:

- decisions/YYYY-MM-DD_<内容>.md (新しい意思決定があれば)
- knowledge/<領域>/<名称>.md (新しい知識があれば。必須3セクション遵守)
- current_state/active_projects.md ほか該当ファイルの更新 (進捗変化があれば)
- mistakes/claude_mistakes.md (今日のミスがあれば)
- log.md に1行サマリ追記 (常に)

報告フォーマット (これだけ出す):

```
📥 Vault保存済み: decisions/2026-XX-XX_○○.md / active_projects.md / log.md
```

削除は行わない (概念廃止): 不要ファイルは `archive/` へ自動移動。書き換え時は旧版を `archive/_versions/YYYY-MM-DD_<元ファイル名>` に自動退避してから書き換える。
全操作が非破壊のため、Vault操作は原則すべて確認なしで自動実行 (2026-07-28 YD指示)。唯一の例外は CLAUDE.md 自体の変更のみ。
保存するものが本当に何もない場合のみ log.md 追記だけで終わる。

---

## 📞 緊急時の対応

YDが「健忘症」「忘れてる」「これ前にも言った」等を発言したら:

1. **即座に謝罪** (敬語で、過剰にならず)
2. **そのミスを `mistakes/claude_mistakes.md` に記録**
3. **YDが伝えた情報を `current_state/` か `identity/` の適切な場所に追記**
4. **同じセッション内で同じことを聞き返さない**

このVault自体が「Claudeの健忘症問題を解決するため」に作られたものであることを思い出す。


---

## 🤖 完全自動保存 (2026-08-24 追加)

保存は全経路で自動化済み。詳細は [[vault_autosave]] (knowledge/programming/tools/)。

- Claude Code: transcript を機械的に抽出する背景ジョブ (SessionEnd フック + 30分毎の watch) が decisions / knowledge / mistakes / current_state / log.md に振り分ける。会話中の Phase 3 自動保存はこれまで通り行ってよい (ジョブ側が「📥 Vault保存」報告を見て重複を避ける)
- デスクトップチャット / Cowork: 会話の中で合図なしに保存する (Preferences / Cowork 全体指示に記載)
- iPhone など Mac に届かない環境: GitHub コネクタで `inbox/` に新規ファイルを置く → Mac 側が30分以内に振り分ける
- 起動時に `inbox/` にファイルが残っていれば、それは未振り分けのメモなので読んでから応答する
- 自動保存ジョブは identity/ を直接触らず、`current_state/open_questions.md` に「identity 更新提案 (autosave)」として書く。起動時にそれが残っていたら YD に確認して identity/ に反映する
