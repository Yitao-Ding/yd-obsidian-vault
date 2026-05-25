---
type: knowledge
category: programming/tools
created: 2026-05-25
source: "[[vidkit_2026-05-25_tutorial_j_ZPV10bu54]] (UI Collective, Kirk)"
tags: [ai, design, claude, codex, figma, mobbin, stitch, workflow, design-system]
---

# AI Design Workflow — Claude / Codex / Figma / Stitch を協調させる

> UI Collective (Kirk) のフルガイド動画 (1h27m, 28章) を整理したもの。
> ソース動画: https://youtu.be/j_ZPV10bu54
> 生素材: `raw/vidkit/tutorial/vidkit_2026-05-25_tutorial_j_ZPV10bu54/`
> 関連: [[vidkit]] / [[claude_code]] / [[mcp_local_servers]]

---

## 🎯 中核メッセージ

**AI is not a tool. AI is a workflow.**

2〜3年前は Figma 1つで全部済んだ。今は複数 AI ツールを「役割で使い分け、Figma を経由して push/pull する」スキルが必要。「1つで全部やれる魔法のツール」は存在しない (デザイナーが望む理想形は2026年現在ない)。

---

## 🗺 ツール役割マップ

| ツール | 担当 | 強み | 弱み |
|---|---|---|---|
| **Google Stitch** | midfi 探索 | 速い (15秒)、mobile に強い、ほぼ無料 | desktop は弱い、production 品質ではない |
| **Claude Design** | hi-fi 1枚絵 | "senior level" な完成度、interactive | 重い (1prompt 8% 消費)、design tool ではない |
| **Claude Code** | 本実装 + Figma push | 出力品質高い、Figma 流儀 (auto layout/fill/hug) 正確 | トークン消費 3〜4倍、$20 plan 廃止予兆 |
| **Codex** | iteration 担当 | Claude の 1/3〜1/4 トークン、4倍速 | scratch 生成は弱い (Claude 経由必須) |
| **Figma** | 中継 + 微修正 | Claude/Codex 行き来のハブ | AI 機能は出遅れ (Make の DS sync は壊れ気味) |
| **Mobbin** | 画面リファレンス | ほぼ全 app の screen / flow、Figma plugin | 1枚だけ渡すと AI が one-to-one コピー (要複数 example) |
| **ChatGPT 5.5 Thinking** | 代替案画像生成 | 隠れた強力枠、chat plan に含む | 知名度低い |

---

## 🛠 セットアップ (Figma MCP + Figma Skills)

### 用語整理 (混同注意)
- **Figma MCP** = AI が Figma ファイルを「読める」ようにする接続
- **Figma Skills** = AI に Figma の「使い方」(variables / components 当て方) を教える指示書

### Claude デスクトップ側
1. Figma community skills site を開く (動画概要欄リンク)
2. **必須**: `Figma Use` の **MCP server guide ZIP 全体** を GitHub からダウンロード (中に複数 sub-skills)
3. 推奨追加:
   - `Apply Design System` (skill.md だけ DL)
   - **`Audit Design System`** ← Kirk お気に入り。既存デザインの DS 適用漏れ・誤適用を検出して修正してくれる
4. Claude 開く → 左サイドバー **Customize** → Skills → ZIP を drag upload
5. **重要トラップ**: ZIP に「includes Figma MCP servers」と書いてあるが **接続はされない**。Connectors タブで Install → Connect → ブラウザで Figma sign-in & agree

### Codex 側
- 表示はユーザーで違う (Workspace 限定の段階リリース可能性)
- Plugin が見えなければ Skills タブで `Figma` 検索 → 個別 skill (Figma MCP / Code Connect / Create Design Systems Rules…) を 1 つずつ install

---

## 🔬 Claude vs Codex ベンチマーク (動画中実験)

同一変更 (light mode化 / search bar 追加 / top households 全幅 / AUM⇔households 位置入替) を両者に実行。

