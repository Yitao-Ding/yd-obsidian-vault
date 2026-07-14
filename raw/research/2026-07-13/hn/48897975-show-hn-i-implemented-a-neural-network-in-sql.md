---
type: research
source: hn
source_id: '48897975'
url: https://github.com/xqlsystems/xarray-sql/blob/claude/xarray-sql-mnist-demo/benchmarks/nn.py
published_at: '2026-07-13T20:00:10+00:00'
collected_at: '2026-07-13T22:04:40.219715+00:00'
score: 10.0
importance: 4
categories:
- code-llm
- infra
- tooling
- theory
related_projects:
- lecture_hub
implementation_difficulty: high
authors: []
title: 'Show HN: I implemented a neural network in SQL'
---

# Show HN: I implemented a neural network in SQL

> https://github.com/xqlsystems/xarray-sql/blob/claude/xarray-sql-mnist-demo/benchmarks/nn.py

## 要約 (日本語5行)

Xarray-SQL ライブラリで、著者が SQL を用いてニューラルネットワークを実装しました。
行列乗算を JOIN + GROUP BY として、自動微分を DataFusion の visitor pattern で実装。
物理計算や地理空間クエリをデータベース内で実行可能にするアプローチ。
配列データを 2D テーブルで表現する統一的なモデルが、線形代数と微積分を SQL で実現。
従来の array-based compute ではなく、リレーショナルモデルで numerical computing を再発明。

## 既存プロジェクトとの関連
- **lecture_hub** — 個人ナレッジハブ (Next.js + Supabase Postgres + pgvector + BlockNote + AI SDK)

## メタ
- 重要度: 4/5
- スコア: 10.00/10
- 実装難易度: high
- カテゴリ: code-llm, infra, tooling, theory

## 原文 (Abstract)

Two weeks ago I was on my babymoon in Corfu, Greece. While in transit, I was overseeing a GSoC intern submit an important feature to my array database library, Xarray-SQL. He added `to_dataset()`, which completed the roundtrip between thinking of array data in a tabular model simultaneously as gridded rasters (the premise of the project is that every Nd array can be mapped to 2d, where orthogonal dims of the Nd array are just primary keys of a tabular representation). We discussed in chat, now that this feature existed, what demos could we make that would prove this data model works?<p>With down time on a warm beach during a heatwave, cool salty water giving me fresh ideas, I had an idea: what if we used Coiled&#x27;s Geospatial benchmark discussion as a comprehensive overview of geo and climate queries. Are all of these common operations secretly relational, just with the wrong data model? Using Claude Code on the beach, I can confirm that this seemed to be the case: Claude and I publish a benchmark that illustrated how every common operation in geo and climate sciences (at the 100 TB range) were actually secretly relational operations: <a href="https:&#x2F;&#x2F;github.com&#x2F;xqlsystems&#x2F;xarray-sql&#x2F;blob&#x2F;main&#x2F;docs&#x2F;geospatial.md" rel="nofollow">https:&#x2F;&#x2F;github.com&#x2F;xqlsystems&#x2F;xarray-sql&#x2F;blob&#x2F;main&#x2F;docs&#x2F;geos...</a>.<p>Most surprisingly of all, from these examples was that a core operation, regridding, was just a sparse matrix-vector product. Claude had pointed out to me that in this data model, matmul was just a `SUM(val * val) ... JOIN .. GROUP BY`. This has a direct parallel to einsum notation, but can be expressed in (arguably) elegant SQL syntax! This capability seemed to be greater than the sum of it&#x27;s parts.<p>Back in the cool water of the Ionian, I thought about the implications of this more deeply. I reflected that, all of the Coiled benchmarks did, deep down, was _post process_ simulations that happen in numerical&#x2F;array code. Why couldn&#x27;t these physics calculations be push down into the database also, if we could so matmul in SQL? Then it hit me: maybe they could, if in addition to linear algebra, if SQL could do calculus! <a href="https:&#x2F;&#x2F;bsky.app&#x2F;profile&#x2F;al.merose.com&#x2F;post&#x2F;3mpbods7wts2y" rel="nofollow">https:&#x2F;&#x2F;bsky.app&#x2F;profile&#x2F;al.merose.com&#x2F;post&#x2F;3mpbods7wts2y</a><p>Later on, I implemented autograd on top of DataFusion&#x27;s visitor pattern based on JAX&#x27;s implementation. In my simplified array model, it turns out that we only care about partial differentiation on the diagonal of the Jacobian, meaning that `grad()`, `jvp` and `vjp` are just row-wise operations! I then implemented a common physics calculation from the coiled benchmark that required gradients. From here, I realized if I can autograd in the database, why can&#x27;t I create a neural network?<p>As I came back home, I created some slides, and presented this work to DataFusion&#x27;s inaugural showcase: <a href="https:&#x2F;&#x2F;www.youtube.com&#x2F;watch?t=1511&amp;v=5o-4hL8vGPw&amp;feature=youtu.be" rel="nofollow">https:&#x2F;&#x2F;www.youtube.com&#x2F;watch?t=1511&amp;v=5o-4hL8vGPw&amp;feature=y...</a> I realized in this synthesis that SQL is not necessarily a toy language for writing neural networks, but in fact, may be highly desirable in the future due to the fundamental principles of relational databases: the logical layer should be independent from the physical layer. If that property holds, and a neural network is a series of relations, could we create a SOTA distributed system for training more easily? For example, if we had one global logical plan of dataflow, could we better distribute work on 1000+ GPUs?<p>Several scientists and engineers and I are working together to explore this weird world of relational arrays at <a href="https:&#x2F;&#x2F;xql.systems" rel="nofollow">https:&#x2F;&#x2F;xql.systems</a>


*Generated by ai-researcher at 2026-07-13T22:04:40.220630+00:00*