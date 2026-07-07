---
title: D.3 Pluggable Compliance Gateway
description: A foundation-layer compliance hook architecture
---

# D.3 Pluggable Compliance Gateway

> **Design status**: proposed design. Compliance rollout proceeds in phases across crypto-friendly jurisdictions; specific licenses/providers are TBD.

## D.3.1 Why Build It Into the Foundation

Payment infrastructure cannot avoid compliance (KYC/AML, sanctions lists, geographic jurisdiction, Travel Rule). The question is not "whether to comply" but "at which layer compliance lives". Each application implementing it separately → inconsistent standards, bypassable; AXON makes compliance a **foundation-layer pluggable gateway**, sitting at the very top of the five-layer architecture (whitepaper [3.6](../part3-architecture/3-6-compliance-gateway.md)), the first gate before a transaction enters the sequencing layer ([B.4](b4-sequencing.md)).

## D.3.2 The Hook Architecture

The gateway is a composable pipeline of **compliance hooks** executed before transaction sequencing:

$$\mathsf{Screen}(\mathsf{tx}) = \bigwedge_{i} \mathsf{hook}_i(\mathsf{tx}, \mathsf{attest})\ \to\ \{\text{Pass}, \text{Reject}\}$$

```text
Entering the gateway:
  tx arrives → load applicable policy set (by asset/scenario/jurisdiction)
  for hook in policy.hooks:
      if hook.eval(tx, attestations) == REJECT:
          return Rejected(reason)     # explicit interception (B.5.1)
  return Pass → hand to sequencing layer to assign seqNo
```

As a chain-layer mount point, third-party compliance providers can integrate hooks on demand; different business scenarios configure different hook combinations. All compliance decisions happen at a unified entry point, **auditable, traceable, and not bypassable by the application layer**.

## D.3.3 Core Hooks

| Hook | Responsibility |
| --- | --- |
| `Auth` | Verify the initiator's identity credentials |
| `KYC/AML` | Integrate third-party compliance services for on-demand identity and anti-money-laundering checks |
| `Sanctions` | Sanctions-list screening (addresses/entities) |
| `Geofence` | Restrict availability of specific features/assets by jurisdiction |
| `TravelRule` | Compliant transmission of originator/beneficiary information for cross-institution transfers |
| `RiskPreScreen` | Risk assessment and interception before transaction execution |
| `RateLimit` | Abuse prevention and anomalous-traffic control |

## D.3.4 Privacy-Preserving Compliance Verification

Compliance should not come at the cost of exposing users' PII (personally identifiable information) on-chain. AXON's design direction (proposed): **the chain carries only compliance credentials (attestations), not raw PII**.

* The compliance provider completes KYC off-chain and issues an **attestation**: e.g. "address $a$ has passed level-$L$ KYC, is a non-sanctioned entity, jurisdiction $J$".
* The transaction carries a reference/proof to that attestation; the hook verifies the attestation's issuer and validity, without touching the raw identity data.
* Stronger privacy can use **zero-knowledge proofs**: proving "the compliance predicate is satisfied" (e.g. "over 18 and not in a sanctioned jurisdiction") without revealing the specific identity—compliance and privacy at once.

$$\mathsf{hook}(\mathsf{tx}) : \mathsf{Vrf}_{\text{attest}}\big(pk_{\text{issuer}},\ \text{claim},\ \pi\big) \to \{0,1\}$$

This same attestation mechanism is also the format foundation for the copy-trading engine's off-chain **settlement-result proof** ([E.3.5](e3-copy-trading.md)): the quant desk issues a signed claim about "this round's settlement result", which must be verified by $\mathsf{Vrf}_{\text{attest}}$ before proceeds are split.

## D.3.5 Phased Rollout

Compliance capabilities evolve alongside the pluggable architecture:

* **Initial launch**: focus on stablecoin compliance standards—KYC/AML + geofencing + crypto-friendly jurisdictions, serving the relatively clear-cut stablecoin payment/settlement scenarios.
* **TradFi-oriented expansion** (whitepaper P3+): follow a stricter compliance path, since traditional financial assets face far higher regulatory thresholds than stablecoin payments.
* **Mount as the ecosystem grows**: flexibly mount the corresponding hooks when entering a new jurisdiction/scenario.

Building compliance into the foundation is not about being "stricter", but about making **compliance part of the payment experience rather than an after-the-fact friction**—this is the prerequisite for PayFi to connect with real-world commerce. Any external disclosure, fundraising, or product launch is subject to the compliance-review conclusions of the applicable jurisdiction at the relevant time.

---

*Next: [E.1 PayFi Money Market](e1-money-market.md)*
