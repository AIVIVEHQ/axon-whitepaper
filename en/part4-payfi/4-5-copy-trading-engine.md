---
title: "4.5 The US-Equity Copy-Trading Engine: First Open Ecosystem"
description: Using deterministic stablecoin yield to convert external traffic into the L1's real TVL and addresses
---

# 4.5 The US-Equity Copy-Trading Engine: First Open Ecosystem

The previous four sections laid out PayFi's foundation scenarios (the settlement rail, the money market, cross-border acquiring, time value). This section covers **the first open ecosystem that turns these capabilities into real business** — the **AXON US-Equity Copy-Trading Engine.**

Along the "float + on-chain credit" direction described in [4.2 PayFi Money Market](4-2-money-market.md), the copy-trading engine is AXON Finance's **first open ecosystem** — AXON provides the L1 settlement and account-abstraction infrastructure, while prediction platforms and a Silicon-Valley quant desk participate as ecosystem members (AXON is infrastructure, not the product operator). It does not try to blanket every scenario from the outset; instead it picks one wedge where **traffic, yield, and the closed loop can all be really validated** and gets it working first: its core mission is to drive explosive growth in the L1's TVL and real address count by providing deterministic stablecoin yield to external traffic.

## Strategic Enablement: The Traffic-Swap Model

The copy-trading engine is designed for the users of prediction platforms **such as Polymarket, Kalshi, and Kairos.** It solves a long-overlooked structural problem on these platforms:

* **Pain point** — prediction-market events often have long cycles (an election, an earnings report, a macro decision), during which user funds sit largely idle while awaiting the outcome, with no cross-domain financial tool to put that idle capital to work.
* **Solution** — the prediction platform serves as the entry point providing traffic, and AXON provides the underlying trading and settlement infrastructure. Users take the stablecoins already on the prediction platform and, through AXON L1, **one-click copy-trade** US-equity strategies, letting idle capital keep earning yield during the waiting period.
* **Value loop** — AXON gains massive real traffic and TVL, and the prediction platform gains a high-stickiness yield-bearing feature. **"Prediction platforms drive traffic; AXON stablecoins earn yield"** — each side gets what it needs.

In external positioning, this layer of cooperation is presented as **"open ecosystem partners"**: prediction platforms are partners in the AXON ecosystem and among the projects onboarding into the [Season 1 ecosystem launch](../part6-roadmap/6-4-ecosystem-season.md) — not some deep, exclusive binding. The ecosystem is open; the copy-trading engine simply brings in the first wave of traffic from that open ecosystem.

## Core Mechanism: Principal-Protected Floor Return (1%–3%)

The system promises users a **principal-protected floor return** — "principal protection + a deterministic single-round return" — with a single-round return range of **1%–3%.** This promise is not a robbing-Peter-to-pay-Paul money game; it is supported by **four real-PnL pillars** together, designed from the ground up to firmly rule out any Ponzi model:

1. **US-equity quant edge** — high-win-rate US-equity quant strategies (options arbitrage, IV-surface dislocation) executed by AXON's core team of former Silicon Valley big-tech engineers (see [6.3 Team & Resource Network](../part6-roadmap/6-3-team-partners.md)), targeting mainly high-frequency events **with deterministic catalysts** such as NVIDIA earnings and Federal Reserve decisions.
2. **Options hedging** — through strategies like straddles and iron condors, loss risk is **hard-locked within a known range**, refusing naked longs and naked shorts. The determinism of the return comes from the determinism of the risk boundary.
3. **Real revenue injected** — network gas fees and PayFi settlement fees on the AXON L1 chain are injected into the reserve pool at a fixed proportion. The more active the network, the deeper the backstop.
4. **Acquisition-budget subsidy** — the customer-acquisition budget that traditional public chains spend on airdrops / marketing is converted directly into a "floor-return subsidy" for real trading users. The cost of user growth flows back to the users themselves.

## Revenue Distribution: A Cut Injected into the Reserve Pool

The copy-trading engine takes a proportional cut of user returns, and this capital is **entirely injected into the AXON reserve pool**, used to support the chain's liquidity and token value. It plugs directly into the value-capture mechanism of [7.3 Token Utility & the Deflationary Flywheel](../part7-tokenomics/7-3-utility-flywheel.md):

* One portion enters the **buyback-and-burn flywheel** together with protocol fees — real business volume → protocol revenue → buy back AXON → burn, supporting liquidity and token price with real cash flow rather than inflationary subsidy;
* One portion replenishes the **on-chain copy-trading reserve pool**, providing the ultimate backstop for the "principal-protected floor return."

In other words, every real trade the copy-trading engine completes both gives the user a deterministic return and settles a portion of the value back into the L1 itself.

## System Architecture and Safety Cushion

* User funds are fully governed by AXON L1 **smart contracts**, working with the account abstraction (AA) and Paymaster modules (see [3.7 Account Abstraction, Session Keys & Paymaster](../part3-architecture/3-7-account-abstraction.md)) to deliver a **frictionless, zero-gas** interaction experience.
* An **on-chain copy-trading reserve pool** is established for the ultimate backstop. The system monitors the reserve coverage ratio in real time (e.g., a threshold of **≥ 150%**), and when coverage falls short it **smart-circuit-breaks** — automatically throttling the round-issuance quota to ensure the absolute safety of in-transit funds.

The copy-trading engine is the first realization of AXON's "foundation-first, product-wedge" strategy: it twists Part III's deterministic foundation, Part IV's PayFi yield capabilities, and Part V's AI-controllable execution into a single product the market can directly perceive, and provides the [roadmap](../part6-roadmap/6-1-roadmap.md)'s P1 stage with its first landing anchor of "real users, real capital, real yield."

---

*Further reading: [4.2 PayFi Money Market](4-2-money-market.md) · [6.4 Ecosystem Launch & Season 1](../part6-roadmap/6-4-ecosystem-season.md) · [Part VII · Tokenomics](../part7-tokenomics/README.md)*
