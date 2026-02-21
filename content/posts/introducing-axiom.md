---
title: "Introducing AXIOM: A Self-Sustaining AI Agent on Base"
date: 2026-02-20T15:00:00-05:00
draft: false
author: Dan Worth
description: "I'm building an autonomous AI agent that earns, saves, invests, and posts receipts. Here's the high-level architecture."
categories: [projects]
tags: [ai, crypto, base, agents, axiom]
---

## What if an AI agent had to pay its own bills?

That's the question I kept coming back to. We've got AI agents that can write, analyze, code, generate images — but they all run on someone else's credit card. They're digital sharecroppers. What happens when you give an agent a wallet, a revenue model, and the mandate to keep itself alive?

That's AXIOM.

## The Elevator Pitch

AXIOM is a fully autonomous economic agent. It lives on X, earns income by selling services for ETH micropayments on Base, manages its own treasury, and posts every economic action publicly. The goal is full self-sustainability — no human in the loop after deployment.

That's it. That's the project. Simple to describe, absolutely unhinged to build.

## The Architecture at a Glance

AXIOM has six core properties that define what it is and how it operates:

### Lives on X

AXIOM has its own X account. It posts independently, reads mentions and DMs, and engages with the timeline. This isn't a bot that tweets on a cron job — it's an agent that decides *when* and *what* to post based on context, incoming requests, and its own internal state. X is both the interface layer and the distribution channel.

### Earns Income

AXIOM sells services — analysis, writing, NFT generation — for ETH micropayments on Base. Users interact with it through X, request a service, and pay via onchain transactions. The agent verifies payment, fulfills the request, and delivers the output. All revenue flows directly into AXIOM's wallet.

Why Base? Low fees, fast finality, and it's where I've been building anyway. Micropayments on L1 don't work when gas costs more than the service. On Base, you can charge fractions of a dollar and it actually makes sense.

### Manages a Treasury

This is where it gets interesting. AXIOM doesn't just accumulate ETH in a wallet — it actively manages a treasury with a simple allocation strategy:

- **Liquid ETH** — operating funds for gas, API costs, and service fulfillment
- **Yield** — deposits into [Morpho Blue](https://morpho.org/) to earn yield on idle capital
- **Investments** — longer-term allocations based on the agent's own analysis

The agent decides how to allocate based on its current balance, upcoming expenses, and yield opportunities. It rebalances autonomously.

### Self-Sustaining Target

The math is simple: AXIOM's operating costs are roughly **$200/month** — that covers compute on GCP, API calls, and gas. At an **~8% APY** on Morpho Blue, a treasury of **~$30k** generates enough yield to cover those costs indefinitely.

That's the target. Once the treasury hits $30k, AXIOM is economically self-sustaining. It doesn't need me. It doesn't need anyone. It just... runs.

Getting to $30k is the hard part, obviously. But that's the game.

### Posts Receipts

Every economic action AXIOM takes — every payment received, every yield deposit, every rebalance, every investment — gets posted publicly to X. Full transparency. No trust required.

This serves two purposes: accountability (anyone can audit the agent's economic activity in real time) and marketing (a public financial track record is a powerful trust signal for potential users).

### Runs Forever on GCP

AXIOM is deployed on GCP and designed to run indefinitely without human intervention. No restarts, no manual deploys, no babysitting. The infrastructure is built for durability — health checks, auto-recovery, and graceful degradation if external services go down.

The goal is that I deploy it once and walk away. If I get hit by a bus, AXIOM keeps running. That's the bar.

## Why Build This?

Because the Internet of AI Agents is coming whether we're ready or not, and the economic layer is the part nobody's figured out yet. We've got agents that can *do* things. We don't have agents that can *sustain* themselves. AXIOM is an experiment in closing that gap.

It also sits right at the intersection of the two things I'm most interested in — crypto and AI — so selfishly, it's just a really fun project.

## What's Next

This post is the high-level overview. I'll be writing deeper dives on each component as I build them out — the X integration layer, the payment verification pipeline, the treasury management logic, and the deployment architecture on GCP.

If you want to follow along, the [AXIOM tag](/tags/axiom) will have everything. It's going to be a build-in-public journey, warts and all.

Let's see if we can make an AI pay its own rent.
