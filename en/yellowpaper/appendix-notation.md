---
title: Appendix I · Notation
description: Formal notation conventions used throughout
---

# Appendix I · Notation

The formal notation used throughout is defined uniformly here for reference by each section.

## System & Consensus

| Symbol | Meaning | Source |
| --- | --- | --- |
| $\mathcal{V} = \{v_1,\dots,v_n\}$ | Validator set | [A.1.1](a1-system-model.md) |
| $s_i$ | Effective stake of validator $v_i$ (self + delegated) | [A.1.1](a1-system-model.md)·[F.2.1](f2-staking-slashing.md) |
| $S = \sum_i s_i$ | Total stake | [A.1.1](a1-system-model.md) |
| $S_f$ | Byzantine-node stake, assumed $S_f < \tfrac13 S$ | [A.1.3](a1-system-model.md) |
| $\Delta,\ \mathrm{GST}$ | Message-delay bound, global stabilization time (partial synchrony) | [A.1.2](a1-system-model.md) |
| $r$ | View (consensus round) | [B.1.2](b1-consensus.md) |
| $\mathsf{QC}(b)$ | Quorum certificate of block $b$ (stake $>\tfrac23 S$) | [B.1.2](b1-consensus.md) |
| $\mathsf{Leader}(r)$ | Leader of view $r$ (VRF-derived) | [B.2.3](b2-validators.md) |
| $\mathsf{commit}(b,h)$ | Block $b$ finally committed at height $h$ | [A.1.4](a1-system-model.md) |
| $T_{\text{final}}$ | Finality latency | [B.1.7](b1-consensus.md)·[G.2.2](g2-performance.md) |

## Cryptography

| Symbol | Meaning | Source |
| --- | --- | --- |
| $H(\cdot)$ | Collision-resistant hash (BLAKE3) | [A.2.1](a2-cryptography.md) |
| $\Sigma=(\mathsf{KeyGen},\mathsf{Sign},\mathsf{Vrf})$ | Signature scheme (Ed25519) | [A.2.2](a2-cryptography.md) |
| $\sigma_{\mathsf{agg}}$ | BLS aggregate signature | [A.2.3](a2-cryptography.md) |
| $\mathsf{root}(\mathsf{st})$ | State commitment root | [A.2.4](a2-cryptography.md)·[B.3.3](b3-state.md) |
| $\mathsf{VRF}_{sk}(x)\to(y,\pi)$ | Verifiable random function | [A.2.5](a2-cryptography.md) |

## State & Execution

| Symbol | Meaning | Source |
| --- | --- | --- |
| $\mathsf{st}:\mathsf{Addr}\to\mathsf{Account}$ | Global state | [B.3.1](b3-state.md) |
| $\delta,\ \delta_{\mathsf{tx}}$ | State-transition function (block / single transaction) | [B.3.2](b3-state.md) |
| $\mathsf{seqNo}(\mathsf{tx})$ | Global monotonic sequence number | [B.4.1](b4-sequencing.md) |
| $\mathsf{Log},\ e_k,\ h_k$ | Entry-Log, log entry, hash chain | [B.4.2](b4-sequencing.md) |
| $\mathsf{Replay}(\mathsf{st}_0,\mathsf{Log})$ | Replay function | [B.4.3](b4-sequencing.md) |

## Authorization & AI

| Symbol | Meaning | Source |
| --- | --- | --- |
| $\mathsf{Verify}(\mathsf{Account},\mathsf{Tx})$ | Programmable verifier | [C.1.2](c1-account-abstraction.md) |
| $P=\{c_1,\dots,c_k\}$ | Authorization policy (constraint set) | [C.2.2](c2-session-keys.md) |
| $\mathsf{Auth}_P(\mathsf{tx},\mathsf{ctx})=\bigwedge_c c$ | Authorization predicate | [C.2.3](c2-session-keys.md) |
| $L_{\text{total}},L_{\text{tx}},W,\mathcal{S},\rho$ | Total limit / per-tx cap / allowlist / asset scope / frequency | [C.2.2](c2-session-keys.md) |
| $\mathsf{spent},\mathsf{count}$ | Session cumulative spend / window count | [C.2.3](c2-session-keys.md) |

## PayFi & Economics

| Symbol | Meaning | Source |
| --- | --- | --- |
| $U = B/D$ | Utilization (borrowed / supplied) | [E.1.2](e1-money-market.md) |
| $r_{\text{borrow}}(U),\ r_{\text{supply}}(U)$ | Borrow / supply interest-rate curves | [E.1.3](e1-money-market.md) |
| $U^{*}$ | Optimal-utilization kink | [E.1.3](e1-money-market.md) |
| $H=\sum C_i\lambda_i/\text{Debt}$ | Health factor | [E.1.5](e1-money-market.md) |
| $\lambda_i$ | Liquidation threshold of collateral $i$ | [E.1.5](e1-money-market.md) |
| $\phi$ | Reserve factor | [E.1.3](e1-money-market.md)·[E.2.4](e2-liquidation.md) |
| $\hat{p},\ \mathrm{MAD},\ z_i$ | Aggregated price / median absolute deviation / deviation score | [D.2](d2-oracle.md) |
| $g$ | Copy-trading guaranteed floor ($\in[1\%,3\%]$) | [E.3.5](e3-copy-trading.md) |
| $\chi$ | Post-hedge max loss ratio per round | [E.4.2](e4-reserve-risk.md) |
| $R_{ct}$ | Copy-trading reserve pool | [E.4.3](e4-reserve-risk.md) |
| $\Xi=R_{ct}/\sum\text{MaxLoss}$ | Copy-trading reserve coverage ($\geq\Xi_{\min}=150\%$) | [E.4.3](e4-reserve-risk.md) |
| $\phi_{ct}$ | Copy-trading reserve injection rate | [E.4.3](e4-reserve-risk.md) |
| $\Delta_{\text{chal}}$ | Settlement challenge window | [E.3.5](e3-copy-trading.md) |
| $\mathsf{gas}(\mathsf{tx})$ | Transaction gas | [F.1.1](f1-gas-fees.md) |
| $T_{\text{unbond}}$ | Unbonding period | [F.2.4](f2-staking-slashing.md) |
| $\Theta,\ \text{fee}$ | Throughput / per-transaction fee | [G.2](g2-performance.md) |

---

*Next: [Appendix II · Protocol Parameters](appendix-parameters.md)*
