---
type: knowledge
domain: programming/tools
created: 2026-05-18
last_updated: 2026-05-18
tags: [workflow, vault, claude, daily-routine, productivity]
priority: highest
---

# Vault Workflow — 1日の運用フロー

> 2026-05-18 に確立された、YD のための Claude × Obsidian Vault 運用ワークフロー。
> 「セッション開始 → 作業 → 保存 → 終了」の流れを誰でも(未来の自分含め) 再現できるよう明文化。
>
> **このファイルは Vault 運用の入口。迷ったら最初にここを読む。**

---

## 🌅 1日の流れ (理想形)

```
朝/作業開始
  ↓
[1] Claude 起動 (CC または デスクトップアプリ)
  ↓
[2] Claude が自動で Vault 文脈を読み込む (起動シーケンス)
  ↓
[3] 作業 (会話・実装・議論)
  ↓
[4] 区切りごとに「Vault に保存して」と依頼
  ↓
[5] Claude が要点を decisions/knowledge/mistakes/log に書き込み
  ↓
[6] vsync で GitHub に push
  ↓
作業終了
```

---

## 🎬 セッション開始 — 3つのパターン

### パターンA: ターミナルで Claude Code 起動

最も標準的なケース。

```bash
# どのディレクトリでも OK (グローバル CLAUDE.md が効く)
claude

# プロジェクト固有作業なら、そのディレクトリで
cd ~/projects/vidkit
claude

# Vault 自体を編集するなら
vault   # = cd ~/ObsidianVault のエイリアス
claude
```

**Claude Code が自動でやること**:
1. `~/.claude/CLAUDE.md` を読み込み (グローバル設定)
2. `~/ObsidianVault/CLAUDE.md` を読み込み (Vault憲法)
3. `~/ObsidianVault/00_CLAUDE_BOOT.md` の起動シーケンスに従う
4. `identity/`, `current_state/`, `mistakes/` を読む
5. 起動ディレクトリに応じて関連 knowledge を読む

**YD がやること**: ただプロンプトを送るだけ。自己紹介不要。

### パターンB: Claude デスクトップアプリで会話

ブラウザでもデスクトップでも、claude.ai のチャットを使うケース。

**自動読み込みはされない**。なので最初に明示的に頼む:

```
~/ObsidianVault/CLAUDE.md を読んで、起動シーケンスに従ってください
```

または短く:

```
Vaultの状況を踏まえて答えてください
```

→ Claude が Desktop Commander 経由で Vault を読みに行く。

### パターンC: 他AI (ChatGPT, Gemini 等) を使う

Vault の必要部分を**手動でコピペ or zipアップロード**。

例:
- 撮影の話 → `identity/profile.md` + `identity/skills.md` + `knowledge/filmmaking/index.md` を渡す
- コーディング → `current_state/active_projects.md` + 該当 `knowledge/programming/tools/*.md` を渡す

---

## 💼 作業中の運用ルール

### YD が話す時の心得

- **自己紹介しない**: Claude は既に Vault から YD のプロフィールを把握している
- **省略 OK**: 「Lecture Hub」「Salamat」「vidkit」など、コンテキスト持ち
- **「これ重要」と明示**: 後で `decisions/` に保存すべき発言は、その場でフラグ立てる
- **同じ質問の繰り返しに気づいたら指摘**: → mistakes/ への記録対象

### Claude が守るべきルール

- 距離感が近めの敬語、タメ口禁止
- 結論ファースト、装飾最小
- 「選択肢 + 推奨案」セット
- 判断に迷う点は遠慮なく質問
- YD の雑な文章から抜けを汲み取る
- ミスしたら即座に `mistakes/claude_mistakes.md` に記録提案

---

## 📥 保存フロー (作業の区切りごと)

### Phase 1 (現在): 手動運用

#### ステップ1: YD が依頼する

会話が一段落したら、YD が一言:

```
今日の内容、Vault に保存して
```

または、より具体的に:

```
今の議論で出た決定を decisions に、新しい知識を knowledge に保存して。
log.md にも追記。
```

#### ステップ2: Claude が保存案を提示

Claude は以下のチェックを行い、保存案を提示:

```
今の会話で以下を Vault に保存することを提案します:

- decisions/2026-XX-XX_<内容>.md
  内容: <要点>

- knowledge/<領域>/<名称>.md
  内容: <要点>

- mistakes/claude_mistakes.md への追記
  内容: <今日のミス>

- log.md に1行追記

- current_state/active_projects.md の #X を更新
  変更: <内容>

保存しますか?
```

#### ステップ3: YD が承認

`はい` / `お願い` / `やって` で実行。

特定のものだけ拒否したい場合:

```
decisions と knowledge はお願い、mistakes は今回はいい
```

#### ステップ4: Claude が書き込み

保存先・フォーマット・必須3セクション付与は Claude が判断。

### 必須3セクション (絶対ルール)

