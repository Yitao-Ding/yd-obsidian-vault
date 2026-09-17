---
type: research
source: hn
source_id: '49743049'
url: https://news.ycombinator.com/item?id=49743049
published_at: '2026-09-17T16:22:15+00:00'
collected_at: '2026-09-17T21:04:17.864561+00:00'
score: 10.0
importance: 4
categories:
- agents
- tooling
- infra
related_projects: []
implementation_difficulty: high
authors: []
title: 'Launch HN: Skillsync (YC W26) – AI chat sessions made portable across agents'
---

# Launch HN: Skillsync (YC W26) – AI chat sessions made portable across agents

> https://news.ycombinator.com/item?id=49743049

## 要約 (日本語5行)

異なるエージェント形式に散在する AI チャットセッションのポータビリティが現状で失われており、ユーザーは単一プロバイダーにロックインされています。Skillsync は ffmpeg や pandoc のようなオープンソース Rust エンジン txcript を使用して、セッション全体（メッセージ・推論・ツール呼び出し）をエージェント間で翻訳します。ローカルファースト設計でセッション管理と形式変換を個人マシン上で完結させ、共有時のみサーバーを経由します。スキルとメモリはポータブルな Markdown として格納され、MCP 経由で任意のエージェントに公開・検索できます。セッションは使い捨てログから協調作業の単位へと昇華し、パターン認識やチームメイト間での引き継ぎが可能になります。

## メタ
- 重要度: 4/5
- スコア: 10.00/10
- 実装難易度: high
- カテゴリ: agents, tooling, infra

## 原文 (Abstract)

Hey HN, we&#x27;re Nars &amp; Nishant, founders of Skillsync (<a href="https:&#x2F;&#x2F;skillsync.com">https:&#x2F;&#x2F;skillsync.com</a>)<p>Skillsync lets you move your AI chats across every coding agent. Most of our work exists as conversations, which are currently scattered across our agents. Though stored locally, these conversations use different formats. This is annoying because you cannot simply switch between agents without starting over. We get locked into a single provider and their agent as we invest in skills and memories over time. Skillsync acts as a universal converter. It moves the entire session, including all the messages, reasoning and tool calls so you can pick up right where you left off.<p>Skillsync collects all your sessions in one place and makes them searchable. It breaks down what each session is carrying, including which loaded skills the agent is actually using, making stale context easy to spot. You can also create shared workspaces to sync sessions across your team. You can build your own closed loop systems. Everything runs locally except when you share to workspaces.<p>The core is an open-source Rust engine called txcript (<a href="https:&#x2F;&#x2F;github.com&#x2F;skillsynchq&#x2F;txcript" rel="nofollow">https:&#x2F;&#x2F;github.com&#x2F;skillsynchq&#x2F;txcript</a>). It translates a session from one agent&#x27;s on-disk format into another&#x27;s, mapping conversation, reasoning, and tool history. Think ffmpeg or pandoc, but for agent sessions.<p>On top of that engine is a local-first desktop app. Your agent sessions are normally scattered across different tools&#x27; folders in formats you&#x27;d never read by hand; the app surfaces them in one place with a UI that makes them actually readable, the conversation, the reasoning, and the tool calls, so you can revisit what happened, move a session into another agent, or share it with a teammate.<p>Skills and memory are stored as portable, human-readable markdown you own, and exposed to any agent over MCP for search and selective retrieval.<p>Sessions and translation run locally on your machine. The one thing that leaves is what you explicitly share into a team workspace.<p>It’s been surprising to us to see how locked-in people already are without realizing it. You don&#x27;t notice you&#x27;re trapped in one agent or harness until you try to leave, and by then you&#x27;ve got months of sessions stuck in a format only that tool can read. The lock-in is invisible right up until it&#x27;s expensive.<p>Once sessions are portable, they stop being disposable logs and become something you can actually build on, spotting patterns in how you work, handing a session off to someone else, letting a non-technical teammate pick up where an engineer left off. The session turns out to be the unit of collaboration, and right now it&#x27;s being thrown away.<p>Before Skillsync, Nars and I built an open-source payment orchestrator that let merchants route across many processors instead of getting locked into one (30k+ GitHub stars). Fighting vendor lock-in by making incompatible systems interoperate was the whole job. We came into YC with a different idea, but the more we lived inside coding agents the more we saw the same problem from the other side: your context is locked into whatever agent you start with, because every agent stores sessions differently and none of it moves.<p>Some of the things we’re seeing people do with Skillsync are:<p>- Moving a session between Claude Code, Codex, and Cursor mid-task, including when you hit a usage limit on one and want to keep going on another.<p>- Thinking through a problem in a browser Claude chat, then handing the whole thread to Claude Code or Codex to build.<p>- Sharing research sessions with a team. Everyone uploads their sessions, so teammates and their agents can build on each other&#x27;s work instead of repeating it.<p>It&#x27;s available as a Mac app, CLI and MCP. Here&#x27;s the demo: <a href="https:&#x2F;&#


*Generated by ai-researcher at 2026-09-17T21:04:17.865843+00:00*