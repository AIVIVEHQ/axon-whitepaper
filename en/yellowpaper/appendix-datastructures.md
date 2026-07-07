---
title: Appendix III · Core Data Structures
description: Definitions of blocks, transactions, accounts, and proofs
---

# Appendix III · Core Data Structures

> **Design status**: proposed design. The following are illustrative definitions; fields and encodings adjust with implementation; type notation is only indicative (`u64` = 64-bit unsigned integer, etc.).

## Block

```text
Block := {
  header : BlockHeader
  txs    : [ Tx ]              // contiguous seqNo interval (B.2.4)
}

BlockHeader := {
  parent_hash  : Hash         // parent block hash
  height       : u64          // block height
  view         : u64          // consensus view r
  tx_root      : Hash         // Merkle(txs)
  state_root   : Hash         // post-execution state commitment (B.3.3)
  log_head     : Hash         // Entry-Log hash-chain head h_k (B.4.5)
  timestamp    : u64
  leader       : ValidatorId
  leader_vrf   : VrfProof     // leader legitimacy proof (B.2.3)
  justify_qc   : QuorumCert   // QC on parent (B.1.2)
}
```

## Quorum Certificate

```text
QuorumCert := {
  block_hash : Hash           // the certified block
  height     : u64
  view       : u64
  agg_sig    : BlsAggSig      // aggregate vote signature (A.2.3)
  signers    : Bitmap         // participating-validator bitmap (used to recover stake)
}
// validity: signers correspond to stake > (2/3)·S and agg_sig verifies
```

## Transaction (Tx)

```text
Tx := {
  sender    : Addr
  nonce     : u64             // anti-replay (B.3.2)
  calls     : [ Call ]        // atomic batch calls (C.1.3)
  auth      : AuthData        // authorization required by the verifier (signature / session-key proof)
  gas       : GasParams
  paymaster : PaymasterRef?   // optional fee sponsorship (C.3.2)
}

Call := { target: Addr, asset: AssetId?, amount: u128?, data: bytes? }

GasParams := { limit: u64, price_params: FeeParams }
```

## Account

```text
Account := {
  nonce        : u64
  balances     : Map<AssetId, u128>   // multi-asset first-class balances (D.1.2)
  verifier     : VerifierRef          // programmable verification logic (C.1.2)
  policy_root  : Hash?                // session-key/authorization-policy commitment (C.2.2)
  code         : Hash?                // optional: WASM policy/contract commitment (C.3)
  storage_root : Hash?                // optional: account storage subtree root
}
```

## Session-Key Policy (SessionPolicy)

```text
SessionPolicy := {
  session_id : Hash
  pubkey     : PubKey                 // session-key public key
  scope : {
    L_total   : u128                  // total limit (cumulative)
    L_tx      : u128                  // per-tx cap
    window    : (t0: u64, t1: u64)    // time window
    allowlist : [ Addr ]              // allowlist W
    assets    : [ AssetId ]           // asset scope S
    rate      : (max: u32, per: u64)  // frequency ρ
    calls     : [ CallPattern ]       // call scope F
  }
  state : {                           // on-chain attested session state (C.2.3)
    spent : u128
    count : u32
    revoked : bool
  }
}
```

## Copy-Trading Round (CopyRound)

```text
CopyRound := {                        // official copy-trading round (E.3.3)
  round_id   : Hash
  asset      : SymbolId               // underlying (top-100 by market cap or core ETF)
  t_cat      : u64                    // deterministic catalyst timestamp
  g          : Ratio                  // guaranteed floor ∈ [1%,3%] (E.3.5)
  cap        : u128                   // total per-round copy-trade cap
  tier_min   : u8                     // minimum participation tier (Silver/Gold/Diamond)
  window     : (t_open: u64, t_settle: u64)   // settlement window 2–24h
  state      : Open | Locked | Settling | Settled | Aborted | Refunded  // E.3.4
  max_loss   : u128                   // max possible post-hedge loss χ·P (E.4.2)
}
```

## Escrow Account (EscrowAccount)

```text
EscrowAccount := {                    // per-round escrow, an Escrow/Conditional instance (D.1 / E.3.4)
  round_id   : Hash
  principals : Map<Addr, u128>        // each copy-trader's principal p_i
  total_P    : u128                   // total copy-trade amount ΣP_i ≤ cap
  release    : Predicate              // release predicate = "oracle proves this round has settled"
  isolation  : { user, desk, treasury strictly isolated }   // E.3.2 invariant
}
```

## Settlement Attestation (SettlementAttestation)

```text
SettlementAttestation := {            // on-chain proof of off-chain settlement result (E.3.5)
  round_id   : Hash
  net_pnl    : i128                   // proven off-chain net PnL
  price_ref  : PriceProof             // settlement price: multi-source median + MAD (D.2)
  sig        : AttestSig              // quant-desk signature, verified by Vrf_attest (D.3.4)
  challenge  : (until: u64, disputed: bool)   // challenge window Δ_chal
}
```

## Copy-Trading Reserve Pool (ReservePool)

```text
ReservePool := {                      // copy-trading reserve pool R_ct (E.4.3)
  balance    : u128                   // funds in pool
  open_loss  : u128                   // Σ MaxLoss(ρ), all unsettled rounds
  coverage   : Ratio                  // Ξ = balance / open_loss ≥ 150%
  index      : u128                   // rebasing share index (borrowed from E.1.2)
  state      : Normal | Throttled | Halted | Protection   // E.4.4
}
```

## State Proof (Proof)

```text
InclusionProof := {                   // inclusion proof (A.2.4 / B.3.4)
  key   : Key
  value : bytes
  path  : [ Node ]                    // Merkle path
}
// verify: Vrf_trie(state_root, key, value, path) → {0,1}

ExclusionProof := { key: Key, path: [Node] }   // non-existence proof
```

## Entry-Log Entry

```text
LogEntry := {
  seqNo : u64                         // global monotonic sequence number (B.4.1)
  tx    : Tx
  hash  : Hash                        // h_k = H(h_{k-1} ‖ seqNo ‖ tx) (B.4.2)
}
```

---

> **End of document.** This yellowpaper describes AXON Finance's proposed design; concrete implementation, parameters, and choices are subject to testnet, security review, and governance conclusions. Back to: [Yellowpaper Introduction](README.md) · [Whitepaper Home](../README.md)
