# wiki/index.md — Vault 全体の索引・ハブページ

> Vault に蓄積されたノートを横断的に見渡すためのハブ。
> Dataview クエリを多用して、自動更新の動的インデックスとして機能させる。
>
> 最終更新: 2026-05-18

---

## 🧭 主要エリア

### コア (Claude の外部記憶)
- [[CLAUDE]] — エントリポイント
- [[00_CLAUDE_BOOT]] — 起動時必読
- [[claude_mistakes]] — ミス記録の親

### アイデンティティ
- [[profile]] / [[values]] / [[preferences]] / [[skills]] / [[relationships]]

### 現状
- [[active_projects]] / [[current_focus]] / [[recent_decisions]] / [[open_questions]] / [[tools_available]]

### 知識領域
- [[filmmaking/index|🎬 Filmmaking]]
- [[programming/index|💻 Programming]]
- [[salamat/index|🍽 Salamat]]
- [[arte_grow/index|🎨 Arte Grow]]
- [[career/index|💼 Career]]
- [[academic/index|📚 Academic]]
- [[philosophy/index|🧠 Philosophy]]
- [[languages/index|🌐 Languages]]

---

## 📊 動的ビュー (Dataview)

> 以下のクエリは Dataview プラグイン有効化後に動作します。
> プラグイン未導入の状態ではコードブロックがそのまま表示されます。

### 最近編集されたノート (上位10件)

```dataview
TABLE file.mtime AS "更新"
FROM "."
WHERE file.name != "index"
SORT file.mtime DESC
LIMIT 10
```

### 進行中プロジェクト

```dataview
LIST
FROM "current_state"
WHERE contains(file.name, "active")
```

### 未解決の問い

```dataview
LIST
FROM "current_state/open_questions"
```

### 直近の決定 (decisions ディレクトリ)

```dataview
TABLE file.ctime AS "作成"
FROM "decisions"
SORT file.ctime DESC
LIMIT 10
```

---

## 🔖 タグ別索引

```dataview
TABLE length(rows) AS "件数"
FROM ""
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows) DESC
```

---

## 📝 メモ

このページは Vault 全体のハブとして育てていく。
プラグイン (Dataview / Templater) 導入後に真価を発揮します。
