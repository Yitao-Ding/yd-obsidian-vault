---
type: research
source: hn
source_id: '48937020'
url: https://news.ycombinator.com/item?id=48937020
published_at: '2026-07-16T16:52:16+00:00'
collected_at: '2026-07-17T02:05:27.500402+00:00'
score: 10.0
importance: 4
categories:
- agents
- tooling
- safety
related_projects:
- salamat_wbs
- task_hub
implementation_difficulty: high
authors: []
title: 'Launch HN: Traceforce (YC S26) – Company-wide security monitoring for AI apps'
---

# Launch HN: Traceforce (YC S26) – Company-wide security monitoring for AI apps

> https://news.ycombinator.com/item?id=48937020

## 要約 (日本語5行)

Traceforce は企業の全デバイスで動作する Claude・ChatGPT などの AI アプリを監視し、MCP 経由のデータ漏洩リスクを検出する企業向けセキュリティ SaaS です。動的な MCP ペンテスト、tool call の検査、リアルタイムアラート機能を備え、テックスタートアップの AI 導入に伴う「可視性の喪失」問題に直接対応します。企業側は従業員の生産性を損なわないまま、AI アプリの安全性と compliance を一元管理できるのが差別化点です。MCP（Model Context Protocol）のセキュリティについてはまだ産業レベルの認識が浅く、Traceforce のような先行プレイヤーが出てきた段階であり、今後数年で金融・ヘルスケア・製造など規制産業での採用が加速する見通しが立ちます。

## 既存プロジェクトとの関連
- **salamat_wbs** — Salamat 公式サイト (Next.js 16 + Tailwind v4 + three + cobe + shaders)
- **task_hub** — サークル向けタスク管理 (Next.js + Firebase)

## メタ
- 重要度: 4/5
- スコア: 10.00/10
- 実装難易度: high
- カテゴリ: agents, tooling, safety

## 原文 (Abstract)

Hey HN, we’re Xia and Varun, the founders of Traceforce (<a href="https:&#x2F;&#x2F;www.traceforce.ai&#x2F;" rel="nofollow">https:&#x2F;&#x2F;www.traceforce.ai&#x2F;</a>). Traceforce provides visibility and control over AI apps such as ChatGPT, Claude etc directly on all devices (laptops, sandboxes, virtual machines) by discovering not just which apps are being used but also how they are connected to other data sources via MCPs. We also have an open-source dynamic MCP pentesting tool <a href="https:&#x2F;&#x2F;github.com&#x2F;traceforce&#x2F;mcp-xray" rel="nofollow">https:&#x2F;&#x2F;github.com&#x2F;traceforce&#x2F;mcp-xray</a> to detect vulnerable MCPs.<p>The purpose of Traceforce is to:<p>- Give a company’s employees a standardized way to ensure that AI software running on their device is operating safely<p>- Give the company’s security team visibility of the activities of AI software on the company’s devices, and to detect and prevent unsafe actions and security breaches as early as possible.<p>How Traceforce works<p>1. Traceforce is installed on each device as a lightweight binary and browser extension.<p>2. Within 30 minutes, the device is uploading live data to the company profile, displaying all the AI agents&#x2F;apps running across all company devices on a dashboard.<p>3. Company security staff can monitor the activity of all the agents in real time, implement controls, and be alerted to any security risks as soon as they arise.<p>Here’s the video demo: <a href="https:&#x2F;&#x2F;youtube.com&#x2F;watch?v=IdK2WKg7kaM" rel="nofollow">https:&#x2F;&#x2F;youtube.com&#x2F;watch?v=IdK2WKg7kaM</a><p>The inspiration for Traceforce came via Xia’s experience as Director of Engineering at a startup called Clumio (which was acquired by Commvault in Oct 2024). Being able to monitor how team members are using AI without slowing them down was a top priority at Clumio. After speaking with 50+ CISOs and CIOs, it became clear that this is a much-needed solution right now across industries. We keep hearing that new AI features are being adopted so quickly and so broadly that visibility and control just can&#x27;t keep up.<p>Traceforce is transparent about what we monitor and collect. By default, Traceforce collects only metadata and telemetry about the AI applications, MCPs, and tools running on a device. Security teams can enable options to inspect tool calls for the purpose of detecting, warning on, or blocking predefined high-risk or potentially destructive actions. All content inspection happens locally on the device. User prompts are never stored unless explicitly configured by the organization&#x27;s security administrators.<p>We work closely with end-users of the product, and once they understand what is being monitored&#x2F;shared, they actually have great comfort that they have a powerful layer of protection on their device to prevent security incidents. It enables them to just focus on their work without worrying about what leaks and breaches may be happening under the hood without their awareness.<p>The Traceforce binary is built using Go and the browser extension is written in Node JS. The hardest part is building a complete connectivity graph between AI applications, MCPs, and tools, then identifying the vulnerabilities and attack paths introduced by those connections. Traditional security tools fall short: EDRs see processes, CASBs see network traffic, but neither has visibility into the application-level activity happening inside AI apps. The way we got it to work was by understanding the configurations and logs of each and every app. It’s a labor intensive process because every app is different and AI features change frequently.<p>Traceforce is currently deployed across more than 1,000 devices at 10 organizations. On average, we discover over 15 AI applications per device with each application connected to 5-10 MCPs. We&#x27;ve helped customers identify exposed plaintext secrets in MCP configurations, prevent API keys fro


*Generated by ai-researcher at 2026-07-17T02:05:27.501344+00:00*