---
title: Appendix II · Protocol Parameters
description: Protocol parameters and status annotations
---

# Appendix II · Protocol Parameters

The key protocol parameters used throughout are consolidated here, each annotated with its degree of certainty:

* `Design Target` — the target engineering aims to achieve; final values await benchmarking.
* `design` — a determined design direction; concrete values undetermined.
* `TBD` — not yet determined; announced as development / compliance / governance progresses.

> All parameters are proposed design; final values are subject to testnet benchmarks, security review, and governance conclusions; they constitute no commitment.

## Performance

| Parameter | Value | Status |
| --- | --- | --- |
| Throughput $\Theta$ | $> 10{,}000$ TPS (architecturally can push higher) | `Design Target` |
| Finality latency $T_{\text{final}}$ | sub-second ($<1$s, target 400–800ms) | `Design Target` |
| Block interval $\Delta_{\text{block}}$ | 250–500 ms | `Design Target` |
| Per-transaction fee | sub-cent ($<\$0.001$) | `Design Target` |
| Pipeline depth $k$ | 2–3 (consecutive-QC commit) | `design` |

## Consensus & Validators

| Parameter | Value | Status |
| --- | --- | --- |
| Consensus | HotStuff-style pipelined BFT PoS | `design` |
| BFT threshold | $S_f < \tfrac13 S$; quorum $>\tfrac23 S$ | `design` |
| Minimum stake $s_{\min}$ | — | `TBD` |
| Validator cap $n_{\max}$ | small at launch, expanded per P2 | `design` / roadmap |
| Epoch length | — | `TBD` |
| Unbonding period $T_{\text{unbond}}$ | $>$ slashing evidence window | `design` |
| View timeout $\Delta_{\text{view}}$ | adaptive exponential backoff | `design` |

## Cryptography

| Parameter | Value | Status |
| --- | --- | --- |
| Hash | BLAKE3 (Keccak-256 compatible) | `design` |
| Transaction signature | Ed25519 (account-customizable verifier) | `design` |
| Consensus signature | BLS12-381 aggregation | `design` |
| Randomness | VRF | `design` |
| State commitment | Merkle (Verkle-ready) | `design` |

## Authorization & Execution

| Parameter | Value | Status |
| --- | --- | --- |
| Account model | account-based, chain-native AA | `design` |
| Policy sandbox | WASM, gas-metered, deterministic, capability-safe | `design` |
| Session-key constraints | total limit / per-tx / time / allowlist / asset / frequency / call | `design` |
| Gas fee model | steady-state fixed + congestion protection valve | `design` |

## Price Feed (Oracle)

| Parameter | Value | Status |
| --- | --- | --- |
| Aggregation | median | `design` |
| Deviation-detection threshold $\tau_{\text{dev}}$ | — | `TBD` |
| Minimum live-source count $m_{\min}$ | — | `TBD` |
| TWAP window $\Delta$ | — | `TBD` |
| De-peg threshold $\tau_{\text{peg}}$ | — | `TBD` |
| Price-feed sources / oracle partnerships | — | `TBD` (whitepaper [6.3](../part6-roadmap/6-3-team-partners.md)) |

## PayFi Money Market

| Parameter | Value | Status |
| --- | --- | --- |
| Interest-rate curve $r_0,r_1,r_2,U^{*}$ | governance-set | `TBD` |
| Reserve factor $\phi$ | governance-set | `TBD` |
| Collateral liquidation threshold $\lambda_i$ | governance-set | `TBD` |
| Position-opening health factor $H_{\text{open}}$ | $>1$ | `design` |
| Close factor / liquidation bonus | governance-set | `TBD` |

## US-Equity Copy-Trading Engine

| Parameter | Value | Status |
| --- | --- | --- |
| Guaranteed floor $g$ | $1\%\sim3\%$ / round | `finalized product parameter` |
| Coverage floor $\Xi_{\min}$ | $\geq 150\%$ | `finalized product parameter` |
| Per-round limit $L_{\text{tx}}$ | governance-set / per-round | `design` |
| Settlement window $t_{\text{settle}}-t_{\text{open}}$ | $2\sim24$ h | `finalized product parameter` |
| Reserve injection rate $\phi_{ct}$ | governance-set | `TBD` |
| Post-hedge max loss ratio $\chi$ | governance-set (determined by the options portfolio) | `TBD` |
| Settlement challenge window $\Delta_{\text{chal}}$ | governance-set | `TBD` |
| Per-round copy-trade cap $\text{Cap}$ | governance-set / per-round | `TBD` |

## Compliance & Stablecoins

| Parameter | Value | Status |
| --- | --- | --- |
| Supported stablecoin list | multi-asset, fiat-pegged first | `TBD` |
| Compliance jurisdiction / licensing path | crypto-friendly jurisdictions, phased | `TBD` |
| Compliance hook composition | KYC/AML/sanctions/geofencing/Travel Rule | `design` |

## Out of Scope for This Document

| Item | Note |
| --- | --- |
| Token supply / allocation / circulation / financing | Belongs to **token economics**; not expanded on in this yellowpaper, see whitepaper [Part VII · Tokenomics](../part7-tokenomics/README.md) |
| Distribution of fees/rewards between validators and the treasury | Same as above; belongs to token economics, see whitepaper [Part VII](../part7-tokenomics/README.md) |

---

*Next: [Appendix III · Core Data Structures](appendix-datastructures.md)*
