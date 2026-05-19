---
type: cert_log
cert_name: AWS Certified AI Practitioner (AIF-C01)
provider: Amazon Web Services
status: not_started
target_date: 2026-07-15
last_updated: 2026-05-19
priority: high
tags: [certification, aws, ai-practitioner, aif-c01, generative-ai]
---

# AWS Certified AI Practitioner (AIF-C01)

> CCA-F (Anthropic Partner Network 経由のみ受験可) の代替として 2026-05-19 に採用。
> 個人受験可能、コスト $100、Cloud Practitioner より易しい評価。
> AI/ML/Generative AI on AWS の基礎を validate する Foundational tier 認定。

---

## 📊 試験概要 (2026-05-19 時点、web調査済み)

| 項目 | 内容 |
|------|------|
| 試験コード | AIF-C01 |
| 問題数 | 65問 (multiple-choice + multiple-response) |
| 試験時間 | 90分 |
| 合格点 | 720 / 1000 (scaled score) |
| 価格 | $100 USD (地域により $75 程度のことも) |
| 形式 | ローカルテストセンター or オンラインプロクター (Pearson VUE) |
| 受験条件 | **個人受験OK**、前提資格・組織加盟不要 |
| 言語 | 英語 (日本語対応見込み、要確認) |
| 有効期限 | 3年 |
| 公式 | https://aws.amazon.com/certification/certified-ai-practitioner/ |

---

## 🎯 5領域 (出題比率)

| # | ドメイン | 比率 | YDの現状理解 |
|---|---------|------|------------|
| 1 | Fundamentals of AI and ML | 20% | ★中 (概念理解は十分、AWS固有用語の確認が必要) |
| 2 | Fundamentals of Generative AI | 24% | ★高 (Claude/Gemini 使用経験、LLM の理解は深い) |
| 3 | Applications of Foundation Models | 28% | ★中 (Bedrock 未経験、ここが最大の学習領域) |
| 4 | Guidelines for Responsible AI | 14% | ★中 (Anthropic Academy AI Fluency で補強済み想定) |
| 5 | Security, Compliance, and Governance | 14% | ★低 (AWS Security 未学習) |

**Bedrock が最大の弱点**: Anthropic Academy の `12_claude_with_amazon_bedrock.md` が直接の前提教材になる。

---

## 📅 準備スケジュール

```
2026-05-19 ─┬─ Anthropic Academy スプリント開始
            │   ※ 特に 12_claude_with_amazon_bedrock が AIF-C01 の Domain 3 (Foundation Models 28%) と直結
            │
2026-06-02 ─┼─ Academy 18コース完走
            │
2026-06-03 ─┼─ AWS Skill Builder で公式無料コース受講開始
            │
2026-06-15 ─┼─ Tutorials Dojo / Stephane Maarek の Udemy講座 ($20前後) 1本受講
            │
2026-06-25 ─┼─ 模擬問題でセルフチェック (Tutorials Dojo の practice exam 6本程度)
            │
2026-07-15 ─┼─ ★ AIF-C01 本試験受験 ★ (オンラインプロクター)
            │
2026-07-16 ─┴─ 結果次第で再受験 or LinkedIn更新
```

学習時間目安: **30-40時間** (AWS未経験のため)。Anthropic Academy の Bedrock コースで 5-10 時間先取り済み扱い。

---

## ✅ 準備状況チェックリスト

### 受験前に必須

- [ ] AWS アカウント作成 (`save.yitao@gmail.com` で新規 or 既存)
- [ ] AWS Skill Builder 無料アカウント登録
- [ ] 50% 割引バウチャー (AWS の不定期キャンペーン) チェック
- [ ] Pearson VUE アカウント作成 → 試験予約
- [ ] 模擬試験 6本以上で 80% 以上安定
- [ ] Bedrock のハンズオン経験 (無料枠で 1 時間でも触る)
- [ ] 5領域それぞれで Skill Builder + Academy 該当コース完了

### 学習リソース

- 公式: https://aws.amazon.com/training/learn-about/ai/
- 公式無料コース: AWS Skill Builder (検索: AIF-C01)
- Anthropic Academy: 12_claude_with_amazon_bedrock (Domain 3 直結) → [[../anthropic_academy/12_claude_with_amazon_bedrock]]
- Udemy: Stephane Maarek の Ultimate AWS Certified AI Practitioner (DataCumulus でクーポン取得 → $20 前後)
- Udemy: Frank Kane の Mastering AWS Certified AI Practitioner (Sundog Education でクーポン → $20 前後)
- 模擬試験: Tutorials Dojo AIF-C01 Practice Tests (公認パートナー)
- 公式 Exam Guide PDF (aws.amazon.com から無料DL)

---

## 📌 YDへの引き寄せ

- AI Engineering 系で AWS の公式 credential を取ることで「Google + AWS の AI 資格を揃える」状態を作れる (差別化)
- 進路決定後の AI Engineer ポジションで「Bedrock 触れる」は地味に強い (Claude on AWS は企業導入が多い)
- CCA-F が組織所属待ちなのに対して、こちらは個人でいつでも受験可能 → スプリント計画が破綻しない
- AI Practitioner → AWS Solutions Architect Associate → Machine Learning Specialty へのステップアップが明確
- Foundational tier なので、就活的には「AI 興味あります」のシグナル止まり (深い専門性の証明にはならない、過大評価しない)

---

## 💡 学習中ログ

(受講・受験を進めるごとに追記)

## ✅ うまく行ったこと

(受験後に記入)

## ❌ 詰まったこと

(受験後に記入)

## 📋 次回同じことをするときのチェックリスト

(受験後に「次に AIF-C01 を受ける人がスムーズに進めるための注意点」を書く)

## 🔗 関連

- 親: [[../README|ai_certifications dashboard]]
- 前提: [[../anthropic_academy/README]] (特に 12_claude_with_amazon_bedrock)
- 代替元: [[../claude_certified_architect/README]] (Partner Network 制約により延期、所属確定後に再判断)
- 次: [[../google_ai_professional/README]] (Coursera、6-7月並行)
- スプリント決定: [[../../../decisions/2026-05-19_AI学習スプリント開始]]
