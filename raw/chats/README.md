---
type: meta
created: 2026-05-19
last_updated: 2026-05-19
tags: [chat-history, archive, claude]
---

# raw/chats — 過去会話アーカイブ

> Claude (デスクトップ・モバイル) との過去会話を保存する場所。
> Claude の `conversation_search` でいつでも検索できるが、ここに保存することで
> Obsidian でリンク・タグ・グラフビューで横断できるようにする。

## 構造

```
raw/chats/
├── README.md              # このファイル
├── _index/                # 全体インデックス・メタデータ
│   ├── all_chats.md       # 全会話一覧 (URL + タイトル + 日付)
│   ├── by_month.md        # 月別サマリ
│   └── stats.md           # 会話統計
├── by_date/               # 日付別ファイル (YYYY-MM-DD_<slug>.md)
├── by_theme/              # テーマ別 (programming / film / business / life)
└── by_project/            # プロジェクト別 (task-hub / wbs / vidkit / arte-grow)
```

## 1ファイルのフォーマット

```yaml
---
type: chat
date: 2026-04-10
url: https://claude.ai/chat/<uuid>
title: "<元のタイトル>"
project: task-hub
themes: [programming, firebase]
key_decisions:
  - "Vercel デプロイ採用"
  - "Firebase Auth で Google ログイン実装"
participants: [YD, Claude-Desktop]
---

# <タイトル>

## 要約
3-5行で何が決まったか・何を学んだか

## 主要な決定事項
- 決定1
- 決定2

## 主要なやり取り抜粋
> 会話の核心部分を引用 (Claude 検索のスニペットから)

## 関連リンク
- [[active_projects]]
- [[該当する knowledge ファイル]]
- 元 URL: https://claude.ai/chat/<uuid>
```

## インポート方針 (C案ハイブリッド)

1. **全件保存** (`by_date/`): デスクトップ Claude の `conversation_search` で取得した
   会話を、要約 + 元 URL + 主要やり取りで保存
2. **テーマ別再構成** (`by_theme/`): プログラミング / 映像 / Salamat / 就活 など
   横断テーマで束ねる
3. **プロジェクト別** (`by_project/`): Task Hub / WBS / vidkit など、active_projects
   と紐づけ

## 進め方

過去会話インポートはターン外で動けない (デスクトップ Claude の制約) ため、
**会話のたびに1ステップずつ** 進める。YD が話を振るたびに関連会話を
`conversation_search` で引いて、新規ファイルとして保存する。

主要会話の優先順位:
1. アクティブプロジェクト (Task Hub, WBS, vidkit, lecture-hub) の構築会話
2. 重要な意思決定 (FCPXML 採用、Lecture Hub 個人用転換、Max 20x 完結化など)
3. Salamat 関連の歴史 (フィリピン視察、政府交渉、組織運営)
4. その他 (life decisions, philosophy 系)

## 関連

- [[../../00_CLAUDE_BOOT]] — Claude 起動時のシーケンス
- [[../../current_state/active_projects]] — 現在進行中のプロジェクト
- [[../../knowledge/programming/tools/obsidian_vault]] — Vault 全体運用