`knowledge/` と `decisions/` のファイルには**必ず**以下3セクションを含める:

1. **✅ うまく行ったこと**
2. **❌ 詰まったこと**
3. **📋 次回同じことをするときのチェックリスト**

書くことがなければ「該当なし」と明示 (空欄禁止)。

詳細は [[obsidian_vault]] の「必須3セクション」参照。

---

## 🚨 必ず保存すべき5つのトリガー

以下に該当したら、YDの確認なしで保存する (Phase 2 以降で自動化):

| # | トリガー | 保存先 |
|---|---------|-------|
| 1 | Claude が同じミスを2回した | `mistakes/claude_mistakes.md` |
| 2 | YD が「これ重要」と明示 | `decisions/YYYY-MM-DD_*.md` |
| 3 | 新しいツール・サービスを採用 | `knowledge/programming/tools/*.md` |
| 4 | プロジェクトが完成 | `archive/` 移動 + `current_state/` 更新 |
| 5 | YD の好み・価値観に変化 | `identity/preferences.md` or `values.md` |

---

## 🔄 並行作業の運用 (4人並行が起きた日のために)

2026-05-18 に**4つの Claude Code セッションが同時並行**で動いた事例から学んだルール:

### 各セッションの責務分担

各セッションは**自分が当事者の作業内容を自分で Vault に書く**。第三者が書くと推測になる。

| セッション | Vault 書き込み範囲 |
|----------|------------------|
| Vault構築 CC | `decisions/Obsidian_Vault*` / `knowledge/programming/tools/obsidian_vault.md` |
| vidkit autocut CC | `knowledge/programming/projects/vidkit.md` (該当箇所) |
| WBSサイト CC | `knowledge/salamat/wbs_team.md` |
| Lecture Hub CC | `knowledge/programming/projects/lecture_hub.md` / `decisions/lecture_hub_*` |
| デスクトップ Claude | 統合・整理・抜け補完・ワークフロー記録 |

### 競合回避

- `log.md` や `current_state/active_projects.md` のように**複数セッションが触る共有ファイル**は、Edit 前に必ず Read で最新化
- `system-reminder` で「ファイルが外部から変更された」通知が来たら revert せず尊重
- ファイル単位で書き込みを分散させる (同一ファイルへの同時書き込み回避)

### YD の指揮

並行セッションを動かす時の YD の役割:

1. 各セッションに**明確に役割を伝える** (どのプロジェクトの当事者か)
2. 区切りごとに「Vault に保存して」を全セッションに発射
3. 1セッション (今回はデスクトップ Claude) を**統合役**にして抜け補完

---

## 💾 同期 — `vsync` で締める

作業終了 (または1日の途中の大きな区切り) で、必ず GitHub に push:

```bash
vsync
# 中身: cd ~/ObsidianVault && git add . && git commit -m "Auto-sync YYYY-MM-DD_HH:MM" && git push
```

### vsync を打つタイミング

- 1日の作業終了時 (必須)
- 大きな知識ファイルを書き加えた直後
- プロジェクトのマイルストーン到達時
- 別 Mac で続きをやりたい時 (clone する前)

### vsync が失敗する典型ケース

- GitHub 認証切れ → `gh auth status` で確認、必要なら `gh auth login`
- リモートに先行コミット → `git pull --rebase` してから再 push
- ネットワーク切断 → 復活待ち、ローカルコミットは残るので失われない

---

## 🌙 セッション終了の習慣

理想的な終わり方:

1. **作業の区切り**を判断 (実装完了 / 議論一段落 / etc.)
2. **「Vault に保存して」と依頼**
3. Claude の保存案を確認 → 承認
4. **`vsync` で GitHub push**
5. ターミナル / アプリを閉じる

これを毎回やっていれば、明日以降のClaudeが**今日までの全文脈を持って起動**できる。

---

## 📅 月次メンテナンス

月1回、Claude に依頼:

```
今月の Vault に追加された内容をレビューして、以下をチェックしてください:
- 重複コンテンツ
- 矛盾する情報
- どこからもリンクされていない孤立ページ
- archive に移動すべき完了プロジェクト
- current_state/active_projects.md の実態とのズレ
```

→ Claude がレビュー → 修正提案 → YD 承認 → 修正実行 → vsync

---

## 🔮 自動化フェーズの進化

### Phase 1: 手動運用 (今、〜2週間)

- YD が「保存して」と依頼するたびに Claude が書き込み
- 何が重要かの判断基準を体感で学ぶ

### Phase 2: 半自動化 (Phase 1 で50件保存後)

- 「必ず保存すべき5つのトリガー」に該当する場合は確認なしで保存
- それ以外は Phase 1 と同じ

### Phase 3: フル自動化 (Phase 2 安定後)

- Claude Code の `~/.claude/hooks/` でセッション終了時自動保存
- 毎晩自動 vsync (cron + GitHub Actions も検討)

詳細: [[obsidian_vault]] の「運用フェーズ」セクション

