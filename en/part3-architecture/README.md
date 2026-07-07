---
title: Part III · Technical Architecture
description: Building determinism into the foundation
---

# Part III · Technical Architecture

> The hardest thing about a payment chain isn't throughput — it's determinism, authorization, and recovery.

Part I and Part II argued **why** you need an L1 built for PayFi + AI-agent payments. This part answers **how to build it**.

We take AXON's technical design apart from the top down: from the foundational argument for "why a purpose-built L1 is unavoidable," to a panorama of the five-layer architecture, and then layer by layer through consensus, payment finality, price-feed safety, the compliance gateway, and account abstraction.

The chapter map for this part:

* **3.1** Why a Purpose-Built L1 — four reasons that cannot be sidestepped.
* **3.2** The Five-Layer Architecture — a payment's journey from the top down.
* **3.3** Consensus, Sub-Second Finality & Performance Targets — and how we treat performance numbers honestly.
* **3.4** Payment Finality & Double-Spend Prevention — the hardest thing a payment chain does.
* **3.5** Stablecoins & Price Feeds — multi-source validation and deviation circuit breakers.
* **3.6** Pluggable Compliance Gateway — building compliance into the foundation.
* **3.7** Account Abstraction, Session Keys & Paymaster — laying the primitives for AI-agent payments.
