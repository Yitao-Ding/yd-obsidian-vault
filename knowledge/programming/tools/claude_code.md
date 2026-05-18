---
type: knowledge
domain: programming/tools
created: 2026-05-18
last_updated: 2026-05-18
tags: [claude-code, ai-tools, workflow, permissions, prompt-design]
priority: high
---

# Claude Code — セッション運用ノウハウ

> Claude Code (CLI / IDE 拡張) を YD のワークフローで使うための実践メモ。
> Vault そのものの運用は [[obsidian_vault]] を参照、こちらは Claude Code 側の話。
>
> 初版執筆: 2026-05-18 (本 Vault の構築直後)

## 🎯 Claude Code とは

Anthropic 公式の対話型 CLI / IDE エージェント。
- ターミナルから `claude` で起動
- ファイル編集 (Read/Edit/Write)、Bash 実行、Git/gh 操作、Web 検索、サブエージェント生成などを一気通貫で扱う
- desktop アプリの Claude と違って「現在のディレクトリで動く開発エージェント」

## 🔐 権限モードの3パターン

危険操作 (rm、git push --force など) を勝手に走らせないよう、ツール呼び出しごとに人間の承認を要求する。承認の出し方が以下の3つ。

### モード1: 通常モード (デフォルト)

- 毎回プロンプトで承認を求める
- 安全、ただし大規模タスクだとプロンプト疲れになりがち
- 業務系ファイル / 共有リポジトリを触る時はこれが基本

### モード2: settings.json の許可リスト方式 (推奨)

`~/.claude/settings.json` または `<project>/.claude/settings.local.json` で事前ルールを定義する。

```json
{
  "permissions": {
    "allow": [
      "Bash(git:*)",
      "Bash(mkdir:*)",
      "Bash(cp:*)",
      "Bash(gh repo create:*)",
      "Write(~/ObsidianVault/**)",
      "Edit(~/ObsidianVault/**)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(sudo:*)"
    ]
  }
}
```

- パターンマッチで「これは自動 OK」「これは絶対 NG」を事前定義
- 安全と効率のバランスが最も良い
- **今回の Vault 構築はこの方式で進めた** → ほぼプロンプトなしで Step 1-13 を完走できた
- `deny` を一緒に書いておくと、うっかり広めの `allow` でも破壊操作だけは止まる安心感がある

### モード3: `--dangerously-skip-permissions`

- `claude --dangerously-skip-permissions` で起動
- すべてのツール呼び出しを無条件承認
- メリット: 究極のノンストップ
- リスク: 誤動作・破壊的操作も止まらない (rm -rf 系も通り得る)
- 使い所:
  - 使い捨ての sandbox (Docker / 一時 VM / Vercel Sandbox)
  - CI のように人がいない環境
  - 「失敗してもロールバックできる」と確信できる単発作業
- 通常開発では避ける。Vault 構築のような「ホームディレクトリの新しい領域に対する純粋な追加作業」程度なら可だが、それでも settings.json 方式の方が事故が起きにくい

## 📝 大規模構築タスクのプロンプト設計

今回 (Vault 構築) を例に、効くパターンを整理。

### 1. 確定方針を先頭にブロックで明示する

- 規模・分離方針・同期方式・認証情報・トーンを冒頭で固める
- 「規模C、機密分離なし、Git + GitHub Private (yd-obsidian-vault)、敬語」のように1行ずつ列挙
- Claude が途中で迷う・聞き返す回数が激減する

### 2. 手動操作が必要なポイントを明示する

- Vault 構築では Step 8 (Obsidian 起動) と Step 9 (プラグイン導入) が手動
- 「この Step で止まる」と書くと Claude 側もそこで一旦待つ実装になり、勝手に進まなくなる

### 3. 進捗報告フォーマットを統一する

- `✅ Step N/M 完了: 内容` のように決め打ち
- ターミナルでスクロールしても何が終わったか目で追える
- TaskCreate / TaskUpdate を組み合わせると IDE 側でも進捗バーが見える

### 4. 「判断ポイントは聞いていい」と明示する

- 「判断に迷う点は遠慮なく質問してください」の一文を入れる
- これがあると Claude が AskUserQuestion を能動的に使ってくれる
- 今回は Step 10 の「プラグインバイナリを Git に含めるか」で発動 → 1.6MB のバイナリを除外する判断を YD と一緒に決められた

### 5. 指示書を別ファイルに切り出す

