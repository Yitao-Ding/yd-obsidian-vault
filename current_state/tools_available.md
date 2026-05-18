---
type: current_state
last_updated: 2026-05-18
priority: high
---

# 使えるツール一覧 (環境別)

> Claudeが「ツールがない」と早合点しないように、環境ごとに使えるツールを明示
> 新セッション開始時に必ず確認

## 🖥 Claude デスクトップアプリ (claude.ai のチャット)

このVaultにアクセスしているClaudeは、通常以下のツールが使える:

### ローカルファイルアクセス
- **Desktop Commander**: `/Users/ittou/` 配下の全ファイル読み書き可
  - `read_file`, `write_file`, `edit_block`
  - `list_directory`, `create_directory`, `move_file`
  - `start_process`, `interact_with_process` (ターミナル実行)

### MCP連携 (常時接続中)
- **Notion**: Arte Grow root等のページ管理
- **Google Drive**: ドキュメント検索・取得
- **Google Calendar**: 予定確認・追加
- **Gmail**: メール送受信
- **Canva**: デザイン生成
- **Zoom for Claude**: ミーティング情報取得
- **Goodnotes**: ノート作成
- **Microsoft 365**: Office文書
- **Figma**: デザインファイル参照
- **Miro**: ボード操作

### Web情報取得
- `web_search`: ウェブ検索
- `web_fetch`: 特定URL取得
- `image_search`: 画像検索

### 視覚化
- `visualize:show_widget`: SVG/HTMLウィジェット描画
- `visualize:read_me`: ウィジェット仕様取得

### 過去文脈
- `conversation_search`: 過去会話のキーワード検索
- `recent_chats`: 直近の会話一覧取得
- `memory_user_edits`: メモリ編集

### その他
- `weather_fetch`: 天気
- `places_search` / `places_map_display_v0`: 地図・場所
- `recipe_display_v0`: レシピ表示
- `message_compose_v1`: メッセージ作成
- PDF Tools: PDF操作
- pdf-viewer: PDF表示

## 💻 Claude Code (ターミナル)

```bash
claude
```

で起動。**完全なローカルアクセス権**を持つ。

### 主要機能
- 全ファイルシステム読み書き
- Bash コマンド実行
- Git 操作
- npm/pip/uv パッケージ管理
- VS Code との連携
- MCPサーバー接続 (`~/.claude/` 設定経由)

### 動作環境
- ターミナル: zsh (デフォルト)
- ホーム: `/Users/ittou/`
- グローバル CLAUDE.md: `~/.claude/CLAUDE.md`

## 🌐 Claude Web版 (一般ブラウザ)

- 基本的なチャット
- Web Search のみ
- ローカルファイルへのアクセスは無し
- このVaultへの直接アクセスは不可

**推奨**: Web版を使う場合、Vault の内容を手動でコピペして渡す

## 📱 Claude モバイルアプリ

- 基本的なチャット
- ローカルファイル不可
- 軽い質問・確認用途のみ

## 🤖 他AI (参考)

このVaultはMarkdownのみで構成されているため、以下のAIからも読み書き可能:

### ChatGPT
- Code Interpreter経由でファイルアップロード
- GPTsでVaultをアップロード

### Gemini
- ファイルアップロード機能
- 大きなファイルにも対応

### Cursor
- VS Code互換、Vaultフォルダを直接開ける
- AIアシスタントとして使用可

### Manus
- 引き継ぎドキュメント経由

## ⚠️ 注意事項

### Claudeがやりがちなミス
- 「ローカルファイルにアクセスできない」と早合点する
  → このファイルを再確認すること
- 「Notion情報が取れない」と諦める
  → Notion MCP を試すこと
- 「ツールがない」と諦める
  → `tool_search` で動的にロード可能なツールを検索

### ツール検索のキーワード例
- `tool_search("calendar events")` → Google Calendar
- `tool_search("notion search")` → Notion
- `tool_search("file read")` → Desktop Commander
- `tool_search("conversation history")` → Past Chats

## 📚 関連

- [[active_projects]]
- `mistakes/tool_usage_mistakes.md` - ツール使用のミス記録
