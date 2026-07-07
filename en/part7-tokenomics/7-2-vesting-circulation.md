---
title: 7.2 Vesting & Circulating Supply
description: Each bucket's vesting and the 60-month circulating-release cadence
---

# 7.2 Vesting & Circulating Supply

## Vesting rules per bucket

At TGE only a very small portion is released; the rest unlocks per each bucket's independent lockup and linear-release rules:

| Bucket | Vesting rule |
| --- | --- |
| Ecosystem | TGE 3% + 60-month linear (governance-controlled) |
| Team & Advisors | 1-year lock + 3-year linear |
| Angel | 2-year lock + 3-year linear (5 years total) |
| Strategic (private) | 12-month cliff + 24-month linear |
| Airdrop | TGE 25% + quarterly linear |
| Liquidity / Market-Making | Released after a 6-month lock |

The core intent of this design is to **bind the interests of early capital and the team to the network's long-term maturation** — Angel at 5 years total, Team at 4 years (1-year lock + 3-year linear), Strategic at 3 years — all lockup periods markedly longer than industry convention.

## The 60-month circulating curve

TGE has a low float of **4%** (400M), after which release proceeds linearly per each bucket's vesting rules, reaching ≈ **100%** by month 60. Key milestones:

| Time | Cumulative circulating share |
| --- | --- |
| TGE (m0) | 4.0% |
| m6 | ≈ 12.6% |
| m12 | ≈ 29.2% |
| m24 | ≈ 47.9% |
| m36 | ≈ 69.9% |
| m48 | ≈ 87.0% |
| m60 | ≈ 100% |

Release-cadence visualization (▓ represents the share already in circulation):

```
TGE   ▏                    4.0%
m6    ▋                    12.6%
m12   ██▏                  29.2%
m24   ███▊                 47.9%
m36   █████▌               69.9%
m48   ██████▉              87.0%
m60   ████████             100%
```

The combination of **low initial float + long linear release** means the float on the market at TGE is very thin (4%), easing early sell pressure; while the full 5-year release period leaves the network a time window to cash "real business growth" into "token value" — which is exactly where the [7.3 Deflationary Flywheel](7-3-utility-flywheel.md) comes into play.

---

*Previous: [7.1 Supply & Allocation](7-1-supply-allocation.md) · Next: [7.3 Token Utility & the Deflationary Flywheel](7-3-utility-flywheel.md)*
