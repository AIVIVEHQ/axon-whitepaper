---
title: Appendix A · Glossary
description: Unified definitions of the key terms used throughout the book
---

# Appendix A · Glossary

Organized by topic, for cross-reference throughout the book.

## PayFi & Finance

**PayFi (Payment Finance)**
On-chain finance built around real payment flows. Its core idea is to bring the "time value of money" on-chain — not trading crypto assets, but putting on-chain how real-world money actually moves, settles, and earns yield. Coined by Lily Liu of the Solana Foundation in 2024. See [2.2](../part2-market/2-2-payfi-thesis.md).

**Time Value of Money (TVM)**
A first principle of finance: the same sum of money is more valuable the earlier you hold it, because it can immediately be put to work generating returns. Interest, discounting, and financing are all, at bottom, ways of pricing "time." See [4.4](../part4-payfi/4-4-time-value-of-money.md).

**Float**
Funds "already in hand, but not yet cleared, and not yet yours" — money in transit. Float exists while a payment is in flight, before a credit-card settlement, and between a prepayment and delivery. Its time value is an important yield source for the PayFi money market.

**Factoring / Receivables Financing**
A financing method in which a business converts receivables it will only collect in the future into cash today, at a discount. It is the on-chain counterpart within the PayFi money market.

**Present Value / Future Value**
Future value $FV = PV(1+r)^n$; present value $PV = FV/(1+r)^n$. The core formulas for discounting future money back to today (or vice versa).

**Nostro / Vostro accounts**
The accounts banks hold with one another in the correspondent-banking system ("ours held with you" / "yours held with us"). The root cause of multi-hop clearing and pre-funded capital lockup in cross-border payments. See [2.3](../part2-market/2-3-crossborder-pain.md).

**TAM / SAM / SOM**
Total Addressable Market / Serviceable Addressable Market / Serviceable Obtainable Market — a three-tier framework for sizing a market. See [2.6](../part2-market/2-6-whitespace.md).

## Blockchain & Consensus

**L1 (Layer-1)**
A self-contained base blockchain with its own consensus and settlement layer, rather than one built atop another chain.

**Finality**
The degree to which a transaction is "irreversible." **Probabilistic finality** approaches, but never reaches, certainty as confirmations accumulate; **deterministic / BFT finality** becomes irreversible immediately once confirmed by a quorum. AXON adopts the latter. See [3.3](../part3-architecture/3-3-consensus-finality.md).

**PoS (Proof of Stake)**
A mechanism in which validators participate in consensus by staking. AXON uses high-throughput PoS + BFT-style deterministic finality.

**Sequencer**
The role responsible for ordering transactions. AXON requires the Sequencer, even if centralized, to prove that its log is tamper-evident and can be fully replayed. See [3.4](../part3-architecture/3-4-payment-finality.md).

**Entry-Log / Write-Ahead Log (WAL)**
An append-only, tamper-evident log written before a transaction is executed, which can be fully replayed to reconstruct state. The foundation for "always traceable, always recoverable when something goes wrong" in payments.

**State Root**
A provable digest of the chain's global state, used to verify state correctness.

**TPS (Transactions Per Second)**
The number of transactions processed per second, a measure of throughput.

## AI & Accounts

**Account Abstraction**
Making the account itself programmable — able to define its own verification logic, authorization rules, and recovery mechanisms, rather than "one private key decides life and death." See [3.7](../part3-architecture/3-7-account-abstraction.md).

**Session Keys**
Temporary keys issued by a primary account — bounded and revocable — that can be constrained by limit / time window / allowlist / frequency. The key that safely connects AI agents to payments. See [5.2](../part5-ai/5-2-controlled-execution.md).

**Paymaster (fee sponsorship)**
Allows a third party to sponsor a transaction's gas, so users / agents can pay without holding the native gas token. See [3.7](../part3-architecture/3-7-account-abstraction.md).

**Controlled Payment Execution**
AXON's positioning for AI payments: account abstraction + session keys + verifiable policies + bounded authorization, so that AI "can pay, but can't run off and can't overspend." See [5.2](../part5-ai/5-2-controlled-execution.md).

**Agentic Payments**
Payments initiated autonomously by AI agents. The real challenge is authorization and security, not the payment capability itself. See [5.1](../part5-ai/5-1-agentic-payments.md).

**M2M (Machine-to-Machine) micropayments**
High-frequency, low-value, automatic payments where both payer and payee are software. See [5.3](../part5-ai/5-3-x402-m2m.md).

**x402**
A protocol that revives the HTTP status code `402 Payment Required`, realizing a machine-native payment interface for "pay per call." See [5.3](../part5-ai/5-3-x402-m2m.md).

**AP2 (and other agentic-payment authorization standards)**
The AI-agent payment-authorization standards that payment and tech giants are racing to define — at heart, a contest over the payment entry point and trust boundary of the machine economy. See [5.1](../part5-ai/5-1-agentic-payments.md).

## Compliance & Operations

**KYC / AML**
Know Your Customer / Anti-Money Laundering, the core requirements of payment compliance. AXON mounts them pluggably at the access layer. See [3.6](../part3-architecture/3-6-compliance-gateway.md).

**Geofencing**
A compliance measure that restricts the availability of specific features / assets by jurisdiction.

**TPV (Total Payment Volume)**
The total value of payments carried by the network, a core metric for real business volume.

**Design Target**
This whitepaper's label for performance metrics, meaning "a design and engineering target, not a currently achieved measured value"; final values will be published after testnet benchmarking. See [Appendix B](b-parameters.md).
