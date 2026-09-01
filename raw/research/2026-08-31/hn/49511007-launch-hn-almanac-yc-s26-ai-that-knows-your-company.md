---
type: research
source: hn
source_id: '49511007'
url: https://usealmanac.com/
published_at: '2026-08-31T15:34:34+00:00'
collected_at: '2026-09-01T08:43:36.025584+00:00'
score: 9.05
importance: 4
categories:
- agents
- rag
- infra
- tooling
related_projects:
- lecture_hub
- morning_briefing
- task_hub
implementation_difficulty: medium
authors: []
title: 'Launch HN: Almanac (YC S26) – AI that knows your company'
---

# Launch HN: Almanac (YC S26) – AI that knows your company

> https://usealmanac.com/

## 要約 (日本語5行)

Almanac は個人・チーム向けの記憶ベースのエージェントで、Gmail・Calendar などのアカウントと自動連携して個人Wiki と企業Wiki を構築し、文脈を保持したAIアシスタントを実現しています。従来のHermesセットアップの煩雑さ(OAuth構築・記憶管理)を解決する設計が特徴です。LLMの記憶層を事前コンパイルされたWiki(RAG的知識ベース)で実装することで、正確なコンテキスト検索を達成しています。プロアクティブなエージェント動作も可能で、バックグラウンドワーカーがタスク完了を自動検出して提案します。講義ハブやモーニングブリーフィングのような知識パイプラインを構築する際の参考になる、実装レベルのアーキテクチャパターンです。

## 既存プロジェクトとの関連
- **lecture_hub** — 個人ナレッジハブ (Next.js + Supabase Postgres + pgvector + BlockNote + AI SDK)
- **morning_briefing** — 毎朝の音声+PDFブリーフィングパイプライン (Vault raw/ を読みに来る)
- **task_hub** — サークル向けタスク管理 (Next.js + Firebase)

## メタ
- 重要度: 4/5
- スコア: 9.05/10
- 実装難易度: medium
- カテゴリ: agents, rag, infra, tooling

## 原文 (Abstract)

Hi HN, I&#x27;m Kushagra, one of three founders of Almanac, a Hermes with a brain that knows everything about your company.<p>We started our journey with setting up Hermes for our company, thinking it must be easy. We wanted an agent that would know every context about our company, so we could ask questions and get context-appropriate responses to.<p>This started a very annoying and difficult journey. Setting up Hermes, getting it to talk right, building OAuth apps for every connector myself, then feeding it context myself, and ultimately struggling with Hermes&#x27;s default memory. At the same time, we saw our YC batchmates struggling with the same problem, and we saw an opportunity.<p>So we built Almanac. This is how it works. You sign up, you get a Hermes agent straight out of the box. You have a one-click connect to any account (Gmail, Calendar, Granola, PostHog, etc). You have personal accounts (only accessible by you) and also shared accounts (accessible by everyone in the company). The consequence being I can never see my cofounders&#x27; accounts.<p>The “brain” of this agent is wikis. We pull in information from your connected sources, and start organizing this information in two wikis. A personal one, for you, which understands who you are, what your preferences are, the people in your life, and the things going on in your life. The second wiki is a company wiki, which includes what the company is, what you’re working on, what the roadmap is, and what the blockers of the company are. Your agent ultimately has access to these two wikis and the original accounts, which invoke the feeling of “it just knows you.”<p>Here’s a demo: <a href="https:&#x2F;&#x2F;www.youtube.com&#x2F;watch?v=ajXP5PHuK18" rel="nofollow">https:&#x2F;&#x2F;www.youtube.com&#x2F;watch?v=ajXP5PHuK18</a><p>We&#x27;re three cofounders, Rohan, Kushagra, and Divit, and we&#x27;ve been friends for 11 years, since studying for the IIT-JEE. We all did Electrical Engineering (Rohan at IIT Delhi, me at IIT Kharagpur, Divit at BITS Pilani, Hyderabad), and Rohan and I later went to Harvard, where this pre-compilation layer became our capstone thesis. We have built multiple products around the idea of a pre-compiled knowledge layer.<p>Our main differentiating point is the way we approach memory and context in general. Most AI assistant tools treat memory as an afterthought. We have worked on wikis for AI for more than a year now, building products for Harvard and NASA. The one thing we have learnt is that one needs to spend a lot more compute upfront, in the pre-compilation of this knowledge base, to get it right.<p>Having this pre-compiled knowledge base enables a lot of interesting ideas. First is a proactive agent. Since I have compiled what’s going on in both my company and my current life, Almanac can start completing tasks on its own. Concretely, we run a background worker which takes a look at tasks that could be completed, pings the main agent, who then pings me, suggesting which tasks it could automate. As a result, I wake up to proactive notifications which look like “I already prepared a draft of your fundraising pitch deck, want to take a look?”<p>Second, long-horizon tasks. In our wikis, we maintain a section on ongoing projects, so Almanac can pick a task back up days later without losing the thread. Most agents are session-bound: they run once, finish, and forget. But a lot of real work isn&#x27;t one shot; it plays out over hours and days with people in the loop. The clearest example is anything that involves waiting on a human, like scheduling a meeting, following up on a sales thread, or chasing a document. Almanac can send an email on your behalf, and because it&#x27;s always on and remembers the project, it notices the reply four hours later and drafts the right follow-up in context.<p>Since launching, we&#x27;ve seen a lot of use cases for Almanac. One person runs her dog-rescue operation through it: finding available fosters, tracking picku


*Generated by ai-researcher at 2026-09-01T08:43:36.026762+00:00*