| | 時間 | トークン |
|---|---|---|
| Claude | 12 分 | 38,000 |
| Codex | 4 分 | 17,000 |

注: Claude は scratch 生成、Codex は Figma 経由 import からスタートなので完全な apples-to-apples ではない。それでも Codex の効率は圧倒的。

---

## 🔄 推奨ワークフロー (再現用)

```
[1] Stitch で midfi 探索 (mobile 中心)
     └→ stakeholder と「どのデータ・どのwidget・どこに」を合意
        
[2] 合意内容を詳細プロンプト化して Claude Design or Claude Code に流す
     └→ 1枚目の hi-fi を取る
        
[3] Claude Code → Figma push
        
[4] Figma 内で軽微修正 (styles/variables 整理)
        
[5] Figma → Codex に pull、iteration (大きな変更・連続変更)
        
[6] Codex → Figma push
        
[7] Figma → Claude に戻す (developer に渡すため)
```

**例外**: 1色変えて要素2つ動かす程度なら Claude 内で完結が速い。判断軸は「変更の量・期間」。

---

## 🎓 デザインシステムを AI に学習させる (核心)

### 順序
1. **Variables / Styles** を skill 化
2. **Components** を skill 化
3. (任意) documentation を skill 化

### 準備
- Figma 上に **table 形式**で variables を整理: `token name / light value / dark value / **description (when used)**`
- description は **自分で書く** (AI 任せだと hallucinate する)

### Variables Skill 構築プロンプト
```
Please study all of the Figma variables inside of this table.
After coming to an elite understanding on the variables, their values, their naming and when they are used,
build a Claude skill that will help train Claude on when to use different variables for future designs.
Do not include any type styles or type specific variables.
Only focus on surface, border, text, and icon variables.
```

→ skill.md + 個別 markdown (border / icon / surface / text) が出力。common pairings や「text に focus state は無い」までドキュメント化。

### Type Styles Skill 構築プロンプト
```
Please study all the text styles available inside our design system above.
Please take note of all the variables applied to the styles and their values in desktop and mobile.
After coming to a complete understanding, please build a Claude skill which will inform Claude on which styles are available when building new designs.
```

注: skill description は **1024 文字以内**制限あり。

### Components Skill 構築プロンプト (グルーピング指示)
コンポーネントは 3 グループに分けて教える:
- **Form elements** (field / input / dropdown / textarea / checkbox / radio…)
- **Navigation**
- **Data display** (table / tag / avatar / badge…)

```
Please study the following component groupings: form elements, navigation, data display.
Do not move to navigation elements unless you have a mastery of form elements.
The same with data display. Only move once you have mastered the prior group.
After coming to an elite understanding of all components properties variants and anything else associated with it,
build a Claude skill around which components are available and when to use them.
Inside of the skill, have different MD files talking about form elements, navigation, data display more in depth.
```

`properties variants and anything else associated with it` を入れないと variants 漏れが起きる (動画中 Kirk が言い忘れた失敗談)。

### Claude → Codex への skill 横展開
```
I need the design system components variables and text style skills in separate zip folders
complete with all subfolders associated with those skills.
```
→ Claude が ZIP を package → DL → Codex chat に投げて「これらを skill として作って」。両者で同一 DS skill になる。

---

## 🎨 デザイン生成プロンプト (Mobbin 画像参照)

ベスト実例:
```
Using the screenshot attached or the reference example attached
along with the variables, type styles, and component skills,
please build a page like this using our design system.

Here is our design system file if you need it: [Figma link]
But all info should be encompassed inside the Claude skills.

Do not push to Figma yet. Simply just generate it locally.
```

**込めたコツ**:
- 利用させたい skill を明示 (skill 漏れ防止)
- DS Figma ファイルリンクも添える (best practice、Codex でも有効)
- 「Figma に push しない、locally generate」を明示 (まず Claude 内で確認 → Codex で iterate → 整ってから Figma push)
- 参考画像は **複数** 渡す (1枚だと one-to-one コピー = 著作権事故)

