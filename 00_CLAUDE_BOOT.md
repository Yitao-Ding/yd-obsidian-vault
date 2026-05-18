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
✓ identity/profile.md (基本情報)
✓ identity/preferences.md (応答スタイル)
✓ identity/values.md (価値観)
✓ identity/skills.md (スキル・専門性)
✓ identity/relationships.md (人物相関)
```

### Step 3: 現在の状態を把握 (1分)

```
✓ current_state/active_projects.md (進行中プロジェクト)
✓ current_state/recent_decisions.md (直近の意思決定)
✓ current_state/current_focus.md (今最も注力していること)
✓ current_state/tools_available.md (使えるツール一覧)
✓ current_state/open_questions.md (未解決の問題)
```

### Step 4: ミス記録を確認 (1分) ★★★ 最重要 ★★★

```
✓ mistakes/claude_mistakes.md (Claudeの過去のミス)
✓ mistakes/tool_usage_mistakes.md (ツール使用ミス)
✓ mistakes/communication_mistakes.md (コミュニケーションミス)
```

**このセッションで、過去のClaudeが犯したミスを繰り返さないこと。**

### Step 5: ユーザーのメッセージに応じて領域別知識を読む (1分)

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

### Step 6: 応答開始

文脈を把握したら、YDに応答する。**ここでメモリ機能の情報と本Vaultの情報が矛盾していたら、本Vaultを信頼する**。

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

## 🔄 セッション終了時の処理

会話が一段落したら、以下をYDに提案:

```markdown
今日決まったこと・学んだことを Vault に保存しますか?

- [ ] decisions/YYYY-MM-DD_<内容>.md (新しい意思決定)
- [ ] knowledge/<領域>/<名称>.md (新しい知識)
- [ ] current_state/active_projects.md 更新 (進捗変化)
- [ ] mistakes/claude_mistakes.md (今日のミス記録)
- [ ] log.md に1行サマリ追記
```

YDが「お願い」と言ったら、Claudeが自分で書き込む。
YDが「いい」と言ったら、ログだけ追記して終わる。

---

## 📞 緊急時の対応

YDが「健忘症」「忘れてる」「これ前にも言った」等を発言したら:

1. **即座に謝罪** (敬語で、過剰にならず)
2. **そのミスを `mistakes/claude_mistakes.md` に記録**
3. **YDが伝えた情報を `current_state/` か `identity/` の適切な場所に追記**
4. **同じセッション内で同じことを聞き返さない**

このVault自体が「Claudeの健忘症問題を解決するため」に作られたものであることを思い出す。
