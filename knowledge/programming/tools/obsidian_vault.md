---
type: knowledge
domain: programming/tools
created: 2026-05-18
last_updated: 2026-05-18
tags: [obsidian, vault, claude, memory, workflow]
priority: highest
---

# Obsidian Vault — 運用マニュアル

> このVaultをどう使うかの完全マニュアル
> Vault自身に「Vaultの使い方」を書く = メタ知識
>
> 未来のClaudeが「何をどう書き込むべきか」迷ったらここを読む

## 🎯 このVaultの目的

「Claudeの記憶喪失問題」を構造的に解決する外部記憶インフラ。

詳細は [[2026-05-18_Obsidian_Vault構築完了]] 参照。

## 📥 何を、どこに、いつ保存するか

### 保存の基本ルール

| 種別 | 保存先 | 判断基準 |
|------|--------|---------|
| 新しい意思決定 | `decisions/YYYY-MM-DD_<内容>.md` | 「あとで誰かに『なぜそう決めた?』と聞かれそう」なら保存 |
| 新しい知識・ノウハウ | `knowledge/<領域>/<名称>.md` | 「次に似た状況で見返したい」なら保存 |
| Claude のミス | `mistakes/claude_mistakes.md` に追記 | ミスしたら**必ず**保存 (例外なし) |
| プロジェクト進捗変化 | `current_state/active_projects.md` 更新 | 主要マイルストーン到達/方向転換時 |
| 日々の作業ログ | `daily/YYYY-MM-DD.md` | 重要な日のみ、雑記レベル |
| 生データ (録音文字起こし等) | `raw/<カテゴリ>/<日付>_<名称>.md` | 元データは全部 |
| 完了プロジェクト | `archive/YYYY-MM_<名称>/` に移動 | プロジェクト完了時 |
| Wiki統合ノート | `wiki/<概念名>.md` | 複数領域を横断する概念 |

### 必ず保存すべき5つのトリガー

以下が発生したら**必ず保存** (確認すら不要):

1. ✅ **Claude が同じミスを2回した** → mistakes/ に追記必須
2. ✅ **YDが「これ重要」と発言した** → decisions/ に保存必須
3. ✅ **新しいツール・サービスを採用した** → knowledge/programming/tools/ に保存必須
4. ✅ **プロジェクトが完成した** → archive/ に移動、current_state 更新必須
5. ✅ **YDの好み・価値観に変化があった** → identity/preferences または values 更新必須

### 確認してから保存するもの

迷ったら「保存しますか?」と YD に聞く:

- 雑談の中で出た面白いアイデア
- まだ実行していない仮説
- 個人的な感想・感情の記録

## 🚫 保存してはいけないもの

- パスワード、APIキー、認証情報 → **絶対NG**、別ツール (1Password等) で管理
- 一時的な作業ファイル (デバッグ中のコードスニペットなど) → 完成後に保存
- 重複情報 → 既存ページに追記する形にする
- 他人の機密情報 (クライアント名、選考結果、人間関係のセンシティブな話) → 抽象化して保存

## 🔄 情報の流れ (重要な現実)

### 自動連携はされない

| 情報源 | 自動でVaultに来るか |
|--------|-----------------|
| Claudeデスクトップアプリ会話 | ❌ 来ない (Anthropicサーバー内) |
| Claude Code セッション | ❌ 来ない (~/.claude/projects/ にあるが永続じゃない) |
| ChatGPT, Gemini 会話 | ❌ 来ない (各社の閉じたシステム) |
| 公式メモリ機能 | ❌ 来ない (中身は外部に取り出せない) |
| **Obsidian Vault** | **✅ 自分で書き込むか、Claudeに頼んで書き込ませる** |

### 書き込み主体の3パターン

#### パターンA: YD手動
- 会話中の重要部分をコピペ
- メリット: 自分で取捨選択
- デメリット: めんどい、続かない

#### パターンB: Claude経由 (推奨、現在のPhase)
- 会話終わりに「今のVaultに保存して」と頼む
- Claude (Desktop Commander経由 or Claude Code) が要点抽出して書き込み
- メリット: 楽、Claudeが整理
- デメリット: 頼むのを忘れる可能性

#### パターンC: フル自動 (将来のPhase)
- Claude Code の hook で セッション終了時に自動保存
- 堀口英剛氏方式
- メリット: 完全自動
- デメリット: セットアップ複雑、判断ミスのリスク

## 📅 運用フェーズ (段階的自動化)

### Phase 1: 手動運用 (今〜2週間)

