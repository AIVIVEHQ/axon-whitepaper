---
title: F.1 Gas 与费用市场
description: 支付级可预测费用模型
---

# F.1 Gas 与费用市场

> **设计状态**：proposed design。费用模型为设计方案，具体费率待测试网基准（[附录 II](appendix-parameters.md)）。本节讲协议级费用机制，不涉及代币供应/分配。

## F.1.1 Gas 计量

每笔交易消耗的资源以 **gas** 计量，涵盖三类成本：

$$\mathsf{gas}(\mathsf{tx}) = g_{\text{compute}} + g_{\text{state}} + g_{\text{data}}$$

* $g_{\text{compute}}$：执行/验证器/策略沙盒（[C.3](c3-policy-paymaster.md)）的指令成本；
* $g_{\text{state}}$：状态读写成本（新增账户/存储更贵，抑制状态膨胀，[B.3.5](b3-state.md)）；
* $g_{\text{data}}$：交易字节与带宽成本。

沙盒逐指令计量，超预算即中止（OOG，[C.3.1](c3-policy-paymaster.md)）——gas 是 DoS 的定价闸门。

## F.1.2 支付级费用模型

通用链多用**拍卖式**费用（EIP-1559 base fee 随拥堵浮动 + 小费竞价）。对支付这是致命的：一个商户无法接受「费用在 0.1 美分到 5 美元之间跳动」——它让定价、对账与商业模型全部失控（白皮书 [3.3](../part3-architecture/3-3-consensus-finality.md)）。

AXON 的核心取舍：**低且可预测 > 单纯更低**。设计方案（proposed）：

* **稳态固定费用**：正常负载下，支付/结算类交易采用**固定、可预测的费用**——`fee = gas × price_fixed`，$price_{\text{fixed}}$ 由治理设定并平滑调整，而非逐区块拍卖。费用应稳定如基础设施定价，而非随情绪波动的拍卖价。
* **拥堵定价作为保护阀**：仅当负载持续逼近容量上限，才启用类 EIP-1559 的拥堵附加费抑制滥用；正常支付流量下不触发。这既保可预测性，又留过载保护。

$$\text{fee}(\mathsf{tx}) = \mathsf{gas}(\mathsf{tx}) \times \big(price_{\text{fixed}} + \mathbb{1}[\text{congested}] \cdot price_{\text{surge}}(U_{\text{block}})\big)$$

设计目标：支付交易费用落在**亚美分级**（$< \$0.001$，白皮书性能设计目标），使按调用付费的 M2M 微支付（[C.2.7](c2-session-keys.md)）在经济上成立。

## F.1.3 费用的去向

协议手续费收入在链层沉淀，构成 AXON 的真实协议营收（白皮书 [3.1](../part3-architecture/3-1-why-own-l1.md) 的「价值在链层沉淀」）——网络可持续性来自真实业务量（TPV）产生的营收，而非通胀补贴。

> 费用收入如何在验证者、协议金库之间分配，以及是否与代币价值捕获关联，属**代币经济**范畴，本黄皮书与白皮书一致**不展开**，将另行成篇。本节仅规范费用的**计量与定价机制**。

## F.1.4 Paymaster 经济

Paymaster（[C.3.2](c3-policy-paymaster.md)）代付 gas 的经济安全：代付前锁定 `max_fee` 保证金、执行后按实付结算、余额退回，并可设单用户/周期配额抵御 gas 抽干。代付方的商业模型（应用补贴获客、商户承担收单成本等）由其自定，协议只保证**代付方始终可偿付、且不被任意白嫖**。

## F.1.5 与结算的协同

因稳定币结算是链层原语（[D.1](d1-settlement.md)），支付交易的 gas 路径短、状态访问模式可预测，$g_{\text{state}}$/$g_{\text{compute}}$ 波动小——这正是「固定可预测费用」能落地的技术前提。加上 Paymaster 让用户无需持有 gas 代币，支付体验可做到「像 Web2 一样顺滑」，同时保留链上确定性。

---

*下一节：[F.2 质押、验证者经济与罚没](f2-staking-slashing.md)*
