---
type: research
source: hn
source_id: '49713894'
url: https://github.com/pizza-bot-app/pizza-bot
published_at: '2026-09-15T15:20:26+00:00'
collected_at: '2026-09-16T02:06:00.344269+00:00'
score: 10.0
importance: 4
categories:
- agents
- tooling
- infra
related_projects:
- task_hub
- vidkit
- morning_briefing
implementation_difficulty: medium
authors: []
title: 'Show HN: Pizza Bot – An inbox for AI agents that work in the background'
---

# Show HN: Pizza Bot – An inbox for AI agents that work in the background

> https://github.com/pizza-bot-app/pizza-bot

## 要約 (日本語5行)

Pizza Bot は自走 AI Agent をメール型 UI で管理するセルフホストデスクトップアプリ。MCP サーバー連携で複数 LLM プロバイダ対応。Amazon 内 2,000 人が 18ヶ月運用した実績から OSS 化。YD の agentic AI + MCP + background automation への直結テーマ。タスク自動実行・結果の非同期通知パターンは vidkit・morning-briefing の次フェーズの設計に応用可。infrastructure-as-code で local-first・privacy-first を実現し、MCP server を recipes (自動化ワークフロー) として束ねるアーキテクチャは lecture_hub の AI 駆動改訂と共通の問題領域。Electron + Tailscale + local model 構成は現在のビサヤ語アプリ (relay サーバー + 4 人テスト) の次段階スケーリングのヒント。

## 既存プロジェクトとの関連
- **task_hub** — サークル向けタスク管理 (Next.js + Firebase)
- **vidkit** — Final Cut Pro 用の動画前処理 CLI (FCPXML 1.13, autocut, tighten, tutorial, dance)
- **morning_briefing** — 毎朝の音声+PDFブリーフィングパイプライン (Vault raw/ を読みに来る)

## メタ
- 重要度: 4/5
- スコア: 10.00/10
- 実装難易度: medium
- カテゴリ: agents, tooling, infra

## 原文 (Abstract)

Hi HN - long-time lurker (since 2012!), first time poster.<p>Pizza Bot is a self-hosted desktop app for Mac, Windows, and Linux that runs AI agents in the background and exposes them through an email-like UI. Finished work shows up in Unread, and anything waiting on your approval shows up in Action. It&#x27;s Apache 2.0-licensed, there&#x27;s no signup and no telemetry, and you bring your own model provider: Anthropic, Amazon Bedrock, Google Gemini, OpenAI, OpenRouter, or a local model through Ollama. There are builds on the releases page, or you can run it from source.<p>Pizza Bot started as an internal passion project I worked on with a small team at Amazon.<p>The whole thing came out of my frustration at having to manually log CRM activities through a browser form. I built a simple REST API called &quot;JoeBot&quot; that connected to my authenticated browser session over CDP and filled out the form for me using Playwright. Then I hacked up a quick Obsidian plugin so I could trigger it from my local notes (no AI and no MCP servers involved).<p>This caught on quickly. My fellow AWS Solutions Architect Igor Fil joined up with me, and we rebranded the project as &quot;Pizza Bot,&quot; named after Amazon&#x27;s two-pizza teams. We started seeing what other automations we could build. We found a GraphQL API we could query and hacked up some &quot;recipes&quot; to pull data out of the CRM to help with meeting prep. That worked great, and it was right around the time MCP servers seemed to be taking off, so we decided to expose Pizza Bot as an MCP server instead, so it would be available to AI tools through natural language.<p>This was a decent solution for technical users, but the Account Managers who live inside our CRM system wanted something too. We decided to rebuild Pizza Bot as an Electron desktop app modeled after an email inbox, so it would be familiar to non-technical users and would run on both Mac and Windows. We also bundled internal MCP servers as OCI images and hosted them in Amazon ECR as an &quot;addon marketplace&quot; so users could install them with one click without having to set up Amazon developer tooling.<p>The project took off organically and expanded outside of AWS into the wider Amazon organization globally. More than 2,000 people ended up using it for meeting prep, email drafting, Slack summaries, CRM logging, prioritizing their day, and web research.<p>Once apps like Claude Cowork and Amazon&#x27;s own Quick Desktop came out, we realized the real growth opportunity was outside of Amazon. Rather than try to rip out the Amazon-specific integrations, we rebuilt Pizza Bot once more as an open source project. We leaned on coding agents heavily, which is the only reason a team our size could pull off a full rewrite. I&#x27;m pleased to say it&#x27;s finally public, and we&#x27;re hoping to bring in community members and see where it goes. We&#x27;d like to do for knowledge workers what Claude Code and Codex have done for programmers.<p>A couple of things to know up front. Most of what made Pizza Bot useful on day one inside Amazon came from that internal catalog of skills and MCP servers for Amazon&#x27;s own systems, and none of it could come out with the app. So it ships thinner than the version those 2,000 people used, and building that catalog back up for tools other people actually use is where we need the most help. It&#x27;s also a community project and not an AWS service, so there&#x27;s no support or SLA behind it. The Windows and Linux builds aren&#x27;t signed yet either.<p>On the technical side, Pizza Bot is a server and a client. The desktop app bundles both, or you can point a client at a remote backend; personally, I self-host the server on my home network and reach it from my phone over Tailscale. The server owns the thread lifecycle and checkpoints state with DeepAgents and LangGraph, and clients rehydrate from it as needed, so you can disconnect mid-run and pick the thread back up from anothe


*Generated by ai-researcher at 2026-09-16T02:06:00.345519+00:00*