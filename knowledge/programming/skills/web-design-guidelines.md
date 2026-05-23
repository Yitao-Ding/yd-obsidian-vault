---
type: knowledge
domain: programming/skills
tool: web-design-guidelines
last_updated: 2026-05-21
---

# web-design-guidelines skill

Vercel 製の UI **レビュー専用** Claude Code skill。YD が 2026-05-21 に提供。設置場所: `~/.claude/skills/web-design-guidelines/SKILL.md`

## 概要

書いた UI コードを「Vercel Web Interface Guidelines」に**適合してるか自動レビュー**する skill。本体ルールは GitHub からその場で取得 (`WebFetch`) するため、常に最新基準でチェックされる。

### ガイドライン取得元

```
https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md
```

このリポジトリ自体がガイドライン本体。命令フォーマット (出力形式) も同ファイル内に書かれている。

## 動作フロー

1. `WebFetch` で最新ガイドラインを取得
2. 指定されたファイル (またはパターン) を Read
3. ガイドラインの全ルールに照合
4. `file:line` 形式 (terse output) で違反箇所を列挙

## トリガーフレーズ

YD が言いそうな日本語含む:
- 「UI レビューして」「アクセシビリティチェックして」「デザイン監査」「UX 見て」「サイトをベストプラクティスに照らして」
- "review my UI" / "check accessibility" / "audit design" / "review UX" / "check my site against best practices"

## 既存 plugin skill との並存

`system-reminder` のスキル一覧に **`web-interface-guidelines`** (Vercel plugin pack 由来と推測) という**ほぼ同等の skill** が既に存在する。名前と用途がほぼ被るが、両者を共存させる:

| Skill | 出処 | 設置形態 | 名前 |
|-------|-----|---------|-----|
| `web-design-guidelines` | YD 直接提供 (この記事の skill) | `~/.claude/skills/web-design-guidelines/SKILL.md` | YD が指定したカスタム名 |
| `web-interface-guidelines` | Vercel plugin (Anthropic plugin marketplace 経由) | plugin cache | 公式正式名 |

実体は同じく `vercel-labs/web-interface-guidelines/main/command.md` を参照すると思われるため、**機能的にはどちらを呼んでも同じ結果が出るはず**。

将来的にどちらかに寄せる判断もあり (検討事項):
- YD 管理の `web-design-guidelines` 側を残す (自分のコントロール下)
- plugin 側 `web-interface-guidelines` に寄せて自分管理を削除 (公式更新追従)

現状は両方残しで運用する。

## ui-ux-pro-max / frontend-design との関係

- **制作時**: frontend-design (方向性) + ui-ux-pro-max (素材データベース) を使う
- **レビュー時**: web-design-guidelines (← この skill) でガイドライン適合を検査
- **3 フェーズ運用の詳細**: [[index]] 参照

## ✅ うまく行ったこと

- YD が SKILL.md 全文を貼ってくれたので、そのまま `~/.claude/skills/web-design-guidelines/SKILL.md` に書き込むだけで skill 認識完了
- ガイドライン本体を WebFetch で取りに行く設計のため、**ローカルにルール集を持たなくて良い** (常に最新版で動く、メンテナンス不要)
- Vercel 公式 plugin の `web-interface-guidelines` と名前が違うので**衝突しない** (両方並存可能)

## ❌ 詰まったこと

- 設置直後に skill 一覧で `web-interface-guidelines` という**ほぼ同名の Vercel plugin 由来 skill** の存在に気付いた。事前確認していれば独立追加せず plugin 側を使うだけで済んだ可能性
- 今回は両方残し運用にしたが、将来重複コストが顕在化したらどちらかに寄せる
- (2026-05-21 設置時点ではまだ実行してないので、レビュー結果の品質比較はこれから)

## 📋 次回同じことをするときのチェックリスト

1. 新 skill を追加する前に、system-reminder のスキル一覧で**類似名/類似機能 skill が無いか先に検索**する (今回は `grep -i 'guideline\|review\|interface'` 相当のチェックを怠った)
2. WebFetch 系 skill は**外部 URL がいつ消えても動かなくなる**ことを意識。リポジトリが消されたらこの skill も死ぬ
3. レビューを依頼する時はファイル/パターンを必ず指定 (引数なしだと「どれをレビューしますか?」と聞き返してくる)
4. 出力は `file:line` 形式の terse output なので、Claude 側で必要に応じて**人間向けに要約**する

## 📚 関連

- [[frontend-design]] — 制作時の方向性 skill
- [[ui-ux-pro-max]] — 制作時の素材 skill
- [[index]] (knowledge/programming/skills/) — 3 フェーズ運用ルール
