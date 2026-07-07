---
title: 3.3 Consensus, Sub-Second Finality & Performance Targets
description: And how we treat performance numbers honestly
---

# 3.3 Consensus, Sub-Second Finality & Performance Targets

## Why Consensus Decides a Payment's Fate

A chain's consensus mechanism determines whether it is fit for payments — because consensus answers exactly the two questions payments care about most: **"How soon is this transaction settled?"** and **"Once settled, can it be reversed?"**

For payments, the second question is especially deadly. That leads to a concept we must make crystal clear: **finality**.

## Two Kinds of Finality: Probabilistic vs. Deterministic

Not all "confirmations" are equal. Blockchains have two fundamentally different models of finality:

{% tabs %}
{% tab title="Probabilistic Finality" %}
**Probabilistic Finality**

The classic example is a Nakamoto-style proof-of-work chain. Once a transaction is included, the probability that it "gets reversed" decays exponentially as subsequent blocks pile up — but **it never strictly reaches zero**.

* Needing to "wait for N confirmations" is, in essence, waiting for the probability to become low enough;
* For large payments, you often have to wait a long time before it is "safe enough";
* For a payment scenario, this "almost certain" always leaves a faint tail of uncertainty.

**The problem**: payments need "funds have definitely arrived," not "funds have probably arrived."
{% endtab %}

{% tab title="Deterministic Finality" %}
**Deterministic / BFT Finality**

The classic example is BFT (Byzantine Fault Tolerant) style consensus. Once a transaction is confirmed by a quorum of validators, it is **immediately and irreversibly final** — there is no possibility of it being "replaced by a longer chain."

* No "waiting for N confirmations" — confirmation is finality;
* Finality can be achieved at the sub-second level;
* For payments, this is the true semantics of "the money has definitely arrived."

**AXON's choice**: deterministic finality. Payments cannot be built on probability.
{% endtab %}
{% endtabs %}

**AXON adopts high-throughput PoS + BFT-style deterministic finality consensus** — because only deterministic finality can give a payment a definite moment of "done."

## Performance Design Targets

Now for the numbers. As an honest whitepaper, we must first make a declaration:

| Metric | Design Target | Status |
| --- | --- | --- |
| Network throughput (TPS) | Sustained payment-grade throughput **> 10,000 TPS**, architecturally evolving toward higher orders of magnitude | `Design Target` |
| Deterministic finality | **Sub-second (< 1s, target range ~400–800ms)** | `Design Target` |
| Block time / block interval | **~250–500ms** | `Design Target` |
| Transaction fee | **Sub-cent (target < $0.001 / tx)**, predictable fixed-fee model | `Design Target` |
| Fee sponsorship | Paymaster support; users can complete payments without holding a gas token | proposed design |
| Consensus | High-throughput PoS + BFT-style deterministic finality | proposed design |
| Decentralization | Validator set starts relatively small, expands progressively per the roadmap (P2), and is handed to governance | roadmap |

Why these target ranges are reasonable and necessary for payments:

* **>10,000 TPS** — to carry global-scale payment and M2M micro-payment flow, throughput must leave an order-of-magnitude of headroom;
* **Sub-second finality** — payments happen inside real-world interactions (scanning a code, a tap, a machine call), so settlement must be fast enough to feel instantaneous;
* **~250–500ms block time** — sets the lower bound from submission to settlement, directly shaping how it feels to the user;
* **Sub-cent fees** — pay-per-call M2M micro-payments simply do not work economically if a single transaction costs more than a few cents.

## Predictability Matters More Than "Cheaper"

For payments, a point often overlooked is this: **the predictability of a fee often matters more than its absolute level.** A merchant can accept "a fixed 0.1 cent per transaction," but cannot accept "a fee that jumps between 0.1 cent and $5 with network congestion" — the latter puts pricing, reconciliation, and the entire business model out of control.

So AXON's goal is not merely "low fees" but a **"low and predictable"** fee model: the cost of a payment should behave like stable infrastructure pricing, not an auction price that swings with market sentiment. This is precisely what general-purpose chains struggle to provide by design.

---

*Further reading: [3.4 Payment Finality & Double-Spend Prevention](3-4-payment-finality.md) · [Appendix B · Parameter Summary](../appendix/b-parameters.md)*
