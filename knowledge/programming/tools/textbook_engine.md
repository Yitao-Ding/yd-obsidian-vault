---
type: knowledge
domain: programming/tools
last_updated: 2026-05-19
status: active
related: [obsidian_vault, claude_code, ai学習スプリント]
---

# textbook-engine — YD専用教科書 Markdown→PDF パイプライン

> Markdown で書いた教材を、**雑誌風スタイリッシュな縦長A4 PDF** に1コマンドで変換するツール。
> iPhoneで通勤中に読めるレイアウト。Mermaid図・コードハイライト・カバーページ・目次・奥付すべて自動。

---

## どこにある? どう動かす?

| 項目 | 値 |
|------|---|
| プロジェクトパス | `~/projects/textbook-engine/` |
| 教材リポジトリ | `~/ObsidianVault/textbook/` |
| PDF出力先 | `~/ObsidianVault/textbook/_output_pdf/` (自動判定) |
| 言語 | Python 3.9+ |
| 主要依存 | WeasyPrint, markdown, pymdown-extensions, Pygments, Jinja2, PyYAML |
| 外部CLI | mmdc (`npx -y -p @mermaid-js/mermaid-cli mmdc`) |
| 状態 | ローカルのみ (git 未化、必要なら後でPrivate push) |

```bash
# 初回 (1回だけ)
cd ~/projects/textbook-engine
./setup.sh

# 教材1冊をPDFに
./build.sh ~/ObsidianVault/textbook/03_ai_engineering/01_claude_code_parallel.md
# → ~/ObsidianVault/textbook/_output_pdf/01_claude_code_parallel.pdf
```

---

## アーキテクチャ

```text
.md (frontmatter + Markdown + Mermaidブロック)
  │
  ├─ parse_frontmatter         (YAML)
  ├─ replace_mermaid_blocks    (mmdc で PNG 生成、SHA1 ハッシュキャッシュ)
  ├─ markdown_to_html          (markdown + pymdown-extensions + Pygments)
  ├─ render_html (Jinja2)      (template.html + style.css)
  └─ WeasyPrint                (HTML → A4縦 PDF)
```

ファイル構成:
```
textbook-engine/
├── setup.sh          (brew で pango/cairo/gdk-pixbuf/libffi + Python venv + pip)
├── build.sh          (薄ラッパー、DYLD_FALLBACK_LIBRARY_PATH を通す)
├── requirements.txt
├── src/build.py      (本体ロジック ~210 行)
├── templates/template.html  (Jinja2)
└── templates/style.css      (雑誌風スタイル ~400 行)
```

---

## デザインの方針

YDの好み (寒色・ネイビー基調・スタイリッシュ・装飾過剰NG) を CSS で反映:

- **カラー**: ネイビー `#0a2540` / アクセント `#4f46e5` (青紫) / 紙 `#fff` / 罫線 `#d6dbe6`
- **フォント**: macOS標準ヒラギノ角ゴ ProN (依存ゼロで美しい) + 等幅 SF Mono
- **カバー**: ネイビーのラジアルグラデーション + アクセント円のオーバーレイ + Kicker badge + No.XX.YY 番号
- **本文 h1**: 採番プレフィックス `01 ─` (SF Mono、アクセント色)
- **本文 h2**: 左に縦線 (アクセント → ホット のグラデーション)
- **コード**: ダーク背景 `#1e1f29` + 左ボーダー + Pygments `one-dark`
- **テーブル**: ヘッダー ネイビー / 偶数行 薄背景でゼブラ
- **Mermaid図**: 薄背景の角丸カードに収める
- **strong**: 蛍光ペン風の下線 (warm)、**em**: アクセント色強調
- **奥付**: `YD textbook` ブランド + `Generated with textbook-engine` の小さなクレジット

---

## frontmatter 仕様

```yaml
---
title: タイトル (必須、これがページヘッダにも入る)
subtitle: サブタイトル (1行)
kicker: AI Engineering          # カバー上部のラベル
number: "03.01"                 # 領域番号.通し番号
series: 03_ai_engineering       # 領域ディレクトリ名
tags: [tag1, tag2, tag3]
date: 2026-05-19
level: ゼロ前提                  # 対象レベル
---
```

---

## ✅ うまく行ったこと

