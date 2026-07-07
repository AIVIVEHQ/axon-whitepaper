---
title: Appendix B · Parameter Summary
description: A consolidated list of performance design targets and TBD parameters
---

# Appendix B · Parameter Summary

This appendix consolidates the key parameters that appear throughout the book, and **honestly labels the status of each**. Statuses fall into three classes:

* `Design Target` — a design and engineering goal to be met; the final value will be published after benchmarking;
* `design` — a confirmed design direction, but with the specific parameter undetermined;
* `TBD` — not yet determined; to be published as development / compliance progresses.

## Performance Design Targets

| Parameter | Design Target / Value | Status |
| --- | --- | --- |
| Network throughput (TPS) | Payment-grade sustained throughput **> 10,000 TPS**, architected to scale to higher orders of magnitude | `Design Target` |
| Deterministic finality | **Sub-second (< 1s, target range ~400–800ms)** | `Design Target` |
| Block production / block interval | **~250–500ms** | `Design Target` |
| Transaction fee | **Sub-cent (target < $0.001 / tx)**, fixed and predictable | `Design Target` |
| Fee model | Low and predictable (not auction-style volatility) | `design` |

## Consensus & Networking

| Parameter | Value | Status |
| --- | --- | --- |
| Consensus mechanism | High-throughput PoS + BFT-style deterministic finality | `design` |
| Finality type | Deterministic / BFT (irreversible once confirmed by a quorum) | `design` |
| Validator set size | Small at launch, expanding progressively along the P2 roadmap | `design` / roadmap |
| Decentralization path | Early centralized but verifiable → P2 validator expansion → governance takeover at maturity | roadmap |

## Foundation Capabilities

| Capability | Status |
| --- | --- |
| Stablecoin settlement primitives (built into the chain layer) | `design` |
| Account abstraction / session keys (first-class citizens) | `design` |
| Paymaster fee sponsorship | `design` |
| Pluggable compliance gateway (KYC/AML / geofencing / risk control) | `design` |
| Verifiable payment-policy sandbox (WASM) | `design` |
| Global monotonic seqNo + write-ahead log | `design` |
| Multi-source price feeds + deviation circuit breaker + depeg guard | `design` |

## TBD / To Be Announced Separately

| Item | Status | Note |
| --- | --- | --- |
| List of supported stablecoins | `TBD` | Determined as mainnet progresses ([3.5](../part3-architecture/3-5-oracle-safety.md)) |
| PayFi money-market rates / yields | `TBD` | Design model; parameters TBD ([4.2](../part4-payfi/4-2-money-market.md)) |
| Credit risk-control / reserve / slashing parameters | `TBD` | Design framework; parameters TBD |
| Compliance registration jurisdiction / licensing path | `TBD` | Crypto-friendly jurisdictions, phased rollout ([3.6](../part3-architecture/3-6-compliance-gateway.md)) |
| Fiat on/off-ramp partners | `in design` | Integrated as mainnet progresses ([4.3](../part4-payfi/4-3-crossborder-b2b.md)) |
| Governance economic parameters (staking / voting weight, etc.) | `design` | Part of token economics; see [Part VII · Tokenomics](../part7-tokenomics/README.md) |
| Token economics (supply / allocation / circulation / financing) | — | See [Part VII · Tokenomics](../part7-tokenomics/README.md) (10B total / six-bucket allocation / vesting / circulation curve / deflationary flywheel) |

## Market Data (Industry Context)

| Metric | Value | Basis |
| --- | --- | --- |
| Stablecoin on-chain settlement, 2025 | $33T (exceeds Visa+MC $25.5T) | Public monitoring; different-layer bases, read the trend |
| Huma cumulative volume | $10B+ (Feb 2026, YoY 3.4×) | PayFi leader, public data |
| Stablecoin share of cross-border payments, 2030E | ~10% | Industry forecast |
| Global cross-border payment annual flow (TAM) | $190T+ | Public monitoring; B2B ~60% of stablecoin cross-border |

*(Market data uses public-monitoring bases; sources in [Appendix D](d-references.md).)*