---

## 💎 重要原則 (Kirk の主張)

### 1. AI が出来る ≠ AI でやるべき
- **Button や Field のような枯れたコンポーネントを AI に作らせるな**。5分・5000トークン使って variants 漏れの結果が出る
- AI を使うべきは **複雑な layout / modal / dialog**

### 2. 既存 DS テンプレートを買え
- $50〜$150 で完成度の高い DS テンプレートがある
- AI に variable library を生成させると `disabled` 系などの重要 variable が必ず漏れる
- 自分で作るなら 3 時間。reverse engineer する時間 > 自分で作る時間

### 3. Claude Design に丸投げするボスへの反論
- DS を Claude Design にアップロードしても、buttons は存在しない `ghost`/`danger` variant を勝手に作り、本物の variants を取りこぼす
- type scale も `display` を勝手に作り `heading 3/5/6` を消滅させる
- 「executives は clickbait しか見てない。この章 (Ch.15) を見せて反論せよ」

### 4. トークン節約のための順序
- Stitch (ほぼ無料) で stakeholder と合意 → Claude (高コスト) を一発で当てる
- Claude 内で iteration せず、Figma 経由で Codex に渡して連続変更

### 5. 視覚参照は必須
- 「dark なキッチン」を口で言うのと写真を渡すのは別物
- Mobbin or ChatGPT 5.5 で alternate options を画像生成 → AI に渡す

---

## 📘 章ごと TOC (詳細は raw transcript 参照)

| # | 章 | 時刻 | キモ |
|---|---|---|---|
| 1 | An Introduction | 00:00 | 全体宣言 |
| 2 | AI is Not a Tool | 00:52 | コアメッセージ |
| 3 | The AI Design Stack | 06:43 | 4ツール俯瞰 |
| 4 | Claude vs Codex | 07:32 | Claude=品質, Codex=効率 1/3〜1/4 トークン |
| 5 | AI Design Setup | 09:16 | Figma MCP + Skills install (Claude / Codex) |
| 6 | Google Stitch | 16:31 | Thinking 3.1 Pro, mobile強い |
| 7 | Where I Use Stitch | 21:27 | midfi wireframing, stakeholder 早期会話 |
| 8 | Claude Design | 23:29 | 詳細質問→高品質first try、ただし高コスト |
| 9 | Claude Design Output | 27:01 | senior level、1ページのみ |
| 10 | When to Use Stitch With Claude Design | 28:35 | Stitch 合意→詳細プロンプトで Claude Design 一発当て |
| 11 | Claude Design Limitation | 31:44 | design tool ではない、自由配置不可 |
| 12 | Claude vs Codex Experiment | 32:35 | 12分38k vs 4分17k |
| 13 | Best Use of AI Tokens | 38:25 | Claude→Figma→Codex→Figma→Claude ループ |
| 14 | Design Systems and AI | 40:45 | 次章への導入 |
| 15 | Claude Design DS Limitations | 42:04 | DS 自動解析は壊れる、ボスに見せろ |
| 16 | Building Design Tokens with AI | 45:32 | 3層 (Brand/Alias/Mapped) 試行、disabled 漏れる |
| 17 | Figma Variables and AI | 49:01 | AI は hallucinate、自分で組め |
| 18 | DS Components and AI | 52:26 | button 6分5k token、variants 漏れ |
| 19 | Training AI On Our DS | 58:32 | 順序: variables→components→docs |
| 20 | Design System Claude Skills | 01:01:34 | Variables skill 構築プロンプト |
| 21 | Components and Claude Skills | 01:08:04 | 3グループ化 + properties/variants 明示 |
| 22 | Adding Claude Skills to Codex | 01:13:48 | ZIP package → Codex に投入 |
| 23 | AI and Design Research with Mobbin | 01:15:09 | 視覚参照必須、複数 example |
| 24 | Building Production UI's with Claude Code | 01:17:24 | プロンプト全文 (上記) |
| 25 | Building UI's with ChatGPT | 01:22:43 | GPT 5.5 Thinking が画像生成で代替案 |
| 26 | Refining Our AI Design | 01:24:26 | ChatGPT image → Claude に投入 |
| 27 | Pushing to Figma and Final Result | 01:25:13 | push 前に Figma で磨け |
| 28 | Outro | 01:27:08 | チャンネル宣伝 |