- **WeasyPrint 採用が大正解**: reportlab だとレイアウトを1点ずつ計算する地獄、Playwright はchromium重すぎ、WeasyPrintは「HTML + CSS Paged Media」で書けるので雑誌っぽい装飾も思いのまま。日本語フォントもfontconfig経由でMacのヒラギノが普通に効く
- **Mermaid をPNGに事前レンダリング**: WeasyPrintはJS実行しないので、Mermaidブロックを正規表現で抜いて mmdc で PNG にしてから `<img>` 置換。SHA1 ハッシュでキャッシュするので2回目以降は爆速
- **ヒラギノ角ゴ ProN がそのまま使える**: Noto Sans CJK を別途インストールせずとも Mac標準フォントだけで美しく出る。`font-family: "Hiragino Kaku Gothic ProN", "Hiragino Sans", sans-serif` で OK
- **テンプレート + スタイル分離**: `template.html` (Jinja2) と `style.css` を完全分離したので、デザイン変更は CSS 1ファイル触るだけ。Markdownや本体コードは触らない
- **build.sh の自動判定**: 出力先 `_output_pdf/` を「.md パスから親を遡って `textbook/` ディレクトリを見つける」ロジックで自動決定。`-o` 明示も可。シェル芸的だが便利
- **設計判断は全部自分で決めた**: YD確認なしで進めた箇所 — ライブラリ選定 / フォント / レイアウト / カラーパレット / Mermaidレンダリング方式 / キャッシュ戦略 / 出力先決定ロジック。完了報告ベースで OK と判明

## ❌ 詰まったこと

- **WeasyPrint が `libgobject-2.0-0` を見つけられない (Apple Silicon)**: `brew install pango` した後でも `OSError: cannot load library 'libgobject-2.0-0'`。原因はMacの dyld が `/opt/homebrew/lib` を見ない (Intel Mac の `/usr/local/lib` は見るが Apple Silicon は別パス)
  - **解決**: `build.sh` で `export DYLD_FALLBACK_LIBRARY_PATH="/opt/homebrew/lib:..."` を起動時に通す。venv の activate と同じパイプで仕込んだ
- **WeasyPrint が `var(--navy-deep)` を radial-gradient 内で解決できずクラッシュ**: `TypeError: 'NoneType' object is not iterable` (css/__init__.py の resolve_var内)
  - **解決**: gradient内のCSS変数はハードコードに置き換える (`var(--navy-deep)` → `#051728`)。`:root` で定義した変数を gradient で使うのは WeasyPrint で危ない。color プロパティ単体での使用は問題ない
- **目次タイトル `::before` の `display: block` が完全には効かない**: TOC タイトルの上に小さな `Contents · ` プレフィックスを置きたかったが、WeasyPrint で擬似要素がインライン化される問題でわずかに重なる。視覚的には致命的でないので保留 (将来直す)

## 📋 次回 教材を1冊追加するときのチェックリスト

- [ ] `~/ObsidianVault/textbook/_template/textbook_template.md` をコピー
- [ ] 対応する領域 (例: `03_ai_engineering/`) に番号付きファイル名で置く (例: `02_xxx.md`)
- [ ] frontmatter を埋める (title / subtitle / kicker / number / series / tags / date / level)
- [ ] 7セクションを書く: 1.何が起きた / 2.図解 (Mermaid必須) / 3.キーコンセプト / 4.コード解説 / 5.チェックリスト / 6.関連リンク / 7.用語集
- [ ] 専門用語は **初出時に必ず本文内で短く注釈** + 用語集にも入れる
- [ ] `cd ~/projects/textbook-engine && ./build.sh <path>` で PDF 化
- [ ] `_output_pdf/` の PDF を開いて表示確認 (preview.app で見るのが手っ取り早い)
- [ ] `~/ObsidianVault/textbook/README.md` の目次表に追記
- [ ] (任意) Google Drive にアップロード — セッションB の自動配信が動き出したら自動化される

### setup-時の落とし穴

- [ ] Homebrew 必須。Apple Silicon Mac なら `/opt/homebrew` 配下に入る
- [ ] `brew install pango cairo gdk-pixbuf libffi` は setup.sh が自動でやる
- [ ] node/npx 必須 (mermaid CLI 用)。なければ `brew install node`
- [ ] Python 3.9+ 必須 (macOS の `/usr/bin/python3` で OK)
- [ ] venv は `.venv/` に作る (リポジトリ直下)

### 共有時の注意

- [ ] PDF をチームに渡すなら Google Drive に置く (セッションB の自動アップが動いたら一元化)
- [ ] iPhone で開くと縦スクロールで読める (A4縦は iPhone Pro Max 系で1ページ全画面表示が綺麗)
- [ ] テキスト読み上げ機能 (iOS の `読み上げ`) を併用すると勉強効率倍増

---

## 関連リンク

- [[obsidian_vault]] — Vault 全体の運用マニュアル
- [[claude_code]] — Claude Code 全般
- [[2026-05-19_AI学習スプリント開始]] — このシステム誕生の経緯
- [[active_projects]] — 教科書システムの現状ステータス
- 公式: https://doc.courtbouillon.org/weasyprint/stable/ — WeasyPrint ドキュメント
- 公式: https://mermaid.js.org/ — Mermaid 図 文法
- 公式: https://python-markdown.github.io/extensions/ — Markdown拡張
