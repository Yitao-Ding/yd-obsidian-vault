---
type: knowledge
domain: programming/tools
created: 2026-05-18
last_updated: 2026-05-19
tags: [claude-code, ai-tools, workflow, permissions, prompt-design, model-config]
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

## 🧠 モデル設定 (model / context window)

Claude Code でどの Claude モデルを使うかは4つの方法で指定できる。優先度順に:

1. **セッション中**: `/model <alias|name>` で即時切替 (`/model` 単独で picker)
2. **起動時**: `claude --model <alias|name>` (そのセッションだけ)
3. **環境変数**: `export ANTHROPIC_MODEL=<alias|name>` (そのシェル以下)
4. **settings.json**: `"model": "<alias|name>"` (永続、最も汎用)

### モデルエイリアス

| エイリアス | 中身 |
|----------|------|
| `opus` | 最新 Opus (現状 4.7) |
| `sonnet` | 最新 Sonnet (現状 4.6) |
| `haiku` | 最新 Haiku |
| `opusplan` | Plan モードで Opus、実装で Sonnet に自動切替 |
| `opus[1m]` | Opus に 1M token context window |
| `sonnet[1m]` | Sonnet に 1M token context window |

### 1M context window の有効化

- **書き方**: alias または full model name に `[1m]` を付ける
  - 例: `/model opus[1m]` / `/model claude-opus-4-7[1m]`
- **要件**: Claude Code v2.1.111 以上
- **料金**: Max/Team/Enterprise なら追加料金なし(サブスク込み)。Pro はクレジット消費。Anthropic API は従量
- **品質低下に注意**: "lost-in-the-middle" 現象があるので `/context` で監視し、60% 超えで `/compact` 推奨
- **無効化**: `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` で picker から消える

YD のデフォルトは `~/.claude/settings.json` で `"model": "opus[1m]"` に固定 (2026-05-19 設定)。プロジェクト側 `.claude/settings.json` で別モデルが指定されている場合はそちらが優先される。

### effort level (Opus 4.7)

Opus 4.7 は `low / medium / high / xhigh / max` の5段階。YD は `xhigh` (デフォルト) で運用中。`/effort` で変更、`max` はそのセッション限り。

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

---

## ✅ うまく行ったこと

### Vault 構築タスクで効いたこと

- **`settings.json` の `permissions.allow` 事前整備** — `Bash(git:*)` `Bash(mkdir:*)` `Bash(cp:*)` `Write(~/ObsidianVault/**)` `Edit(~/ObsidianVault/**)` を許可リストに入れただけで、Step 1-13 で承認プロンプトはほぼゼロ。`--dangerously-skip-permissions` を使わず安全 + 速度を両立できた
- **指示書を別ファイル化** — `~/Downloads/obsidian-vault-setup/99_CC_実行指示_フル装備.md` を渡すだけで構築開始。プロンプト本体は「これを読んで通りにやって」のメタ指示にできた。修正・再利用が楽
- **TaskCreate で13ステップ可視化** — IDE 側でも進捗バーが見える。長いセッションでも何がどこまで進んだか一目で分かった
- **並列ツール呼び出し** — 初期状況把握で Bash を 4-5 本同時投げ → 数秒で全環境確認完了
- **AskUserQuestion を能動的に発動** — Step 10 でプラグインバイナリ (1.6MB) の追跡可否を確認 → 除外する判断を YD と共有して決定。事後の後悔を回避できた
- **手動操作待ちが綺麗に止まる** — Step 8/9 で「開いた」「入れ終わった」のシグナル待ちが明確で、勝手に走らなかった
- **system-reminder の外部変更通知** — Obsidian 起動後に `.obsidian/core-plugins.json` が自動更新された事を即時検知。衝突を避けながら作業継続できた

## ❌ 詰まったこと

### 仕様の踏み外し

- **Edit ツールは事前 Read 必須** — `~/.zshrc` 編集時に「File has not been read yet」で1回失敗。同一会話内で対象ファイルを Read 済みでないと Edit / Write は通らない。詳細は [[claude_mistakes]] A-4
- **エイリアスは Claude Code の Bash からは効かない** — `vsync` を頼まれた時、`.zshrc` を source した IDE 外部のインタラクティブシェルしかエイリアスを認識しない。Claude Code 側では alias の中身 (`cd ~/ObsidianVault && git add . && git commit -m "..." && git push`) を直接書く必要があった

### 判断が難しかった

- **既存ファイルが「枠だけ存在」していた時の上書き判断** — `~/ObsidianVault` がディレクトリ構造だけ作られていた。中身ゼロを確認してスキップ判定したが、中身があれば確認すべき。文脈依存の判断
- **Obsidian 自身が `.obsidian/` 配下を書き換える** — `core-plugins.json` などは Obsidian 起動時に自動更新される。git で追跡しても恒常的に diff が出る → 人間が手で固定したい設定は最小限に絞るのが現実的

### 複数セッション間の干渉

- **同じ Vault を別 Claude セッションが同時編集** — `log.md` や `active_projects.md` が外部から更新される現象あり。`system-reminder` の「ファイルが modified された」通知を尊重して revert しない運用が必要

## 📋 次回同じことをするときのチェックリスト

### 大規模構築タスクを始める前

- [ ] `~/.claude/settings.json` の `permissions.allow` を整備 (タスクで使う Bash / Write / Edit パターンを列挙)
- [ ] `deny` で破壊操作 (`Bash(rm -rf:*)` `Bash(sudo:*)`) を必ず止める
- [ ] 指示書を別 Markdown ファイルに切り出し、Claude Code に「これを読んで通りにやって」と渡す
- [ ] 確定方針 (規模・分離方針・同期方式・認証情報・トーン) を冒頭で1行ずつ列挙
- [ ] 手動操作が必要なステップは指示書で明示 (「Step N で止まる」と書く)
- [ ] 「判断に迷う点は遠慮なく質問してください」の一文を入れる

### 実行中

- [ ] 既存ファイルを編集する前に必ず Read を入れる (Read → Edit の順序)
- [ ] Read と Edit を並列発行しない
- [ ] 進捗は `✅ Step N/M 完了: 内容` の決め打ちフォーマット
- [ ] 大規模なら TaskCreate でタスクリスト化
- [ ] 判断ポイントは AskUserQuestion で選択肢提示 (推奨案を1番目に置く)

### 完了時

- [ ] 完了報告は1〜2文 + 「次にどうするか」
- [ ] 装飾は控えめ、結論ファースト
- [ ] エイリアスを使う指示があったら、Claude Code 側では alias 中身を直接書く

### よくある落とし穴

- [ ] エイリアスは Claude Code の Bash からは効かない
- [ ] Obsidian 起動後に `.obsidian/` 配下が自動更新される → linter 通知を都度確認
- [ ] 既存ファイルの上書き判断は中身ゼロかどうかで分岐
- [ ] system-reminder で外部変更通知が来たら、revert せず尊重する
- [ ] Claude の「作りました」報告は実ファイルで検証 (Obsidian UI のキャッシュで見えないことがある)

---

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
