---
title: A.1 System Model, Assumptions & Security Goals
description: Network model, fault model, and security properties
---

# A.1 System Model, Assumptions & Security Goals

> **Design status**: proposed design. This section establishes the formal premises for the whole text.

## A.1.1 Participants

The AXON network consists of the following roles. Let the full set of participants be $\mathcal{P}$.

| Role | Notation | Responsibilities |
| --- | --- | --- |
| Validator | $\mathcal{V} = \{v_1,\dots,v_n\}$ | Participate in consensus, produce and verify blocks, maintain state |
| Sequencer | $\mathsf{Seq}$ | Assign global sequence numbers to transactions, write the Entry-Log (early-stage centralized but verifiable; see [B.4](b4-sequencing.md)) |
| Client | $\mathcal{C}$ | Users, wallets, and merchants that submit transactions |
| AI agent | $\mathcal{A} \subseteq \mathcal{C}$ | Automated principals that hold restricted session keys and initiate payments on behalf of users (see [C.2](c2-session-keys.md)) |
| Liquidity provider | $\mathcal{L}$ | Inject funds into the PayFi money market (see [E.1](e1-money-market.md)) |
| Price-feed node | $\mathcal{O}$ | Provide fiat-anchored price data (see [D.2](d2-oracle.md)) |

Each validator $v_i$ is associated with a stake $s_i \geq 0$, with total stake $S = \sum_{i} s_i$. Consensus is **stake-weighted**, not counted per node.

## A.1.2 Network Model

AXON adopts the **partial synchrony** network model (Dwork–Lynch–Stockmeyer):

$$\exists\, \Delta,\ \mathrm{GST}: \quad t \geq \mathrm{GST} \implies \text{any message is delivered within } \Delta$$

where $\mathrm{GST}$ (Global Stabilization Time) is unknown and $\Delta$ is bounded. Meaning: the network may experience asynchronous periods of arbitrary length (unbounded message delay), but eventually enters a synchronous period. This is the realistic assumption for production-grade BFT systems — it lets the protocol be **independent of timing assumptions for safety**, and depend on post-GST synchrony **only for liveness**.

## A.1.3 Fault Model

Validators may suffer **Byzantine faults**: faulty nodes may deviate from the protocol arbitrarily (crash, go silent, send contradictory messages, collude). Let the stake held by Byzantine nodes be $S_f$. AXON's safety argument rests on the standard BFT threshold:

$$S_f < \tfrac{1}{3} S$$

i.e., honest stake is strictly greater than $\tfrac{2}{3} S$. A **quorum** is defined as a set of validators holding more than $\tfrac{2}{3} S$ of stake; the stake intersection of any two quorums must contain an honest node, which is the algebraic basis of safety (see [B.1](b1-consensus.md)).

## A.1.4 Security Goals

The AXON protocol must satisfy three properties. Let $\mathsf{commit}(b, h)$ denote that block $b$ is finalized at height $h$.

**Safety** — no two honest validators finalize different blocks at the same height $h$:

$$\forall h:\ \mathsf{commit}(b,h) \wedge \mathsf{commit}(b',h) \implies b = b'$$

Holds **unconditionally** under $S_f < \tfrac{1}{3}S$ (even if the network is fully asynchronous).

**Liveness** — a valid transaction submitted by an honest client is eventually finalized. Guaranteed only during a synchronous period with $t \geq \mathrm{GST}$.

**Deterministic Finality** — once $\mathsf{commit}(b,h)$ holds, $b$ is **never rolled back**. There is no probabilistic process of "waiting for $N$ confirmations"; confirmation is finality.

Together these three form the determinism bedrock that payments require. **Safety takes precedence over liveness**: under extremes such as a network partition, AXON would rather halt block production (sacrificing liveness) than produce conflicting confirmations (preserving safety) — for a payment system, "pause" is far better than "double-spend."

## A.1.5 Additional Payment-Specific Goals

On top of the general BFT goals, the payment scenario requires:

* **No Double-Spend**: the same funds cannot be spent successfully twice — guaranteed by global monotonic sequence numbers + deterministic execution ([B.4](b4-sequencing.md), [B.5](b5-finality.md)).
* **Replayability**: given the Entry-Log, any node can reconstruct exactly the same state.
* **Recoverability**: after a crash, the system can recover to a consistent state from the write-ahead log, with no loss of funds.
* **Bounded Authorization**: the payment capability of any automated principal is enforced by the chain layer within revocable bounds ([C.2](c2-session-keys.md)).

## A.1.6 Summary of Trust Assumptions

| Component | Trust Assumption |
| --- | --- |
| Consensus safety | $S_f < \tfrac{1}{3}S$, no timing assumptions |
| Consensus liveness | $S_f < \tfrac{1}{3}S$ and $t \geq \mathrm{GST}$ |
| Sequencer (early stage) | Centralized but **verifiable**: must prove its log is tamper-evident and can be fully replayed (trust-minimized, decentralizing along with P2; see [B.4](b4-sequencing.md)) |
| Price feeds | A majority of feed sources are honest + a deviation circuit breaker as backstop ([D.2](d2-oracle.md)) |
| Cryptography | Standard hardness assumptions of Ed25519 / BLS12-381 / collision-resistant hashes ([A.2](a2-cryptography.md)) |

---

*Next: [A.2 Cryptographic Primitives & Commitments](a2-cryptography.md)*
