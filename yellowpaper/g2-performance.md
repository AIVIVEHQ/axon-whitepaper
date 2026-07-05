---
title: G.2 性能模型与设计目标
description: 吞吐/延迟分析与目标推导
---

# G.2 性能模型与设计目标

> **设计状态**：性能指标为 `设计目标(target)`，**非已实现的实测值**；最终值以测试网基准为准。本节给出目标的推导逻辑，而非承诺。

## G.2.1 三个性能维度

支付级 SLA（白皮书 [3.3](../part3-architecture/3-3-consensus-finality.md)）由三个量刻画：

$$\text{吞吐 } \Theta\ (\mathrm{TPS}), \qquad \text{最终性延迟 } T_{\text{final}}\ (\mathrm{s}), \qquad \text{单笔费用 } \text{fee}\ (\$)$$

设计目标：$\Theta > 10{,}000$、$T_{\text{final}} < 1\mathrm{s}$、$\text{fee} < \$0.001$。以下给出各自的推导与瓶颈分析。

## G.2.2 最终性延迟

由 [B.1.7](b1-consensus.md)，最终性延迟为提交所需连续 QC 数 $k$ 乘以每轮开销：

$$T_{\text{final}} \approx k \cdot (\delta_{\text{net}} + \delta_{\text{agg}} + \delta_{\text{exec}})$$

以流水线深度 $k = 3$、区块间隔目标 $\Delta_{\text{block}} \in [250, 500]\,\mathrm{ms}$ 计，且 $\delta_{\text{agg}}$（BLS 聚合验证）、$\delta_{\text{exec}}$（执行）在区块间隔内被流水线吸收，则

$$T_{\text{final}} \approx k \cdot \Delta_{\text{block}} \in [0.75, 1.5]\,\mathrm{s} \ \Rightarrow\ \text{目标亚秒级}$$

优化方向：减小 $k$（更激进的提交规则）、缩短 $\Delta_{\text{block}}$（更快聚合/更近验证者拓扑）。BLS 聚合让 QC 验证与 $n$ 无关（[A.2.3](a2-cryptography.md)），是延迟不随验证者增多而恶化的关键。

## G.2.3 吞吐

单位时间吞吐 = 单区块交易数 / 区块间隔：

$$\Theta = \frac{N_{\text{block}}}{\Delta_{\text{block}}}, \qquad N_{\text{block}} = \min\!\left(\frac{B_{\text{net}}}{\bar{s}_{\text{tx}}},\ \frac{C_{\text{exec}}\cdot\Delta_{\text{block}}}{\bar{g}_{\text{tx}}}\right)$$

单区块交易数 $N_{\text{block}}$ 受两个上界约束（取小）：

* **网络界**：带宽 $B_{\text{net}}$ / 平均交易大小 $\bar{s}_{\text{tx}}$——由 [G.1.3](g1-networking.md) 的压缩区块/纠删码放宽。
* **执行界**：执行吞吐 $C_{\text{exec}}$（gas/s）× 区块间隔 / 平均交易 gas $\bar{g}_{\text{tx}}$。

要达 $\Theta > 10{,}000\,\mathrm{TPS}$，两界都须充分。三个杠杆：

1. **并行执行**：支付交易多为不相交账户对（无状态冲突），可乐观并行执行 + 冲突检测重试——把 $C_{\text{exec}}$ 提升近核数倍。
2. **短区块间隔**：$\Delta_{\text{block}}$ 越小，吞吐与延迟同步改善（受网络下界约束）。
3. **精简执行**：稳定币结算是链层原语（[D.1](d1-settlement.md)），支付路径短、$\bar{g}_{\text{tx}}$ 小，同带宽/算力下容纳更多交易。

## G.2.4 费用

由 [F.1.2](f1-gas-fees.md)，支付交易费用 $= \bar{g}_{\text{tx}} \times price_{\text{fixed}}$。因结算原语使 $\bar{g}_{\text{tx}}$ 低且稳定、且稳态采用固定费率（无拍卖溢价），费用可压至**亚美分级**并**可预测**——这是 M2M 微支付（[C.2.7](c2-session-keys.md)）经济成立的前提。**可预测性本身即是特性**，不只是数值低。

## G.2.5 瓶颈与权衡小结

| 维度 | 主瓶颈 | 主要杠杆 |
| --- | --- | --- |
| 最终性延迟 | $k \cdot \Delta_{\text{block}}$ | BLS 聚合、验证者拓扑、提交规则 |
| 吞吐 | min(网络界, 执行界) | 并行执行、压缩区块、精简支付路径 |
| 费用 | $\bar{g}_{\text{tx}}$、费率机制 | 原语化结算、固定费率 |

## G.2.6 诚实声明

> 本节数字均为 **`设计目标(target)`**，代表工程要达成的目标区间与其推导逻辑，**非当前已实现的实测值**。真实性能受硬件、网络拓扑、负载特征、并行化效果等多重因素影响，最终实测值将在**测试网基准测试**后公布。我们不把设计目标当作已实现的成绩来陈述。参数汇总见 [附录 II](appendix-parameters.md)。

---

*下一节：[附录 I · 符号表](appendix-notation.md)*
