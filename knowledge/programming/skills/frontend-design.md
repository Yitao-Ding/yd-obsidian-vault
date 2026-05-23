---
type: knowledge
domain: programming/skills
tool: frontend-design
last_updated: 2026-05-21
---

# frontend-design skill

YD が 2026-05-21 に提供してくれた Claude Code skill。設置場所: `~/.claude/skills/frontend-design/SKILL.md`

## 概要

「AI が生成しがちな generic な UI 美学」(= AI slop) を**意図的に避けて**、distinctive で production-grade な frontend を作るための **設計哲学 skill**。データベースや CSV を持たない、**判断基準だけの skill**。

## コア原則

### Design Thinking (実装前に決めること)

1. **Purpose**: 何の問題を解くか、誰が使うか
2. **Tone**: 極端な方向を 1 つ commit
   - brutally minimal / maximalist chaos / retro-futuristic / organic / luxury / playful / editorial / brutalist / art deco / soft pastel / industrial
3. **Constraints**: framework / performance / accessibility
4. **Differentiation**: 何が記憶に残るか

> **Choose a clear conceptual direction and execute it with precision. Bold maximalism and refined minimalism both work - the key is intentionality, not intensity.**

### Aesthetics の重点 4 領域

| 領域 | 指針 |
|------|-----|
| Typography | Arial / Inter のような generic 排除、display font + body font の組合せで characterful に |
| Color & Theme | dominant + sharp accent、CSS variables で統一 |
| Motion | high-impact moment に集中、page load の staggered reveal、scroll-trigger、surprise する hover |
| Spatial | asymmetry / overlap / diagonal / grid-breaking / 大胆な negative space or controlled density |
| Background | solid color に逃げない。gradient mesh, noise, geometric pattern, grain, dramatic shadow, custom cursor |

### 絶対回避リスト (AI slop)

- フォント: Inter, Roboto, Arial, system fonts, **Space Grotesk** (これも繰り返し禁止と明記されてる)
- 配色: 紫グラデ on 白
- レイアウト: predictable, cookie-cutter
- 結果: context-specific character が無い

### 実装の密度マッチング

- maximalist 方向 → elaborate code, extensive animation
- minimalist 方向 → restraint, precision, subtle detail
- 「複雑さ自慢」ではなく方向性に**忠実な実装**

## ui-ux-pro-max との関係

[[ui-ux-pro-max]] と**併用必須** (YD 指示)。詳しい運用ルールは [[index]] 参照。

ざっくり:
- frontend-design = 方向性決定 + アンチパターン避け
- ui-ux-pro-max = 具体素材 (palette / font / chart / stack guideline) のデータベース検索

## 設置情報

- 設置元: YD 直接提供 (SKILL.md 全文を貼り付け)
- 設置パス: `~/.claude/skills/frontend-design/SKILL.md`
- frontmatter: `name: frontend-design` / `description: ...AI slop 回避... / license: Complete terms in LICENSE.txt`
- LICENSE.txt は今のところ別途取得せず (frontmatter のみで skill 認識される想定)

## ✅ うまく行ったこと

- YD が SKILL.md 全文を貼ってくれたので、frontmatter ごと**そのまま** `~/.claude/skills/frontend-design/SKILL.md` に書き込むだけで完結
- 既存の `example-skills:frontend-design` (Anthropic 公式 plugin 版) とは独立した「自分管理 skill」として共存可能 (名前衝突なし、prefix で区別)

## ❌ 詰まったこと

- 該当なし (2026-05-21 設置時点)。将来 skill を実際に使ってみて躓いたらここに追記する。

## 📋 次回同じことをするときのチェックリスト

1. SKILL.md は frontmatter (`name`, `description`) があれば最低限 skill 認識される
2. 設置先は `~/.claude/skills/<skill-name>/SKILL.md`
3. `~/.claude/plugins/` 以下の plugin 版とは別物として共存可能 (prefix `example-skills:` などで区別される)
4. UI タスクで使う時は必ず [[ui-ux-pro-max]] と**併用** — index.md の運用ルールを毎回チェック
5. アンチパターンリスト (Inter / Space Grotesk / 紫グラデ) は実装前に**必ず照合**

## 📚 関連

- [[ui-ux-pro-max]] — 併用相手
- [[index]] (knowledge/programming/skills/) — 運用ルール集約
