---
title: E.1 PayFi Money Market
description: Liquidity pools, interest-rate curves, and the health factor
---

# E.1 PayFi Money Market

> **Design status**: proposed design (a design model). Interest-rate parameters and collateral ratios are TBD, and this is **not a promise of returns**.

## E.1.1 Goal

Turn the in-transit funds, receivables, and float along the payment path into an on-chain money market, capturing the **time value of money** (whitepaper [4.2](../part4-payfi/4-2-money-market.md)/[4.4](../part4-payfi/4-4-time-value-of-money.md)). This section gives the design model of its protocol mechanics; the yield is rooted in real payment flows and receivables, not in the funds of new entrants.

## E.1.2 The Liquidity-Pool Model

A money market is an asset pool: liquidity providers (LPs, [A.1.1](a1-system-model.md)) deposit funds, and demand-side parties (payment merchants, factoring parties, holders of in-transit goods payments) borrow. For a given asset pool:

$$\text{Total supply } D = \sum_{\text{LP}} d_j, \qquad \text{Total borrowed } B, \qquad \text{Available liquidity } = D - B$$

**Utilization**:

$$U = \frac{B}{D} \in [0, 1]$$

LP shares are accounted for with an **indexed interest accrual (rebasing index)** to avoid per-account interest settlement: at deposit, record $\text{shares} = d / \mathrm{idx}_{\text{supply}}$, and at any time the value $= \text{shares} \times \mathrm{idx}_{\text{supply}}(t)$, where the index grows as interest accrues.

## E.1.3 The Interest-Rate Curve

The borrow rate is a **kinked function** of utilization—at low utilization the rate is gentle to encourage borrowing, and past the optimal point $U^*$ it rises steeply to protect liquidity and deter bank runs:

$$r_{\text{borrow}}(U) = \begin{cases} r_0 + \dfrac{U}{U^*}\, r_1, & U \leq U^* \\[2mm] r_0 + r_1 + \dfrac{U - U^*}{1 - U^*}\, r_2, & U > U^* \end{cases}$$

where $r_0$ is the base rate, $r_1$ the slope before the optimal point, and $r_2$ the steep rate past overload ($r_2 \gg r_1$). The supply rate is distributed to LPs by utilization after deducting the protocol reserve factor $\phi$ from the borrow interest:

$$r_{\text{supply}}(U) = U \cdot r_{\text{borrow}}(U) \cdot (1 - \phi)$$

The portion accrued by the reserve factor $\phi$ enters the risk reserve ([E.2](e2-liquidation.md)). All parameters are set by governance (whitepaper [6.2](../part6-roadmap/6-2-governance.md)).

## E.1.4 Collateral and Reputation Bond

There are two repayment sources for PayFi lending, which can be combined:

* **Cash-flow self-liquidating**: financing is tied to real receivables/payment flows, and at maturity is automatically repaid from receivables collection—this is the first line of defense (cash flow is the best collateral).
* **Collateral + reputation bond**: the demand-side party/participating node locks up collateral or a reputation bond, which is slashed upon misbehavior/default (echoing the bounded-authorization idea of [C.2](c2-session-keys.md) and the slashing of [F.2](f2-staking-slashing.md)).

## E.1.5 The Health Factor

For collateralized positions, define the **health factor** $H$ to measure the safety margin:

$$H = \frac{\sum_i C_i \cdot \lambda_i}{\text{Debt}}$$

where $C_i$ is the value of the $i$-th collateral item (priced via [D.2](d2-oracle.md)), $\lambda_i \in (0,1]$ is its liquidation threshold (the more volatile the collateral, the lower $\lambda$), and $\text{Debt}$ is the debt value.

$$H \geq 1 \Rightarrow \text{healthy}, \qquad H < 1 \Rightarrow \text{liquidatable (see E.2)}$$

Borrowing requires $H \geq H_{\text{open}} > 1$ to leave a safety buffer; $H$ changes in real time with price movements, and dropping below $1$ triggers liquidation. The copy-trading reserve pool's coverage invariant $\Xi \geq 150\%$ borrows the same idea (the reserve is to exposure as collateral is to debt, [E.4.3](e4-reserve-risk.md)).

## E.1.6 A Bit of Financial Background

This mechanism corresponds to traditional finance's **receivables financing / factoring** (whitepaper [4.2](../part4-payfi/4-2-money-market.md)): after a company ships goods, it waits 30–90 days for payment, and discounts its receivables to realize cash early; whoever provides funds for that "time" earns the time value of money (present value/discounting, whitepaper [4.4](../part4-payfi/4-4-time-value-of-money.md)). AXON brings this multi-trillion-dollar business—long monopolized by institutions—on-chain, pricing "time" transparently, composably, and auditably.

---

*Next: [E.2 Credit Risk Control & Liquidation](e2-liquidation.md)*
