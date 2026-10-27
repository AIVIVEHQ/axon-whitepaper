---
title: 6.1 Roadmap P0 → P3+
description: A four-stage advance from testnet to a global asset network
---

# 6.1 Roadmap P0 → P3+

AXON's roadmap divides into four stages, mapping to the three-phase evolutionary arc of [1.1](../part1-vision/1-1-thesis.md) — first get the foundation and settlement rail running, then grow into a PayFi network, and finally extend into a global asset network. **Each stage has independent exit value.**

## The four-stage panorama

```mermaid
timeline
    title AXON Finance Roadmap (stage-driven, no date commitments)
    P0 · Testnet : L1 core (high-throughput consensus / sub-second finality / low fees) : Stablecoin settlement primitives + account abstraction : Mock payments / settlement correctness · chaos / sequence replay : Target: eliminate "miscounting money / double-spends / exploitability"
    P1 · Mainnet Beta (MVP→PMF) : Stablecoin instant-settlement rail : Ecosystem 1 · US-equity copy-trading engine (first open ecosystem · already open) : Ecosystem 2 · PayFi 1.0 — Crypto U-Card payments launch : 1–2 PayFi scenarios (cross-border remittance / merchant acquiring) : AI agent payment SDK (session keys) + fee sponsorship : KYC plugin + first payment / market-making partners : Network & ecosystem launch
    P2 · Scale / PayFi Network : Ecosystem 2 · PayFi 2.0 — full-scenario ecosystem-mall commerce + AXON Wallet App (transfers / yield / prediction) : PayFi money market / float / on-chain credit at scale : U-Card at scale + WeChat / Alipay aggregation : x402 / M2M micro-payments at scale : More stablecoins and fiat rails : Validator expansion and decentralized governance takeover
    P3+ · Vision (super-ecosystem · TradFi expansion) : Ecosystem 3 · financial-social super-ecosystem : Ecosystem 4 · chat-social app (complete financial-social ecosystem) : Tokenization of US equities / commodities · TradFi on-chain trading : Grow the payment network into a global asset network
```

## Ecosystem Evolution: From Copy-Trading to a Financial-Social Super-Ecosystem

If P0→P3+ views AXON through **technical stages**, then viewed through **the order in which ecosystems open**, AXON is a clear four-tier progressive arc — each tier of ecosystem built on the real traffic, capital, and users accumulated by the one before it, each empowering the mother token AXON:

```mermaid
flowchart LR
    E1["Ecosystem 1 · US-equity copy-trading<br/>first open ecosystem · already open"] --> E2A["Ecosystem 2 · PayFi 1.0<br/>Crypto U-Card payments"]
    E2A --> E2B["Ecosystem 2 · PayFi 2.0<br/>full-scenario ecosystem mall + Wallet App"]
    E2B --> E3["Ecosystem 3 · financial-social super-ecosystem"]
    E3 --> E4["Ecosystem 4 · chat-social app<br/>complete financial-social ecosystem"]
    style E1 fill:#f5541c,stroke:#0a0908,color:#fff
    style E2A fill:#ff8a3d,stroke:#0a0908,color:#0a0908
    style E2B fill:#ff8a3d,stroke:#0a0908,color:#0a0908
    style E3 fill:#f5541c,stroke:#0a0908,color:#fff
    style E4 fill:#f5541c,stroke:#0a0908,color:#fff
```

* **Ecosystem 1 · US-equity copy-trading** — AXON's **first open ecosystem, and it is already open.** It uses deterministic stablecoin yield to convert prediction platforms' external traffic into the L1's real TVL and addresses, proving out "foundation-first, ecosystem-wedge" ([4.5](../part4-payfi/4-5-copy-trading-engine.md)).
* **Ecosystem 2 · PayFi payment ecosystem** — in two steps: **PayFi 1.0** is **Crypto U-Card payments** (turning on-chain stablecoins into spendable purchasing power, [4.6](../part4-payfi/4-6-u-card.md)); **PayFi 2.0** is **full-scenario ecosystem-mall commerce + the AXON Wallet App** (extending spending into a full-scenario mall and activating the balance into on-chain capital that can transfer, earn, and participate in markets, [4.7](../part4-payfi/4-7-wallet-app.md)).
* **Ecosystem 3 · financial-social super-ecosystem** — fusing payment, yield, prediction, and social into one settlement layer, so every interaction can carry value ([4.8](../part4-payfi/4-8-financial-social.md)).
* **Ecosystem 4 · chat-social app** — the most long-range tier: a standalone chat-social product that fully merges finance and social, building a user's **complete financial-social ecosystem** and closing the entire arc.

This ecosystem arc and the P0→P3+ stages below are not two timetables, but two views of the same process: Ecosystems 1 and 2 (1.0) land in P1, Ecosystem 2 (2.0) in P2, and Ecosystems 3 and 4 in P3+.

## Stage-by-stage reading

### P0 · Testnet — polishing determinism to the extreme