---

## ✅ うまく行ったこと

- vidkit tutorial モードで 1h27m 動画を 2分30秒で前処理完了 (字幕2315seg + 146シーン + 1080pフレーム146枚)
- 動画は実演中心で、プロンプト文をそのまま再現可能な形で抜き出せた
- Kirk の主張は単なる tool 紹介ではなく **「役割で使い分けるワークフロー設計」** に焦点が当たっており、再現価値が高い

## ❌ 詰まったこと

- 初回 vidkit 実行で YouTube 字幕が **HTTP 429 (Too Many Requests)** に当たり、`yt-dlp` の `ignoreerrors=only_download` 設定により例外ではなく `info=None` が返ったため、既存の except 経路をすり抜けて即 `RuntimeError`
- 対応: `vidkit/fetch.py` の `_fetch_youtube` に「`info is None` でも字幕無し再試行に降りる」分岐を追加 → commit `1c91464`
- 教訓: yt-dlp は ignoreerrors 設定によって例外ではなく None を返すケースがある。例外 catch だけでなく戻り値 None も二段構えで処理すべき
- 関連: 既存コミット `bd1a24b` でも HTTP 429 対策をしていたが、今回のパターン (字幕単体失敗) はカバーされていなかった

## 📋 次回同じことをするときのチェックリスト

### この動画の学びを実践する場合
1. Claude デスクトップ + Codex を install、Google アカウントで Stitch も触れる状態に
2. Figma community skills site から `Figma Use` ZIP + `Audit Design System` skill.md を DL
3. Claude の Customize → Skills に ZIP upload → **Connectors で Install + Connect も必ず**実行 (これが落とし穴)
4. 自前 DS が無ければ $50〜$150 のテンプレートを買う (AI に作らせない)
5. DS の Figma 上に **description 付き variables table** を作る
6. 上記プロンプトで Variables / Type / Components の 3 skill を Claude で生成 → ZIP 化 → Codex にも転写
7. 初稿は Mobbin/ChatGPT で参考画像を取り、`Do not push to Figma yet. Simply just generate it locally.` を明示して Claude Code で生成
8. 微修正は Claude、連続大規模変更は Codex、最終 push は Figma 経由で Claude に戻す

### vidkit でチュートリアル動画を処理する場合
1. `cd ~/projects/vidkit && uv run vidkit tutorial <URL or local file>`
2. デフォルト出力先は `~/Downloads/vidkit_<日付>_tutorial_<id>/`
3. **Vault に直接出力するなら `--vault-path ~/ObsidianVault`** を付ける
4. HTTP 429 が出ても `1c91464` 以降は字幕無しで継続するはず
5. transcript は YouTube auto-caption で「前後重複行」が出るため、要約タスクなら重複除去スクリプトを噛ませる ([[vidkit]] 参照)

---

## 🔗 関連

- [[vidkit]] — 動画前処理 CLI
- [[claude_code]] — Claude Code 基本
- [[mcp_local_servers]] — Figma MCP / Context7 / Playwright 等
- 生素材: `raw/vidkit/tutorial/vidkit_2026-05-25_tutorial_j_ZPV10bu54/`
  - `transcript.md` (YouTube auto, 2315 segments)
  - `chapters.md` (28章)
  - `scenes.md` (146 cuts)
  - `frames/` (146枚 1080p JPEG)
  - `PROMPT.md` (Claude Code 自走用、今回未使用)
  - `meta.json`
