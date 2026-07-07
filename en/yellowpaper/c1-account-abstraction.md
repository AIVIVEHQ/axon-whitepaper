---
title: C.1 Account Abstraction & Transaction Execution
description: Chain-native programmable accounts
---

# C.1 Account Abstraction & Transaction Execution

> **Design status**: proposed design. Native account abstraction; the interface may be adjusted along with the execution-layer design.

## C.1.1 Why Native, Not Relay-Based

Ethereum's account abstraction (ERC-4337) is an **application-layer** approach: it relies on off-chain bundlers, a separate mempool, and a contract EntryPoint for simulation, because the underlying account model is fixed. AXON is designed from the ground up, making account abstraction a **chain-native** capability—no relayers, no separate mempool; account abstraction *is* the protocol itself. This is necessary for the "controlled execution" of AI-agent payments ([C.2](c2-session-keys.md)): authorization constraints must be **enforced by the chain**, not dependent on off-chain honesty.

## C.1.2 Programmable Verification

A traditional account = one private key, with everything it can do and how it authorizes hard-coded. An AXON account carries a **verifier reference (`verifier`)**, turning "what counts as valid authorization" into a programmable predicate of the account:

$$\mathsf{Verify} : (\mathsf{Account}, \mathsf{Tx}) \to \{0,1\}$$

* **Default verifier**: Ed25519 single signature ([A.2.2](a2-cryptography.md))—equivalent to an ordinary external account.
* **Custom verifier**: an account may declare other schemes—multisig thresholds, passkey/secp256r1 (WebAuthn), social recovery, timelocks, or a **session-key policy** ([C.2](c2-session-keys.md)).

Verifier logic runs in a deterministic sandbox ([C.3](c3-policy-paymaster.md)), and its gas is charged to the transaction.

## C.1.3 Transaction and UserOperation

AXON's unified transaction object (native, not a secondary mempool):

```text
Tx := {
  sender     : Addr
  nonce      : u64
  calls      : [ Call ]        // one or more calls (native batching)
  auth       : AuthData        // authorization data required by the verifier (signature / session-key proof / …)
  gas        : GasParams       // caps, fee parameters (F.1)
  paymaster  : PaymasterRef?   // optional: fee sponsorship (C.3)
}
Call := { target: Addr, asset: AssetId?, amount: u128?, data: bytes? }
```

Compared with an ordinary chain transaction, AXON's `Tx` natively supports: **batch calls** (one authorization for multiple operations, executed atomically), **pluggable authorization** (`auth` is interpreted by the account's `verifier`), and **fee sponsorship** (`paymaster`).

## C.1.4 Execution Semantics

Transaction execution strictly follows $\delta_{\mathsf{tx}}$ from [B.3.2](b3-state.md), unfolding into the two phases of authorization and execution:

```text
δ_tx(st, tx):
  # —— Validation phase ——
  assert st[tx.sender].nonce == tx.nonce
  assert Verify(st[tx.sender], tx) == 1        # programmable verifier
  assert Auth(st[tx.sender], tx) == 1          # authorization-policy predicate (C.2)
  fee_payer ← resolve_paymaster(tx) or tx.sender
  assert can_afford(fee_payer, tx.gas)         # fee is payable (F.1)

  # —— Execution phase ——
  for call in tx.calls:                        # atomic batch
      apply(st, call)                          # transfer / settlement / contract call
  charge_gas(fee_payer, actual_gas)
  st[tx.sender].nonce += 1
  return st'
```

**Validation and execution are separated**: any failure in the validation phase leaves the transaction `Rejected` ([B.5.1](b5-finality.md)) and it never reaches execution; the batch calls in the execution phase are **atomic**—all succeed or all revert, eliminating the fund inconsistencies that partial execution would cause.

## C.1.5 Account Types

The same account model can express a full spectrum of capabilities:

| Type | verifier | Typical use |
| --- | --- | --- |
| Basic account | Ed25519 single signature | Personal wallet |
| Smart account | multisig / passkey / social recovery | Institutions, recoverable wallets |
| Session account | session-key policy ([C.2](c2-session-keys.md)) | Authorizing AI agents, subscriptions, automation |
| Merchant account | acquiring policy + compliance hooks | Merchant acquiring, settlement |

**Key point**: these are not different "kinds" of account, but different `verifier` configurations of the same programmable account—a unified model, no special cases. This provides the foundation for the session-key authorization of [C.2](c2-session-keys.md).

---

*Next: [C.2 Session Keys & the Authorization Model](c2-session-keys.md)*