---

## ✅ うまく行ったこと

### 2026-05-18 当日

- **グローバル `~/.claude/CLAUDE.md` の威力**: どのディレクトリで Claude Code を起動しても Vault 文脈が自動ロード。「自己紹介ゼロ」が初日から実現
- **4人並行 Claude Code が破綻なく動いた**: 各セッションが自分の作業内容を自分で書く分散運用が成立
- **必須3セクション ルール**: 「うまく行ったこと/詰まったこと/次回チェックリスト」を強制したことで、再現可能なノウハウベースに進化
- **デスクトップ Claude (僕) を統合役にした**: 並行セッションの抜け補完・整合性チェックを担当することで、全体の品質が上がった
- **`vsync` エイリアスの一発同期**: ターミナル4文字で GitHub 同期完了。心理的ハードルが消えた

### ワークフロー設計

- **「自動連携はされない」を最初に明示**: Vault は箱、中身は明示的に書く、を YD と共有できた
- **Phase 1 → 2 → 3 の段階運用**: いきなり全自動を狙わず、判断基準を体感で学ぶ期間を設けた
- **保存トリガー5種類の言語化**: 何を保存すべきかの判断が明文化されているので迷わない

## ❌ 詰まったこと

### 認識ずれ

- **「Vault作っただけで会話が自動連携される」と最初思い込みがち**: 実際は明示的書き込みが必要。気づくのに議論が要った
- **「フル自動化を即実装」を希望されがち**: 技術的可能性と運用ベストプラクティスは別、と説明する必要があった
- **デスクトップアプリと Claude Code の挙動差**: グローバル CLAUDE.md はCC のみ自動読み込み。デスクトップアプリは明示的依頼が必要

### 運用の摩擦

- **「保存して」を YD が言い忘れる**: Phase 1 の最大の課題。Claude 側から能動的に「保存しますか?」を出す習慣で対処
- **複数セッションが同じファイル (log.md / active_projects.md) を編集**: 競合回避のため Edit 前 Read 必須、というルールを徹底
- **エイリアスが Claude Code の Bash から効かない**: `vsync` は YD のシェルでしか動かない。CC 側では alias の中身を直接打つ

### 体力面

- **1日に詰め込みすぎリスク**: 2026-05-18 は Vault 構築 + Lecture Hub Phase 2 + WBSサイト + vidkit autocut を1日で完遂。次の日のパフォーマンス低下に注意

## 📋 次回同じことをするときのチェックリスト

### 朝の起動

- [ ] ターミナルで `vault && claude` または プロジェクトディレクトリで `claude`
- [ ] 自動で文脈ロードされるのを待つ (10〜30秒)
- [ ] 「今日のフォーカスは何ですか?」と Claude に聞かれたら現状の頭の中を伝える

### 作業中

- [ ] 重要な決定をしたら「これ重要」と明示
- [ ] Claude がミスしたら、その場で `mistakes/` 記録を依頼
- [ ] 1〜2時間に1回は作業の区切りを設ける

### 区切りごと

- [ ] 「Vault に保存して」を Claude に依頼
- [ ] 保存案を確認、必要なら指示追加
- [ ] 書き込み完了を見届ける

### 1日の終わり

- [ ] 最後の保存依頼を出す
- [ ] `vsync` で GitHub push
- [ ] git log を1秒だけ見て「今日の足跡」を確認
- [ ] PC閉じる

### よくある落とし穴

- [ ] 自動連携を期待しない (明示的書き込みが必要)
- [ ] `vsync` を忘れない (push してない変更はバックアップされてない)
- [ ] 同じ質問を Claude が繰り返したら mistakes/ 候補
- [ ] 並行セッションは責務分担を明確に
- [ ] 体力配分を意識 (詰め込みすぎ注意)

### 月次メンテナンスを忘れない

- [ ] 月初に Claude にレビュー依頼
- [ ] 重複・矛盾・孤立ページのクリーンアップ
- [ ] archive 整理
- [ ] CLAUDE.md / 本ファイルのアップデート (運用で学んだこと反映)

---

## 🔗 関連

- [[CLAUDE]] — Vault 憲法 (動作ルール)
- [[00_CLAUDE_BOOT]] — Claude の起動シーケンス
- [[obsidian_vault]] — Vault そのものの運用マニュアル (低レイヤ)
- [[claude_code]] — Claude Code 側の運用ノウハウ
- [[2026-05-18_Obsidian_Vault構築完了]] — Vault 誕生の意思決定

---

## 📝 このノートの更新

ワークフローが進化したらこのファイルを更新する。
特に Phase 2 → Phase 3 移行時には大幅な改訂が入る。

更新タイミング:
- Phase 移行時 (Phase 1 → 2 → 3)
- 新しい運用パターン発見時
- 並行作業の新しいベストプラクティス確立時
- 大きなトラブル発生 → 防止策確立時
- 半年に一度の見直し