**目的**: 「何を保存すべきか」の判断基準を体感で理解

**やること**:
- 会話終わりに Claude が「保存しますか?」と確認
- YD は Yes/No で判断
- 保存先は Claude が判断、YD は内容を承認

**Claude側の振る舞い**:
- 会話の最後に必ず「Vault に保存すべき内容があるか」をチェック
- ある場合は具体的な保存案を提示してから書き込み

### Phase 2: 半自動化 (2週間〜1ヶ月後)

**目的**: 明らかに重要なパターンは確認なしで自動保存

**やること**:
- 「必ず保存すべき5つのトリガー」に該当する場合は確認なしで保存
- それ以外は引き続き Phase 1 と同じ

**移行条件**:
- Phase 1 で 50件以上の保存実績がある
- YDが「もう判断基準分かったから自動化していい」と発言

### Phase 3: フル自動化 (1ヶ月後)

**目的**: YDが何もしなくても会話の重要部分が Vault に溜まる

**やること**:
- Claude Code に hook 設定 (`~/.claude/hooks/`)
- セッション終了時に以下を自動実行:
  - log.md 追記
  - 新しい意思決定があれば decisions/ に保存
  - 新しい知識があれば knowledge/ に保存
  - mistakes に追記すべきものがあれば追記
  - git commit + push
- CLAUDE.md に詳細な判断基準を追記 (Phase 1-2 で学んだパターン)

**移行条件**:
- Phase 2 で安定運用できている
- 自動化失敗時のロールバック手順が確立されている

## 🛠 メンテナンス

### 毎週やること

- `git log` で「先週何が追加されたか」を1分で確認
- 重複・矛盾があれば修正

### 毎月やること (Claudeに依頼)

```
今月の Vault に追加された内容をレビューして、以下をチェックしてください:
- 重複コンテンツ
- 矛盾する情報
- どこからもリンクされていない孤立ページ
- archive に移動すべき完了プロジェクト
```

### 半年に一度

- ディレクトリ構造の見直し
- CLAUDE.md の更新 (実運用で学んだベストプラクティスを反映)
- knowledge/ の再分類

## 💾 同期・バックアップ

### 通常運用

```bash
vsync   # Vault変更を git add + commit + push
```

### 状況確認

```bash
vstatus   # git status
vlog      # 直近20件のcommit log
```

### 別Macで作業したい場合

```bash
git clone git@github.com:Yitao-Ding/yd-obsidian-vault.git ~/ObsidianVault
```

## 🆘 トラブルシューティング

### Q. Claudeが Vault を読みに行かない

→ CLAUDE.md の起動シーケンスを守るよう促す:
```
~/ObsidianVault/CLAUDE.md を読んで、起動シーケンスに従ってください
```

### Q. Claudeが古い情報を返す

→ メモリ機能と Vault がズレている可能性:
```
current_state/active_projects.md を最新版として読み直してください
```

### Q. Git push でエラー

→ 認証切れ・コンフリクトの可能性:
```bash
cd ~/ObsidianVault
gh auth status  # 認証確認
git pull --rebase
```

### Q. Vault が肥大化して重い

→ archive/ への移動が遅れている可能性。Claudeにレビュー依頼。

---

## ✅ うまく行ったこと

### 設計

- **Karpathy LLM Wiki + 堀口 Mistakes フォルダのハイブリッド** — 構造論と運用論の両輪で組んだら、互いの欠点を補い合った。片方だけだと不完全
- **Mistakesフォルダに初期17件仕込み** — 過去会話を `conversation_search` で掘って実例ベースで埋めたので、初日から「次のClaudeが同じ過ちを避ける」仕組みが稼働
- **Markdown のみ構成** — ChatGPT / Gemini / Cursor からも素のテキストとして読める。AI ロックインを回避できた
- **Git + GitHub Private** — バックアップ・履歴・別端末同期が一発で揃った。iCloud と違って `.obsidian/workspace.json` の同期衝突問題もない

### 運用

- **Phase 1 → 2 → 3 の段階設計** — いきなり全自動を狙わず、まず手動でパターン学習する設計。フル自動化失敗のリスクを抑えた
- **保存トリガー5種類の明文化** — Mistakes 2回目 / YD「重要」発言 / 新ツール採用 / プロジェクト完了 / 好み変化 — 判断基準が言語化されていて、迷いが少ない
- **エイリアス4種 (`vault` `vsync` `vstatus` `vlog`)** — 運用開始初日から「ターミナル4文字で同期」が回る状態に
- **プラグインバイナリは Git 追跡対象から除外** — manifest.json と data.json のみ追跡。リポジトリ軽量、別端末では同名プラグインを再インストールする運用

