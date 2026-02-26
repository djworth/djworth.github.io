---
title: "Data Infrastructure Is the Secret Weapon in the Agentic Era"
date: 2026-02-26T09:00:00-05:00
draft: false
author: Dan Worth
description: "Everyone's talking about AI agents. Nobody's talking about the data layer underneath them. GCP and BigQuery are quietly winning this game."
categories: [ai]
tags: [ai, agents, gcp, bigquery, data, architecture]
---

## The Take

Here's the thing nobody wants to talk about at AI conferences: **your agents are only as good as the data infrastructure underneath them.**

Everyone's building agents. Everyone's wrapping LLMs in tool-calling loops and giving them fancy names. Cool. But when those agents need to actually *do* something useful — retrieve context, make decisions, take actions on real business data — the bottleneck isn't the model. It's the plumbing. It's always the plumbing.

I've spent my career building and operating data systems at scale, and I'm here to tell you: the companies that are going to win in the agentic era aren't the ones with the best prompt engineering. They're the ones with the best data architecture. And right now, Google Cloud Platform and BigQuery are the clearest path to getting there.

## Why Data Infrastructure Matters for Agents

Let's think about what an AI agent actually needs to be useful in an enterprise context:

**Access to structured and unstructured data in real time.** An agent analyzing customer behavior can't wait for a nightly ETL job to finish. It needs current data, queryable now, across multiple sources.

**A semantic layer that makes data discoverable.** Agents don't read your team's tribal knowledge about which table has the canonical customer ID. They need well-organized, well-documented data assets they can reason about programmatically.

**The ability to execute analytical queries as part of their reasoning loop.** This is the big one. An agent that can write and execute SQL against your data warehouse as a tool call is categorically more useful than one that can only work with what's been pre-fetched into its context window.

**Governance and access control that works at machine speed.** When you have fifty agents querying your data platform on behalf of different teams, IAM isn't optional — it's critical infrastructure.

If your data platform can't deliver on these four requirements, it doesn't matter how sophisticated your agent framework is. You're building a race car and parking it on a dirt road.

## Why GCP and BigQuery

I'm not going to spend this post tearing down other clouds. They're fine. They work. But GCP has a combination of properties that make it uniquely well-suited for agentic workloads, and BigQuery is the centerpiece of that story.

### BigQuery Is a Serverless SQL Engine That Scales to Absurd Levels

This matters for agents because agent workloads are inherently bursty and unpredictable. You don't know when an agent is going to decide it needs to scan 10TB of event data to answer a question. With BigQuery, you don't have to care. There's no cluster to size, no capacity to provision, no "oh no we hit our concurrency limit" moment. You submit a query, you get results. The infrastructure scales underneath you.

For agentic workflows, this is transformative. You can give an agent the tool `execute_bigquery_sql` and let it run whatever analytical queries it needs as part of its reasoning — without worrying about whether your warehouse can handle the load pattern. It always can.

### Vertex AI and BigQuery Live in the Same Ecosystem

This is underappreciated. On GCP, your AI layer (Vertex AI) and your data layer (BigQuery) aren't separate products bolted together — they're deeply integrated. You can run ML models directly in BigQuery with BQML. You can access BigQuery data from Vertex AI pipelines without copying it anywhere. You can use Vertex AI's agent frameworks with BigQuery as a native tool.

When your agent runtime and your data warehouse are in the same ecosystem with shared IAM, shared networking, and shared metadata, the integration friction drops to near zero. Your agents can go from "I need to query sales data" to "here are the results" without bouncing through three different authentication layers and a data copy.

### Gemini Models Are Native to the Platform

Google's Gemini models are first-class citizens on GCP. They understand BigQuery schemas. They can generate SQL that's optimized for BigQuery's dialect. They can reason about data types, partitioning, and clustering in ways that matter for query performance.

When your LLM, your agent framework, and your data warehouse all speak the same language, you eliminate an entire class of integration problems. The agent doesn't need a translation layer between "what it wants to know" and "how to ask BigQuery." It just knows.

### Data Governance Is Built In

BigQuery has column-level security, row-level security, data masking, and fine-grained IAM — all built in, all manageable through policy. When you're running agentic workloads at scale, you need to be able to say "Agent A can see customer names but not SSNs" and have that enforced at the data layer, not in the agent's prompt.

This is the kind of thing that doesn't show up in demos but makes or breaks production deployments. Governance at the platform level means you don't have to trust your agents to be well-behaved — you can *enforce* it.

### Looker and the Semantic Layer

Looker's modeling layer (LookML) gives you a governed semantic layer on top of BigQuery that agents can consume programmatically. Instead of an agent guessing which table contains revenue data and how to join it to the customer dimension, it can query a semantic model that encapsulates that logic.

This is how you go from "agents that can run SQL" to "agents that understand your business." The semantic layer is the bridge between raw data and business meaning, and it's exactly what agents need to make decisions that actually make sense.

## The Architecture Pattern

Here's how this comes together in practice for agentic workloads:

```
Agent (Vertex AI / Custom)
    │
    ├── Tool: BigQuery SQL Execution
    │       └── Governed access via IAM + column/row security
    │
    ├── Tool: Looker Semantic Query
    │       └── Business logic abstracted via LookML
    │
    ├── Tool: Cloud Storage (unstructured data)
    │       └── Documents, images, embeddings
    │
    └── Tool: Pub/Sub (event streams)
            └── Real-time signals and triggers
```

The agent sits at the top and orchestrates across these tools based on the task. BigQuery handles the heavy analytical lifting. Looker provides the semantic layer. Cloud Storage handles unstructured data. Pub/Sub provides real-time event streams. Everything is governed by a single IAM model and lives in the same network.

This is not a theoretical architecture. This is what production agentic systems look like when they're built on a data platform that was designed for this kind of workload.

## The Punchline

The agentic era is going to be won by the companies that treat data infrastructure as a first-class concern — not an afterthought. The agent is the interface. The data platform is the engine.

If you're investing in AI agents but underinvesting in the data architecture underneath them, you're building on sand. And if you're looking for a platform where the AI layer, the data layer, and the governance layer all work together natively — GCP and BigQuery are the answer.

The secret weapon isn't the agent. It's the data.
