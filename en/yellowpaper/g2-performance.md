---
title: G.2 Performance Model & Design Targets
description: Throughput/latency analysis and target derivation
---

# G.2 Performance Model & Design Targets

> **Design status**: performance metrics are `Design Targets`, **not implemented measured values**; final values will be set by testnet benchmarks. This section gives the derivation logic behind the targets, not a promise.

## G.2.1 Three Performance Dimensions

A payment-grade SLA (whitepaper [3.3](../part3-architecture/3-3-consensus-finality.md)) is characterized by three quantities:

$$\text{throughput } \Theta\ (\mathrm{TPS}), \qquad \text{finality latency } T_{\text{final}}\ (\mathrm{s}), \qquad \text{per-transaction fee } \text{fee}\ (\$)$$

Design Targets: $\Theta > 10{,}000$, $T_{\text{final}} < 1\mathrm{s}$, $\text{fee} < \$0.001$. Below we give the derivation and bottleneck analysis for each.

## G.2.2 Finality Latency

From [B.1.7](b1-consensus.md), finality latency is the number of consecutive QCs required to commit, $k$, times the per-round overhead:

$$T_{\text{final}} \approx k \cdot (\delta_{\text{net}} + \delta_{\text{agg}} + \delta_{\text{exec}})$$

With pipeline depth $k = 3$, a block-interval target $\Delta_{\text{block}} \in [250, 500]\,\mathrm{ms}$, and with $\delta_{\text{agg}}$ (BLS aggregate verification) and $\delta_{\text{exec}}$ (execution) absorbed by the pipeline within the block interval, then

$$T_{\text{final}} \approx k \cdot \Delta_{\text{block}} \in [0.75, 1.5]\,\mathrm{s} \ \Rightarrow\ \text{sub-second target}$$

Optimization directions: reduce $k$ (a more aggressive commit rule), shorten $\Delta_{\text{block}}$ (faster aggregation / a closer validator topology). BLS aggregation makes QC verification independent of $n$ ([A.2.3](a2-cryptography.md)), the key to latency not degrading as validators grow.

## G.2.3 Throughput

Throughput per unit time = transactions per block / block interval:

$$\Theta = \frac{N_{\text{block}}}{\Delta_{\text{block}}}, \qquad N_{\text{block}} = \min\!\left(\frac{B_{\text{net}}}{\bar{s}_{\text{tx}}},\ \frac{C_{\text{exec}}\cdot\Delta_{\text{block}}}{\bar{g}_{\text{tx}}}\right)$$

Transactions per block $N_{\text{block}}$ is constrained by two upper bounds (take the smaller):

* **Network bound**: bandwidth $B_{\text{net}}$ / average transaction size $\bar{s}_{\text{tx}}$ — relaxed by the compact blocks / erasure coding of [G.1.3](g1-networking.md).
* **Execution bound**: execution throughput $C_{\text{exec}}$ (gas/s) × block interval / average transaction gas $\bar{g}_{\text{tx}}$.

To reach $\Theta > 10{,}000\,\mathrm{TPS}$, both bounds must be ample. Three levers:

1. **Parallel execution**: payment transactions are mostly disjoint account pairs (no state conflict), so they can be optimistically executed in parallel + conflict-detection retry — raising $C_{\text{exec}}$ to nearly the core-count multiple.
2. **Short block interval**: the smaller $\Delta_{\text{block}}$, the more throughput and latency improve together (bounded by the network lower bound).
3. **Lean execution**: stablecoin settlement is a chain-layer primitive ([D.1](d1-settlement.md)), so the payment path is short and $\bar{g}_{\text{tx}}$ is small, fitting more transactions under the same bandwidth/compute.

## G.2.4 Fee

From [F.1.2](f1-gas-fees.md), a payment transaction fee $= \bar{g}_{\text{tx}} \times price_{\text{fixed}}$. Because the settlement primitive makes $\bar{g}_{\text{tx}}$ low and stable, and steady state uses a fixed rate (no auction premium), the fee can be compressed to the **sub-cent range** and made **predictable** — the premise for M2M micropayments ([C.2.7](c2-session-keys.md)) to be economically viable. **Predictability itself is the feature**, not merely a low number.

## G.2.5 Bottleneck & Trade-off Summary

| Dimension | Main bottleneck | Main levers |
| --- | --- | --- |
| Finality latency | $k \cdot \Delta_{\text{block}}$ | BLS aggregation, validator topology, commit rule |
| Throughput | min(network bound, execution bound) | parallel execution, compact blocks, lean payment path |
| Fee | $\bar{g}_{\text{tx}}$, fee-rate mechanism | primitized settlement, fixed rate |

## G.2.6 Honest Disclosure

> The figures in this section are all **`Design Targets`**, representing the target range engineering aims to achieve and its derivation logic, **not currently implemented measured values**. Actual performance is affected by many factors — hardware, network topology, load characteristics, parallelization effectiveness — and final measured values will be published after **testnet benchmarking**. We do not state Design Targets as if they were already-achieved results. See the parameter summary in [Appendix II](appendix-parameters.md).

---

*Next: [Appendix I · Notation](appendix-notation.md)*