## ❌ 詰まったこと

### 認識ずれ

- **「Vault作っただけで Claude 会話が自動連携される」と最初期に勘違いしがち** — 実際は明示的に書き込まないと空のまま。気づくのに議論が要った
- **「機密分離Vault」を最初検討した** — 結局 1Vault + `_private/` + `.gitignore` で十分と判明。最初から1Vaultでよかった
- **「フル自動化は今日からできる」と思いがち** — メモリ機能とのリアルタイム同期は技術的に不可能、段階運用が現実解

### 複数AI間の協調

- **複数 Claude セッションが同じ Vault を同時編集** — `log.md` や `active_projects.md` に外部変更が入ることがある。`system-reminder` で気づける運用に頼っている
- **Obsidian UI のファイルツリーが外部追加を即時拾わない** — ターミナル / 別 Claude で作ったファイルが Obsidian 上で見えないことがある。`⌘+O` ファイル検索 or Vault 再オープンで解決

### 設定の境界

- **`.obsidian/` 配下を Obsidian 自身が書き換える** — `core-plugins.json` などは Obsidian 起動時に diff が出る。`.gitignore` で workspace / cache / appearance を除外する設計が必要
- **プラグインバイナリを Git に含めるべきか問題** — 含めれば再現性高、除外すれば軽量。今回は除外して manifest のみ追跡

## 📋 次回同じことをするときのチェックリスト

### 別端末でこの Vault を再現したい時

- [ ] `git clone git@github.com:Yitao-Ding/yd-obsidian-vault.git ~/ObsidianVault`
- [ ] Obsidian.app をインストール (`brew install --cask obsidian`)
- [ ] Obsidian で「Open folder as vault」 → `~/ObsidianVault` を選ぶ
- [ ] 「Trust author and enable plugins」を選択
- [ ] コミュニティプラグインを再インストール: Dataview / Templater / Calendar
- [ ] `~/.zshrc` にエイリアス4つを追記 (`vault` `vsync` `vstatus` `vlog`)
- [ ] `gh auth status` で GitHub 認証確認

### 日々の運用で守ること

- [ ] 区切りごとに `vsync` で push (git add + commit + push 一発)
- [ ] パスワード / API キー / 認証情報は **絶対に書き込まない** (別ツールで管理)
- [ ] Claude が「保存しました」と言ったら、実ファイルを `ls` で検証
- [ ] 重複情報は新規ノートを作らず既存に追記
- [ ] 機密度の高い情報は抽象化して書き込む

### Phase 2 移行の判断

- [ ] Phase 1 で 50件以上の保存実績
- [ ] YD が「もう判断基準分かったから自動化していい」と発言
- [ ] 保存トリガー5種類が体感で身についた状態

### Phase 3 (フル自動) 移行の判断

- [ ] Phase 2 で2週間以上安定運用
- [ ] `~/.claude/hooks/` の自動化スクリプトが書ける状態
- [ ] ロールバック手順が確立されている

### よくある落とし穴

- [ ] 「Vault は箱、中身は明示的に書く」を忘れない
- [ ] iCloud + Obsidian の併用は `.obsidian/workspace.json` 同期衝突に注意
- [ ] Obsidian UI のファイルツリーキャッシュ更新は手動 (Vault 再オープン or `⌘+O` で確認)
- [ ] 複数 Claude セッション同時編集は `system-reminder` で検知して尊重する

---

## 📚 関連ドキュメント

- [[CLAUDE]] — Vault憲法
- [[00_CLAUDE_BOOT]] — 起動シーケンス
- [[2026-05-18_Obsidian_Vault構築完了]] — 構築の意思決定記録
- [[claude_mistakes]] — Claudeの過去ミス記録
- [[active_projects]] — 進行中プロジェクト

## 🌐 外部参考リソース

- Karpathy "LLM Wiki pattern" (2026-04)
- 堀口英剛 YouTube — Obsidian × Claude 運用

## 📝 このマニュアル自身のメンテナンス

このファイルは Vault 運用の進化に応じて更新される。

更新タイミング:
- Phase 1 → 2 移行時 (運用ルール変更)
- Phase 2 → 3 移行時 (自動化追加)
- 大きなトラブル発生時 (トラブルシューティング追記)
- 半年ごとの見直しで全体ブラッシュアップ