The goal of the first stage is not flashy features, but **the determinism of the foundation**. It delivers the L1 core (high-throughput consensus, sub-second finality, low fees), stablecoin settlement primitives, and account abstraction, and through mock-payment / settlement-correctness tests, chaos engineering, and sequence-replay drills, it hammers on one thing over and over:

> **Eliminate "miscounting money / double-spends / exploitability."**

This is of a piece with [3.4 Payment Finality](../part3-architecture/3-4-payment-finality.md) — if the foundation is unstable, everything after it is a castle in the air. **Exit value**: a payment L1 whose determinism has been validated.

### P1 · Mainnet Beta — from MVP to PMF

The second stage cashes the foundation into a usable product, with the goal of moving from minimum viable (MVP) to product-market fit (PMF):

* **Stablecoin instant-settlement rail** goes live ([4.1](../part4-payfi/4-1-settlement-rail.md));
* **Ecosystem 1 · US-equity copy-trading engine** — AXON's **first open ecosystem (already open)**, empowering the ecosystem of prediction platforms such as Polymarket and Kalshi, using deterministic stablecoin yield to convert external traffic into real TVL and addresses ([4.5](../part4-payfi/4-5-copy-trading-engine.md));
* **Ecosystem 2 · PayFi 1.0: Crypto U-Card payments** launch — turning on-chain stablecoins into spendable purchasing power, the first step of the PayFi payment ecosystem ([4.6](../part4-payfi/4-6-u-card.md));
* **1–2 PayFi scenarios** land (cross-border remittance / merchant acquiring, [4.3](../part4-payfi/4-3-crossborder-b2b.md));
* **AI agent payment SDK** (session keys) + fee sponsorship ([5.2](../part5-ai/5-2-controlled-execution.md), [3.7](../part3-architecture/3-7-account-abstraction.md));
* **KYC plugin** + first payment / market-making partners;
* **Network & ecosystem launch** ([Season 1 · Ecosystem Open Program](6-4-ecosystem-season.md)).

**Exit value**: a PayFi mainnet with real users and real payment flow.

> **Product priority**: the copy-trading engine is AXON's **first open ecosystem (a GTM wedge)** in the PayFi direction — rather than rolling out every scenario at once, it first proves out the "foundation-first, product-wedge" strategy through a single incision whose traffic, yield, and closed loop can all be truly validated. Logically it lands in P1 (the first open-ecosystem launch) and scales up in P2 as the PayFi network grows.

### P2 · Scale / PayFi Network — opening the engine fully

The third stage expands PayFi from "a few scenarios" into "a network":

* **PayFi money market / float yield / on-chain credit** at scale ([4.2](../part4-payfi/4-2-money-market.md));
* **Ecosystem 2 · PayFi 2.0: full-scenario ecosystem-mall commerce + AXON Wallet App** — U-Card at scale with WeChat / Alipay aggregation, full-scenario ecosystem-mall commerce goes live, and the Wallet App (transfers / yield / prediction markets) ships, extending PayFi from "a single payment card" into "a full payment ecosystem" ([4.6](../part4-payfi/4-6-u-card.md), [4.7](../part4-payfi/4-7-wallet-app.md));
* **x402 / M2M micro-payments** at scale ([5.3](../part5-ai/5-3-x402-m2m.md));
* Onboard **more stablecoins and fiat rails**;
* **Validator expansion, decentralized governance takeover** ([6.2](6-2-governance.md)).

**Exit value**: a self-reinforcing PayFi network, with governance beginning to decentralize.

### P3+ · Vision — from payment network to asset network

Last comes the long-range vision, with two parallel lines of extension. The first is the two highest tiers of the consumer ecosystem: first **Ecosystem 3 · the financial-social super-ecosystem** ([4.8](../part4-payfi/4-8-financial-social.md)) — fusing payment, yield, prediction, and social into one settlement layer so every interaction carries value; then further to **Ecosystem 4 · the chat-social app** — a standalone chat-social product that fully merges finance and social, building a user's **complete financial-social ecosystem** and closing the entire arc. The second is that, on top of a payment / clearing rail validated by real business, the network extends into the broader world of assets — **tokenization of US equities / commodities, TradFi on-chain trading**. Reuse the validated funds and clearing rail to grow the payment network into a **global asset network**.

## Why stages, not dates

The attentive reader will notice that this roadmap has **only stages, no specific calendar dates**. This is a deliberate choice:

* An L1 built from the foundation up has progress that depends on many variables — determinism validation, compliance progress, ecosystem maturity — and **date commitments precise to the quarter are often unrealistic, even misleading**;
* We prefer to measure progress by "the exit value of each stage" — **what was accomplished, not what day it was reached**;
* The stage-based division gives the team room to advance honestly, and gives readers a clear yardstick for judging progress.

**L1 + PayFi first, TradFi as the long-range vision** — this ordering says more about AXON's priorities than any date could.

---

*Further reading: [6.2 Governance Framework](6-2-governance.md) · [1.1 Core Thesis](../part1-vision/1-1-thesis.md)*
