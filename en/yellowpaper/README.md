---
title: AXON Finance Technical Yellowpaper
description: Protocol technical design specification
---

# Technical Yellowpaper

> **This yellowpaper is the protocol technical design specification for AXON Finance.**

The whitepaper answers "why" and "what"; this yellowpaper answers "**how to build it**." It takes the architectural concepts from whitepaper [Part III · Technical Architecture](../part3-architecture/README.md) and [Part V · AI-Native](../part5-ai/README.md) and deepens them to the **protocol-specification level** — using formal notation, pseudocode, data structures, and state machines to lay out AXON L1's design layer by layer.

## On the Nature of This Document

> **This document describes a proposed design (a design specification), not a system that is already implemented or deployed.**
>
> AXON is in the design and development stage. The protocol mechanisms, algorithms, and parameters given in this yellowpaper are **engineering design targets**: the specific choices within (consensus algorithm, signature scheme, state structure, etc.) represent current design decisions, and the final implementation may be adjusted in details; parameters labeled `Design Target` or `TBD` will be finalized after testnet benchmarking and mainnet launch. **Features not yet live are not claimed as implemented.**

For readability, this document labels the degree of certainty of each section's content with a one-line `Design status` note, rather than repeating long disclaimers. The full basis-grading is consistent with the whitepaper.

## Design Goals

AXON's protocol design centers on a single core proposition: **provide determinism for payments and settlement**. From it derive four hard constraints that run through the whole text:

1. **Deterministic finality** — a transaction is irreversible once confirmed, without relying on probabilistic accumulation.
2. **Verifiability and recoverability** — every state change can be independently replayed, audited, and recovered.
3. **Bounded authorization** — the payment authority of accounts and AI agents can be constrained precisely and revocably.
4. **Predictable cost and performance** — a payment-grade SLA: high throughput, sub-second finality, and low, predictable fees.

## Relationship to the Whitepaper

| Whitepaper Section | Yellowpaper Deepening |
| --- | --- |
| 3.2 Five-layer architecture | Developed layer by layer into a protocol specification throughout |
| 3.3 Consensus & performance | [B.1](b1-consensus.md) Consensus protocol · [G.2](g2-performance.md) Performance model |
| 3.4 Payment finality | [B.4](b4-sequencing.md) Sequencing/Entry-Log · [B.5](b5-finality.md) Finality |
| 3.5 Price-feed safety | [D.2](d2-oracle.md) Oracle & price feeds |
| 3.6 Compliance gateway | [D.3](d3-compliance.md) Pluggable compliance |
| 3.7 Account abstraction / session keys | [C.1](c1-account-abstraction.md)–[C.3](c3-policy-paymaster.md) |
| 4.2 / 4.4 PayFi money market | [E.1](e1-money-market.md)–[E.2](e2-liquidation.md) |
| 4.5 US-equity copy-trading engine | [E.3](e3-copy-trading.md) Engine protocol · [E.4](e4-reserve-risk.md) Reserve & risk control |
| 5.2 Controlled payment execution | [C.2](c2-session-keys.md) Session-key authorization model |

## Chapter Map

* **A · System Model & Cryptography** — network/fault model, security goals, cryptographic primitives.
* **B · Consensus & Ledger** — BFT PoS consensus, validators, state model, sequencing layer, payment finality.
* **C · Account Abstraction & AI Authorization** — native account abstraction, session-key authorization model, policy sandbox and Paymaster.
* **D · Settlement · Price Feeds · Compliance** — stablecoin settlement engine, oracle safety, pluggable compliance gateway.
* **E · PayFi Protocol Mechanisms** — on-chain money market, credit risk control and liquidation, the US-equity copy-trading engine (the first open ecosystem) and its reserve risk control.
* **F · Protocol Economics & Security** — gas and fee market, staking and slashing, security and threat model.
* **G · Networking & Performance** — P2P networking layer, performance model and derivation of design targets.
* **Appendix** — notation table, protocol parameter table, core data-structure definitions.

## Reading Conventions

* Formal notation is defined uniformly in [Appendix I · Notation](appendix-notation.md).
* All protocol parameters are consolidated in [Appendix II · Protocol Parameters](appendix-parameters.md), labeled with the three states `proposed / Design Target / TBD`.
* Core data structures are defined in [Appendix III](appendix-datastructures.md).
* Pseudocode is **illustrative** — used to express algorithmic intent, not final APIs or implementations.

---

*Next: [A.1 System Model, Assumptions & Security Goals](a1-system-model.md)*
