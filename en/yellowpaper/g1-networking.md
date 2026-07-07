---
title: G.1 Network Layer & Transaction Propagation
description: P2P, mempool, and block propagation
---

# G.1 Network Layer & Transaction Propagation

> **Design status**: proposed design. Network parameters (fanout, mempool size) await benchmarking.

## G.1.1 Network Topology

AXON nodes form a **structured P2P network**: validators maintain low-latency near-full connectivity among themselves (the count is controlled, $n \leq n_{\max}$, [B.2.2](b2-validators.md)), while full nodes / edge nodes connect via a gossip overlay. Consensus messages (proposals, votes, QCs) travel over the validator subnet to minimize finality latency; transaction and block bodies travel over the broader gossip layer.

## G.1.2 Transaction Propagation & Mempool

Client transactions first undergo compliance screening at the access gateway ([D.3](d3-compliance.md)), then enter the **mempool**. Design points:

* **Front-loaded compliance**: non-compliant transactions are blocked before entering the pool, reducing invalid propagation.
* **Ordered by the sequencing layer**: transactions are assigned a global seqNo by $\mathsf{Seq}$ ([B.4.1](b4-sequencing.md)); the leader takes transactions from the **already-sequenced stream** rather than choosing an ordering itself from an unordered mempool — this compresses the MEV/censorship surface from the network layer up.
* **Encrypted mempool (roadmap)**: transactions can be encrypted to the sequencer before sequencing (threshold decryption), further preventing front-running ([F.3.1](f3-security.md)).

## G.1.3 Block Propagation

HotStuff-style consensus ([B.1](b1-consensus.md)) relays through the leader with communication complexity $O(n)$ (paired with BLS aggregation, [A.2.3](a2-cryptography.md)), avoiding network-wide $O(n^2)$ broadcast. Propagation of large block bodies can use two optimizations:

* **Erasure coding**: block sharding + redundant encoding — a node can reconstruct once it receives enough shards, resisting packet loss and lowering tail latency.
* **Compact blocks**: since transactions have already propagated in the mempool, a block need only carry transaction identifiers + missing items, saving bandwidth.

## G.1.4 Data Availability

Before voting on a block, validators must confirm the **block data is available** (that they can fetch all transactions to execute $\delta$, [B.3.2](b3-state.md)) — preventing the leader from proposing a block whose "header is valid but body is unobtainable." Erasure coding + sampling lets nodes confirm data availability at low cost with high probability, forming the basis for stateless / light-verification evolution ([B.3.5](b3-state.md)).

## G.1.5 Relationship Between Network and Performance

The network layer is the physical lower bound on throughput and latency: one-hop latency $\delta_{\text{net}}$ determines finality latency ([B.1.7](b1-consensus.md)), and block-propagation bandwidth determines the upper bound on block size, which in turn determines the number of transactions per block. [G.2](g2-performance.md) combines these physical quantities into a performance model and derives AXON's performance Design Targets.

---

*Next: [G.2 Performance Model & Design Targets](g2-performance.md)*
