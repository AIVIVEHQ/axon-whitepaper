---
title: A.2 Cryptographic Primitives & Commitments
description: Hashing, signatures, aggregation, and state commitments
---

# A.2 Cryptographic Primitives & Commitments

> **Design status**: proposed design. The cryptographic primitives are standard choices and may be adjusted following security review.

This section defines the cryptographic primitives used throughout the text. All primitives are standard, well-analyzed constructions; AXON introduces no home-grown cryptography.

## A.2.1 Hash Function

Let the collision-resistant hash function be

$$H : \{0,1\}^* \to \{0,1\}^{256}$$

AXON adopts **BLAKE3** as its primary hash (high-throughput, parallelizable, with a tree structure that naturally fits Merkle commitments), and retains a **Keccak-256** compatibility path for scenarios that need interoperability with the external EVM ecosystem. Hashing is used for: transaction/block identifiers, Merkle commitments, and random-beacon derivation.

## A.2.2 Transaction Signatures — Ed25519

Client transactions use **Ed25519** (EdDSA over Curve25519). Compared with ECDSA, Ed25519 offers deterministic signatures (no need for a secure random source), fast verification, and no signature malleability — the last point being crucial for double-spend prevention: a transaction hash does not change due to signature re-encoding.

The signature scheme $\Sigma = (\mathsf{KeyGen}, \mathsf{Sign}, \mathsf{Vrf})$:

$$(pk, sk) \gets \mathsf{KeyGen}(1^\lambda), \quad \sigma \gets \mathsf{Sign}(sk, m), \quad \{0,1\} \gets \mathsf{Vrf}(pk, m, \sigma)$$

Account abstraction ([C.1](c1-account-abstraction.md)) allows an account to customize its verification logic; Ed25519 is the default scheme, but an account may declare other verifiers (e.g., multisig, passkey/secp256r1, threshold signatures).

## A.2.3 Consensus Vote Aggregation — BLS12-381

Validator votes at the consensus layer use **BLS signatures (over the BLS12-381 curve)**, whose core advantage is **signature aggregation**: the signatures of $n$ validators on the same message can be aggregated into a single constant-size signature, verifiable in one operation.

Let $\sigma_i = \mathsf{Sign}_{\mathsf{BLS}}(sk_i, m)$; aggregation:

$$\sigma_{\mathsf{agg}} = \prod_{i \in Q} \sigma_i, \qquad \mathsf{VrfAgg}\big(\{pk_i\}_{i\in Q},\, m,\, \sigma_{\mathsf{agg}}\big) \to \{0,1\}$$

This lets a **quorum certificate (QC)** — votes on a block by more than $\tfrac{2}{3}S$ of stake — be compressed into an "aggregate signature + participant bitmap," of a size independent of the number of validators. The QC is the core credential of consensus ([B.1](b1-consensus.md)); aggregate signatures let a block header efficiently carry a finality proof.

## A.2.4 State Commitment — Merkleized State Tree

The global state $\mathsf{st}$ is committed as a **Merkleized authenticated key-value tree**, whose root $\mathsf{root}(\mathsf{st})$ (32 bytes) is written into the block header. The default structure is an optimized variant of the **Merkle Patricia Trie (MPT)**, designed to be **Verkle-ready** — retaining the interface for a future migration to a Verkle Tree (based on vector commitments, with dramatically smaller proof sizes).

The commitment provides two kinds of proof:

* **Inclusion proof**: proves $(k, v) \in \mathsf{st}$, i.e. $\exists\,\pi:\ \mathsf{Vrf}_{\mathsf{trie}}(\mathsf{root}, k, v, \pi) = 1$.
* **Exclusion proof**: proves $k \notin \mathsf{st}$.

Light clients and cross-chain bridges can verify any account balance or payment result from the state root + a proof, without trusting a full node — this is the technical meaning of "provable state root" ([B.3](b3-state.md)).

## A.2.5 Verifiable Randomness — VRF

Leader election and anti-predictability ordering require **non-manipulable randomness**. AXON adopts a **VRF (Verifiable Random Function)**:

$$(y, \pi) \gets \mathsf{VRF}_{sk}(x), \qquad \mathsf{VrfProof}(pk, x, y, \pi) \to \{0,1\}$$

The output $y$ is unpredictable to anyone who does not know $sk$, yet anyone can use $\pi$ to verify that $y$ is uniquely determined by $x$ and $pk$. Use: each round derives the leader from $x = (\text{epoch}, \text{round})$ ([B.2](b2-validators.md)), making the leader sequence impossible to manipulate in advance and compressing the room for censorship and MEV.

## A.2.6 Summary of Primitives

| Primitive | Scheme | Use |
| --- | --- | --- |
| Hash $H$ | BLAKE3 (Keccak-256 compatibility path) | Identifiers, commitments, random derivation |
| Transaction signature | Ed25519 (account-customizable verifier) | Transaction authorization |
| Consensus signature | BLS12-381 aggregation | Quorum Certificate |
| State commitment | Merkle tree (Verkle-ready) | State root, inclusion/exclusion proofs |
| Randomness | VRF | Leader election, anti-manipulation ordering |

The final choice of specific curves, hashes, and parameters is subject to the conclusions of the security review ([Appendix II](appendix-parameters.md)).

---

*Next: [B.1 BFT PoS Consensus Protocol](b1-consensus.md)*
