---
title: C.3 Policy Sandbox & Paymaster
description: Verifiable policy execution and fee sponsorship
---

# C.3 Policy Sandbox & Paymaster

> **Design status**: proposed design. The WASM sandbox and Paymaster protocol are design proposals.

## C.3.1 The Verifiable Payment-Policy Sandbox

The programmable verifier of [C.1](c1-account-abstraction.md) and the authorization predicate of [C.2](c2-session-keys.md) need an execution environment. AXON uses a **WASM sandbox** to run account verification logic and payment policies. Reasons for choosing WASM: mature, portable, deterministic-izable, and precisely gas-meterable.

The sandbox must satisfy three hard properties:

* **Determinism**: the same input must produce the same output. Non-deterministic sources such as floating-point non-determinism, system time, and true randomness are disabled; randomness may come only from the on-chain beacon ([A.2.5](a2-cryptography.md)). Determinism is the prerequisite for replayability of the [B.3.2](b3-state.md) state transition $\delta$.
* **Gas metering**: each instruction is priced in gas, and execution aborts once it exceeds budget ([F.1](f1-gas-fees.md))—preventing policy logic from becoming a DoS vector.
* **Capability security**: policy code can only access explicitly granted capabilities (reading specific state, initiating restricted calls), with no ambient authority. A payment policy cannot overreach and touch resources the policy author did not authorize.

$$\mathsf{Sandbox} : (\text{code},\ \text{input},\ \text{gas\_limit}) \to (\text{output},\ \text{gas\_used}) \ \big|\ \bot_{\text{OOG}}$$

**Verifiability**: because execution is deterministic and gas-metered, any node replaying policy execution must obtain the same result—you can **prove** that the agent will only act according to the stated policy. This turns the authorization constraints of [C.2](c2-session-keys.md) from a "declaration" into "verifiable enforcement".

## C.3.2 Paymaster: The Fee-Sponsorship Protocol

The last friction point in the payment experience is gas: a user/agent must hold the native gas token before they can move stablecoins. **Paymaster** allows a third party (application, merchant, protocol) to sponsor a transaction's gas, so the user/agent needs no gas token (whitepaper [3.7](../part3-architecture/3-7-account-abstraction.md)).

A transaction's `paymaster` field ([C.1.3](c1-account-abstraction.md)) references a sponsor. Resolving the fee-paying principal at execution time:

```text
resolve_paymaster(tx):
  if tx.paymaster == null: return tx.sender
  pm ← st[tx.paymaster]
  assert pm.validate(tx) == 1          # sponsor policy: is it willing to sponsor this tx
  assert pm.deposit ≥ max_fee(tx)      # sponsor has sufficient deposit
  return tx.paymaster                   # gas paid by the sponsor
```

## C.3.3 Paymaster Validation Sequence

```mermaid
sequenceDiagram
    autonumber
    participant U as User / AI agent
    participant PM as Paymaster
    participant P as Protocol (execution layer)
    U->>P: submit tx (paymaster=PM, holds stablecoin only)
    P->>PM: validate(tx)? (sponsor policy + deposit check)
    PM-->>P: agrees to sponsor
    P->>P: execute tx; gas deducted from PM.deposit
    P-->>U: payment complete (user never touches gas)
    Note over PM,P: sponsorship fails / insufficient deposit → tx Rejected (B.5.1)
```

## C.3.4 Paymaster Abuse Prevention

A sponsor is a public resource and must be protected from being drained:

* **Deposit + pre-authorization**: `max_fee` is locked before sponsorship, settled by actual usage after execution, and the balance refunded—ensuring the sponsor is always solvent.
* **Sponsor policy**: `pm.validate` is the sponsor's custom predicate (also running in the sandbox), which can restrict sponsorship to "only for allowlisted applications / specific calls / within a cap"—preventing arbitrary transactions from freeloading.
* **Rate and quota**: a sponsor can set per-user/per-period quotas to resist gas-drain attacks.

## C.3.5 The Three Primitives Working Together

At this point, AXON's three "AI-native" foundational primitives form a closed loop:

| Primitive | Role | Section |
| --- | --- | --- |
| Account abstraction | makes accounts programmable and authorization pluggable | [C.1](c1-account-abstraction.md) |
| Session keys + policy sandbox | gives agents bounded, verifiable, revocable payment authority | [C.2](c2-session-keys.md) · this section |
| Paymaster | agents need not manage gas, and focus on payment itself | this section |

Together they realize the "controlled payment execution" of whitepaper [5.2](../part5-ai/5-2-controlled-execution.md)—**not adding an AI feature to the chain, but making the chain's authorization model assume, from the foundation up, that the payer may be a machine that needs to be constrained**. This same set of primitives also serves human users: the **zero-gas one-click copy-trading** of the copy-trading engine ([E.3.6](e3-copy-trading.md)) has the prediction platform act as Paymaster to sponsor the copy-trading gas, so the user never has to perceive gas at all.

---

*Next: [D.1 Stablecoin Settlement Engine](d1-settlement.md)*
