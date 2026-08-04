---
type: knowledge_index
domain: filmmaking
created: 2026-07-27
last_updated: 2026-07-27
tags: [video_analysis, youtube, 構成研究]
---

# 動画構成研究 - 蓄積フォルダ

## 目的

参考にしたい動画(ドキュメンタリー、vlog等)の構成・演出をGeminiで文書化して蓄積し、数本貯まったら横断分析して自分の動画の構成設計に活かす。

## 運用フロー

1. YouTube埋め込みGeminiに構成研究用プロンプト(②)を投げる
   - プロンプト本体: [[knowledge/programming/workflows/youtube_gemini_prompts|YouTube→Gemini文書化プロンプト集]]
2. 出力をこのフォルダに「1動画 = 1ノート」で保存
3. 5本前後貯まったらClaudeに横断分析させる

## ノート命名規則

`YYYY-MM-DD_チャンネル名_動画タイトル.md`
例: `2026-07-27_natgeo_okavango_doc.md`

## ノートテンプレート

```
---
type: video_analysis
channel:
url:
genre:
analyzed:
---

# (動画タイトル)

## Gemini出力

(貼り付け)

## 自分の気づき

- 盗みたい点:
- 自分ならこうする:
```

## 横断分析プロンプト(Claude用)

数本貯まったら、このフォルダをClaudeに読ませて:

> このフォルダの動画分析ノートを横断して、
> 1. 冒頭フックの共通パターン
> 2. 視聴維持の仕掛けの頻出手法
> 3. 構成テンプレとして抽出できる「型」
> をまとめてください。次に作る動画「(概要)」への適用案も出してください。

## 分析済み動画一覧

- (まだなし)