- プロンプト内に全部書くより `~/Downloads/<project>/99_CC_実行指示.md` のような別ファイルにする
- 「このファイルを読んで、書いてある通りにやって」のメタ指示にできる
- 再利用・修正が楽 (Vault 構築指示書はそのまま再利用 = 別 Mac でも同じ Vault を再現可能)

## 💪 今回の構築で見えた強み

1. **並列ツール呼び出しで時短** — 初期チェックで Read 1+ Bash 4 を同時投げ → 数秒で全状況把握
2. **JSON / Markdown の一括生成** — 7個の `.obsidian/*.json` を1ターンで書き切れる
3. **能動的なベストプラクティス提案** — `.gitignore` のプラグインバイナリ除外を「これ含めますか?」とこちらから確認してきた
4. **進捗管理を TaskCreate / TaskUpdate で可視化** — 13ステップを完全に追跡
5. **手動操作待ちで素直に止まる** — Step 8/9 で「開いた」「入れ終わった」のシグナルを待つ実装が綺麗
6. **system-reminder で外部変更を検知できる** — Obsidian 起動後に `.obsidian/core-plugins.json` が自動更新された事を、Edit ツールが教えてくれた (= 衝突回避できる)

## ⚠️ 今回の構築で見えた弱み・引っ掛かりポイント

1. **Edit は事前 Read が必須** — `~/.zshrc` 編集時に `File has not been read yet` で1回詰まった。詳細は [[claude_mistakes]] A-4
2. **既存ファイルの上書き判断は文脈勝負** — `~/ObsidianVault` が「ディレクトリ枠だけ存在」していた時、上書き確認を出すかどうかは中身ゼロを確認してスキップ判定した。中身があれば確認すべき
3. **エイリアスは Claude Code の Bash からは効かない** — `vsync` は `.zshrc` を読んだインタラクティブシェルで効くもの。Claude Code 側で「vsync 実行して」と言われたら alias の中身 (`cd ~/ObsidianVault && git add . && git commit -m "..." && git push`) を直接打つ必要がある
4. **Obsidian 自身が `.obsidian/` 配下を書き換える** — `core-plugins.json` などは Obsidian 起動時に自動更新される。git で追跡しても恒常的に diff が出るので、人間が手で固定したい設定は最小限に絞るのが現実的

## 🔄 セッション終わりの習慣 (Phase 1 運用)

区切りが良いところで YD が「Vault に保存して」と頼む流れ。保存内容は4種類に分けると整理しやすい:

| 種類 | ファイル | 何を書くか |
|------|---------|---------|
| 時系列ログ | `log.md` | その日の操作・出来事を append-only で |
| ミス記録 | `mistakes/claude_mistakes.md` | 同じ過ちを次のセッションで防ぐため |
| 意思決定 | `decisions/YYYY-MM-DD_<内容>.md` | 「なぜそう決めたか」の保存 |
| 知識 | `knowledge/<領域>/<名称>.md` | 再利用したいノウハウ |

最後に `vsync` で GitHub に push。

## 🧰 Claude Code でよく使うツール (今回ベース)

- **Read / Edit / Write** — ファイル系。Edit は同一会話内で対象ファイルを Read 済みであることが前提
- **Bash** — シェル実行。`run_in_background: true` で長時間ジョブを非同期化できる
- **TaskCreate / TaskUpdate / TaskList** — タスク管理。大規模工程の進捗可視化
- **AskUserQuestion** — 判断ポイントで選択肢提示 (preview 付きの単一選択 / 複数選択)
- **Skill** — `/skill-name` で呼ぶ。今回は使わなかったが、Vercel 系 / UI/UX / Final Cut Pro オートカットなどが利用可能
- **Agent** — サブエージェント生成。並列調査や独立タスク向け、コンテキスト節約に有効
- **ToolSearch** — 多数の deferred ツール (Gmail / Notion / Figma / Calendar など MCP 連携) を必要時にロード

## 📚 関連

- [[obsidian_vault]] — Vault 運用マニュアル (情報の保存ルール、運用フェーズ)
- [[claude_mistakes]] — Claude の過去ミス記録 (Edit 前 Read 必須は A-4)
- [[2026-05-18_Obsidian_Vault構築完了]] — 本 Vault の構築意思決定
- [[tools_available]] — 接続中ツール一覧

## 🌐 外部参考

- Anthropic 公式: Claude Code ドキュメント (`claude --help` から辿れる)
- `~/.claude/settings.json` のスキーマ — `permissions.allow` / `deny` のパターン記法

## 📝 このノートの更新タイミング

- 新しい権限モード / Skill が登場した時
- プロンプト設計の新パターンを発見した時
- Claude Code の重大な仕様変更があった時
- 半年に一度の見直し
