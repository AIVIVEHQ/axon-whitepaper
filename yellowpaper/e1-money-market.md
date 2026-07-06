---
title: E.1 PayFi 货币市场
description: 资金池、利率曲线与健康因子
---

# E.1 PayFi 货币市场

> **设计状态**：proposed design（设计模型）。利率参数、抵押率均待定，**不构成收益承诺**。

## E.1.1 目标

把支付链路里的在途资金、应收账款、浮存金做成链上货币市场，捕获**货币的时间价值**（白皮书 [4.2](../part4-payfi/4-2-money-market.md)/[4.4](../part4-payfi/4-4-time-value-of-money.md)）。本节给出其协议机制的设计模型；收益扎根于真实支付流与应收账款，而非新入场者资金。

## E.1.2 资金池模型

一个货币市场是一个资产池：流动性提供方（LP，[A.1.1](a1-system-model.md)）存入资金，资金需求方（支付商户、保理方、在途货款持有人）借出。设某资产池：

$$\text{总供给 } D = \sum_{\text{LP}} d_j, \qquad \text{总借出 } B, \qquad \text{可用流动性 } = D - B$$

**利用率（Utilization）**：

$$U = \frac{B}{D} \in [0, 1]$$

LP 的份额用**指数化利率累积**记账（rebasing index）避免逐账户结息：存入时记 $\text{shares} = d / \mathrm{idx}_{\text{supply}}$，任意时刻价值 $= \text{shares} \times \mathrm{idx}_{\text{supply}}(t)$，其中指数随利息累积增长。

## E.1.3 利率曲线

借款利率是利用率的**分段（kinked）函数**——利用率低时利率平缓鼓励借贷，越过最优点 $U^*$ 后陡升以保护流动性、抑制挤兑：

$$r_{\text{borrow}}(U) = \begin{cases} r_0 + \dfrac{U}{U^*}\, r_1, & U \leq U^* \\[2mm] r_0 + r_1 + \dfrac{U - U^*}{1 - U^*}\, r_2, & U > U^* \end{cases}$$

其中 $r_0$ 基础利率、$r_1$ 最优点前斜率、$r_2$ 超载后陡率（$r_2 \gg r_1$）。存款利率由借款利息扣除协议准备金因子 $\phi$ 后按利用率分配给 LP：

$$r_{\text{supply}}(U) = U \cdot r_{\text{borrow}}(U) \cdot (1 - \phi)$$

准备金因子 $\phi$ 计提的部分进入风险准备金（[E.2](e2-liquidation.md)）。所有参数由治理设定（白皮书 [6.2](../part6-roadmap/6-2-governance.md)）。

## E.1.4 抵押与信誉押金

PayFi 借贷的还款来源有两类，可组合：

* **现金流自偿**：融资绑定真实应收/支付流，到期由应收账款回款自动偿还——这是第一道防线（现金流是最好的抵押）。
* **抵押 + 信誉押金**：资金需求方/参与节点锁定抵押物或信誉押金，作恶/违约则罚没（呼应 [C.2](c2-session-keys.md) 的有界授权思路、[F.2](f2-staking-slashing.md) 的 slashing）。

## E.1.5 健康因子

对有抵押的仓位，定义**健康因子（Health Factor）** $H$ 衡量其安全边际：

$$H = \frac{\sum_i C_i \cdot \lambda_i}{\text{Debt}}$$

其中 $C_i$ 为第 $i$ 项抵押物的价值（经 [D.2](d2-oracle.md) 喂价）、$\lambda_i \in (0,1]$ 为其清算阈值（liquidation threshold，越波动的抵押物 $\lambda$ 越低）、$\text{Debt}$ 为债务价值。

$$H \geq 1 \Rightarrow \text{健康}, \qquad H < 1 \Rightarrow \text{可清算（见 E.2）}$$

借款时要求 $H \geq H_{\text{open}} > 1$ 留出安全缓冲；随喂价波动 $H$ 实时变化，跌破 $1$ 触发清算。带单准备金池的覆盖率不变式 $\Xi \geq 150\%$ 借用同一思想（准备金之于敞口，正如抵押之于债务，[E.4.3](e4-reserve-risk.md)）。

## E.1.6 一点金融背景

这套机制对应传统金融的**应收账款融资/保理**（白皮书 [4.2](../part4-payfi/4-2-money-market.md)）：企业发货后等 30–90 天回款，把应收账款折价提前变现；为这段「时间」提供资金者赚取货币时间价值（现值/贴现，白皮书 [4.4](../part4-payfi/4-4-time-value-of-money.md)）。AXON 把这门数万亿美元、长期被机构垄断的生意搬上链——透明、可组合、可审计地为「时间」定价。

---

*下一节：[E.2 信贷风控与清算](e2-liquidation.md)*
