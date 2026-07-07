---
title: D.1 Stablecoin Settlement Engine
description: Chain-native multi-asset atomic settlement
---

# D.1 Stablecoin Settlement Engine

> **Design status**: proposed design. The list of supported stablecoins is TBD ([Appendix II](appendix-parameters.md)).

## D.1.1 Settlement as a First-Class Capability

On a general-purpose chain, a stablecoin transfer is an ERC-20 contract call—a stablecoin is just "a token that happens to be called USDC". AXON makes **stablecoin settlement a chain-layer primitive**: stablecoin balances are a first-class field of the account (`balances` in [B.3.1](b3-state.md)), and settlement is handled directly by the settlement engine rather than by the internal bookkeeping of some contract. The benefits: a shorter settlement path, predictable gas, and native integration with account abstraction / the compliance gateway / finality proofs.

## D.1.2 The Multi-Asset Model

An account balance is a mapping from asset to amount:

$$\mathsf{balances} : \mathsf{AssetId} \to \mathbb{Z}_{\geq 0}$$

$\mathsf{AssetId}$ covers the native gas asset and multiple fiat-pegged stablecoins. Design principles (proposed):

* **Multi-asset, not bound to a single issuer**—reducing dependence on any one stablecoin.
* **Fiat-pegged first**—the initial launch focuses on USD and other fiat-pegged stablecoins, which are the current mainstay of cross-border settlement.
* **Asset metadata**: each $\mathsf{AssetId}$ is associated with an issuer, precision, a compliance tag (for the [D.3](d3-compliance.md) gateway to evaluate), and a price feed source ([D.2](d2-oracle.md)).

## D.1.3 Settlement Primitives

The core primitives provided by the settlement engine (all atomic—either fully succeed or fully abort):

| Primitive | Semantics |
| --- | --- |
| `Transfer(a → b, asset, amt)` | Single-asset transfer, balance-conserving |
| `BatchSettle([...])` | Atomic settlement of multiple transfers (batch acquiring / disbursement) |
| `PvP(a↔b, x↔y)` | Payment-versus-payment: atomic swap of two assets, with no risk of one party paying first |
| `Escrow / Conditional` | Conditional settlement: releases only when a predicate is satisfied (e.g. delivery confirmation, or an oracle proof that a copy-trading round has settled, [E.3.4](e3-copy-trading.md)) |

**Balance conservation** is the invariant of the settlement engine: for any asset, apart from minting/burning (by the issuer via an authorized path), the total amount is constant before and after settlement:

$$\forall\, \mathsf{asset}:\quad \sum_{a} \mathsf{balances}[a][\mathsf{asset}] = \text{const}$$

This invariant can be checked after every settlement, and is the settlement-layer manifestation of double-spend prevention ([B.5.2](b5-finality.md)).

## D.1.4 Atomicity and Finality

Settlement atomicity is guaranteed by the execution layer: all transfers within a `BatchSettle`/`PvP` execute in the same $\delta_{\mathsf{tx}}$ ([C.1.4](c1-account-abstraction.md)), and if any fails the whole thing reverts—there is no half-finished "A paid, B didn't" state.

Settlement finality is guaranteed by [B.5](b5-finality.md): once a settlement transaction is `Finalized`, it is irreversible. Combined with the provable state root of [B.3.4](b3-state.md), the settlement engine provides a **verifiable receipt** for every settlement—external systems (fiat on/off-ramps, merchant back offices, audits) can confirm, using the state root plus an inclusion proof, that "this has settled T+0, the amount is certain, and it is irreversible", without trusting any single node. This is the protocol realization of the "instant stablecoin settlement rail" (whitepaper [4.1](../part4-payfi/4-1-settlement-rail.md)).

## D.1.5 The Interface to the Money Market

The settlement engine provides the underlying bookkeeping for the PayFi money market ([E.1](e1-money-market.md)): the fund movements for float, receivables financing, and liquidation all execute through the settlement primitives, sharing the same balance-conservation and finality guarantees—the money market need not reimplement bookkeeping, and only expresses credit logic on top of it.

---

*Next: [D.2 Oracle & Price-Feed Safety](d2-oracle.md)*
