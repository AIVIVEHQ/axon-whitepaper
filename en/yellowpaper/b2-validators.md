---
title: B.2 Validator Set & Block Production
description: Stake, leader election, and set evolution
---

# B.2 Validator Set & Block Production

> **Design status**: proposed design. The staking threshold, epoch length, and validator-set size are TBD parameters ([Appendix II](appendix-parameters.md)).

## B.2.1 Stake & Epochs

System time is divided into **epochs** (intervals of a fixed number of views or fixed duration). The validator set $\mathcal{V}_e$ is **frozen** within epoch $e$ — set changes (join/leave/stake adjustment) take effect uniformly at epoch boundaries. This gives consensus a stable voting-weight baseline, and lets light clients track validator-set changes only at the epoch level.

The voting weight of validator $v_i$ is proportional to its **effective stake** $s_i$ (own stake + delegation, see [F.2](f2-staking-slashing.md)). All quorum decisions are by stake, not by node count.

## B.2.2 Admission & Exit

$$\text{Join}:\ s_i \geq s_{\min} \ \Rightarrow\ \text{enter the activation queue} \xrightarrow{\text{epoch boundary}} \mathcal{V}_{e+1}$$

* **Activation queue**: new validators queue up and are activated epoch by epoch at a rate limit, preventing the set from being flooded in an instant and destabilized.
* **Exit queue + unbonding period**: after a validator exits, its stake enters an **unbonding period** $T_{\text{unbond}}$ before it can be withdrawn. The unbonding period covers the possible slashing accountability window ([F.2](f2-staking-slashing.md)) — preventing a malicious actor from immediately withdrawing to escape slashing.
* **Cap**: the number of validators has a soft cap $n_{\max}$ (to control consensus communication cost); those beyond it compete for selection by stake, with the rest serving as candidates.

## B.2.3 Leader Election (VRF, stake-weighted)

The leader of each view $r$ is determined by VRF ([A.2.5](a2-cryptography.md)), with the probability of selection proportional to stake:

$$\Pr[\mathsf{Leader}(r) = v_i] = \frac{s_i}{S}$$

Flow: with the random beacon $\mathsf{beacon}_e$ (derived from the aggregation of the previous epoch's VRF outputs, not manipulable by any single party) as the seed, $x = (\mathsf{beacon}_e, r)$; each validator locally computes $\mathsf{VRF}_{sk_i}(x)$ and decides, by a stake threshold, whether it is selected.

Three properties of VRF election:

* **Unpredictable**: the leader sequence cannot be known in advance before the beacon is determined — compressing the room for premeditated censorship and MEV.
* **Verifiable**: the selected node presents $\pi$, and anyone can verify its legitimacy — a leader cannot be impersonated.
* **Unbiased**: the probability of selection is strictly by stake, with no dilution.

## B.2.4 Block Production

Leader $\mathsf{Leader}(r)$ takes a batch from the **ordered transaction stream delivered by the sequencing layer** ([B.4](b4-sequencing.md)) and packages it into a block:

```text
ProduceBlock(r):
  parent  ← the block pointed to by the local highest QC
  txs     ← the contiguous interval [seqNo_start .. seqNo_end] delivered by the Sequencer
  header  ← { parent_hash, height, view=r, state_root_prev,
              tx_root = Merkle(txs), timestamp, leader_vrf_proof }
  b       ← { header, txs, QC_just = parent's QC }
  sign and broadcast b
```

Key constraint: the `seqNo`s of the transactions in a block must be a **contiguous interval** that strictly continues from the parent block — the leader cannot skip, reorder, or inject sequence numbers (ordering authority is separated from block-production authority, see [B.4](b4-sequencing.md)), which limits the leader's ability to manipulate ordering.

## B.2.5 Delegation

Token holders can **delegate** their stake to a validator, sharing in its consensus rewards and jointly bearing slashing risk, without running a node themselves. Delegation is a pure consensus/security mechanism; its mathematics and reward distribution are covered in [F.2](f2-staking-slashing.md). Delegation lowers the capital threshold of the validator set and improves decentralization, while not increasing the number of voting principals in consensus (delegations are aggregated under the validator).

## B.2.6 Progressive Decentralization

Consistent with the trust assumptions of [A.1.6](a1-system-model.md), the validator set expands progressively along the roadmap (P2 of whitepaper [6.1](../part6-roadmap/6-1-roadmap.md)): small at the start to ensure performance and fast iteration; then, having built credibility through slashing, the unbonding period, and unbiased VRF election, it progressively opens admission, raises $n_{\max}$, and pushes up the degree of decentralization of both sequencing and validation.

---

*Next: [B.3 State Model & State Commitments](b3-state.md)*
