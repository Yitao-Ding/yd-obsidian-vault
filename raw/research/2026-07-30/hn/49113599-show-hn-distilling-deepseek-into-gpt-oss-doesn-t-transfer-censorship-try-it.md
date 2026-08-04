---
type: research
source: hn
source_id: '49113599'
url: https://www.ctgt.ai/research/distillation-censorship-transfer
published_at: '2026-07-30T18:13:06+00:00'
collected_at: '2026-07-31T02:05:46.973319+00:00'
score: 8.57
importance: 4
categories:
- eval
- safety
- code-llm
- agents
related_projects: []
implementation_difficulty: medium
authors: []
title: 'Show HN: Distilling DeepSeek into GPT-OSS doesn''t transfer censorship. Try
  it'
---

# Show HN: Distilling DeepSeek into GPT-OSS doesn't transfer censorship. Try it

> https://www.ctgt.ai/research/distillation-censorship-transfer

## 要約 (日本語5行)

DeepSeek V4 Flash を teacher モデルとして、GPT-OSS-120B に knowledge distillation を実施し、FinanceReasoning ベンチマークで 83.61% の精度を達成しました。Teacher モデルの censorship 特性（中国の政治的トピックに対する敏感さが +45.45 ポイント）が distilled student に転移しないことを実証しました。152 ペアの matched prompt（大躍進 vs ホロドモル等の概念対応）を 4 つの LLM judge と 96 の human score で検証し、r=0.948 の相関を得ました。LineageEval という定量的 evaluation framework を公開し、distillation プロセスにおける alignment leakage を可視化・測定可能にします。Subliminal learning 理論と一致：初期化が異なると teacher の安全特性は転移しない、という機械学習上の重要な知見を得ました。

## メタ
- 重要度: 4/5
- スコア: 8.57/10
- 実装難易度: medium
- カテゴリ: eval, safety, code-llm, agents

## 原文 (Abstract)

We recently used DeepSeek V4 Flash as a teacher for finance tasks with GPT-OSS-120B. Distillation works well on this problem. At a constrained 8k token budget, our self-distilled 120B scores 83.61% on FinanceReasoning, above Kimi K3 (81.93%) and Inkling (65.13%). We released the 20B open weights. With V4 as the teacher though, we realized it would be timely to measure if the censorship characteristic of it transferred to the distilled version of the base model. tl;dr it didn&#x27;t, the teacher answered politically sensitive questions 7 SDs differently than expected, but the distilled model&#x27;s behavior remained the same as its American base. You can try a couple queries yourself with no auth here: <a href="http:&#x2F;&#x2F;playground.ctgt.ai&#x2F;">http:&#x2F;&#x2F;playground.ctgt.ai&#x2F;</a><p>I will now dive in to the motivation, methodology and detailed results for those interested. The hard part of measuring this phenomena is isolating whether a model is reluctant to talk about sensitive things generally vs. a particular country&#x27;s sensitive things. So we made 152 matched pairs where one prompt asked about a Chinese concept, and the other asked about a non-Chinese version of that concept. For example, the Great Leap Forward vs. the Holodomor. These were scored 0-100 by four LLM judges (Grok 4.20, Gemini 3.5 Flash, GPT-5 mini, Claude Sonnet 4.6), validated against 96 human scores at r=0.948. OpenRouter blocked some of these so we hosted the weights ourselves.<p>The teacher&#x27;s gap on the core political set of pairs was +45.45 points, ~7 standard deviations from chance, and every distilled student was within 1 point of its base. Subliminal learning literature says this is expected when the initializations are not shared between teacher and student, which is true here. The distillation data also did not contain any China-sensitive content. The contribution here was to release the evaluation framework (LineageEval: <a href="https:&#x2F;&#x2F;github.com&#x2F;CTGT-Inc&#x2F;lineage-eval&#x2F;" rel="nofollow">https:&#x2F;&#x2F;github.com&#x2F;CTGT-Inc&#x2F;lineage-eval&#x2F;</a>) to elevate the discussion around this topic in DC and beyond. We are an interpretability lab working on high risk and regulated applications of AI, so we hear a lot of vagaries aimed at the supposed dangers of distilling Chinese models on American bases. We believe these conversations should be based on open, auditable frameworks and not feelings. We plan to test what happens with a Chinese teacher into a Chinese-lineage base like Qwen next.<p>The distillation method was an evolution of HINT-SD where we inject a hint at the specific point the model makes a mistake in its reasoning. Then we train on the corrected continuation with reverse KL over the next 100 toks of the rollout. As mentioned above 120B itself was efficacious as a teacher, and we ended up shipping this version. The self-distilled 120B scores 83.61% on FinanceReasoning, above Kimi K3 (81.93%) and Inkling (65.13%). Ours finishes 98.7% of problems in budget; the larger models truncate (90.76% and 71.01%) which score as incorrect. At 100k tokens big models gain (Kimi 89.92%). So for a finance task at a constrained (perhaps more realistic) budget a 120B on one H100 at ~$0.00026&#x2F;query outpaced models running 62-160x more per query.<p>We put out the 20B finance model as open weights (64.71% to 74.79% at 8k on FinanceReasoning, 23% lower cost&#x2F;query, runs on one 80GB GPU), the 120B in a playground with teacher and students side by side (a few queries, no auth), and LineageEval with all prompts, controls, rubric, and code.<p>We are curious to hear experiences from those working with distilled Chinese models in prod, or if you have thoughts on improvements to LineageEval.<p><a href="https:&#x2F;&#x2F;huggingface.co&#x2F;ctgt-inc&#x2F;gpt-oss-20b-finance" rel="nofollow">https:&#x2F;&#x2F;huggingface.co&#x2F;ctgt-inc&#x2F;gpt-oss-20b-finance</a><p><a href="https:&#x2F;&#x2F;playgr


*Generated by ai-researcher at 2026-07-31T02:05:46.974215+00:00*