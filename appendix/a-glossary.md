---
title: 附录 A · 术语表
description: 全书关键术语的统一定义
---

# 附录 A · 术语表

按主题组织，供全书回指。

## PayFi 与金融

**PayFi（Payment Finance，支付金融）**
围绕真实支付流构建的链上金融。核心是把「货币的时间价值」搬上链——不是交易加密资产，而是把现实世界的钱如何流动、结算与生息搬上链。由 Solana 基金会 Lily Liu 于 2024 年提出。见 [2.2](../part2-market/2-2-payfi-thesis.md)。

**货币时间价值（Time Value of Money, TVM）**
金融学第一性原理：同一笔钱，越早拥有越有价值，因为它可以立即投入使用产生收益。利息、贴现、融资本质上都是在为「时间」定价。见 [4.4](../part4-payfi/4-4-time-value-of-money.md)。

**浮存金（Float）**
「已经在手里、但尚未清算、暂不属于你」的在途资金。支付在途、信用卡结算前、预付款到交付前都存在浮存金。其时间价值是 PayFi 货币市场的重要收益来源。

**保理 / 应收账款融资（Factoring / Receivables Financing）**
企业把未来才能收到的应收账款，以一定折价提前变现的融资方式。PayFi 货币市场的链上版本。

**现值 / 终值（Present Value / Future Value）**
终值 $FV = PV(1+r)^n$；现值 $PV = FV/(1+r)^n$。把未来的钱折算到今天（或反之）的核心公式。

**Nostro / Vostro 账户**
代理行体系中银行互设的账户（「我们在你那里的」/「你们在我们这里的」）。跨境支付多跳清算与预筹资金占用的根源。见 [2.3](../part2-market/2-3-crossborder-pain.md)。

**TAM / SAM / SOM**
总可及市场 / 可服务市场 / 可获取市场，衡量市场规模的三层框架。见 [2.6](../part2-market/2-6-whitespace.md)。

## 区块链与共识

**L1（Layer-1）**
自有的基础区块链，拥有独立的共识与结算层，而非建立在其它链之上。

**最终性（Finality）**
交易「不可逆」的程度。**概率最终性**随确认累积无限逼近但不归零；**确定性 / BFT 最终性**一经法定多数确认即立即不可逆。AXON 采用后者。见 [3.3](../part3-architecture/3-3-consensus-finality.md)。

**PoS（Proof of Stake，权益证明）**
验证者通过质押参与共识的机制。AXON 采用高吞吐 PoS + BFT 风格确定性最终性。

**Sequencer（排序器）**
负责给交易定序的角色。AXON 要求 Sequencer 即使中心化也须自证日志不可篡改、可完整重放。见 [3.4](../part3-architecture/3-4-payment-finality.md)。

**Entry-Log / 预写日志（Write-Ahead Log, WAL）**
交易执行前先写入的只追加、不可篡改日志，可被完整重放以重建状态。支付「出错必可查、可恢复」的基础。

**状态根（State Root）**
链上全局状态的可证明摘要，用于验证状态的正确性。

**TPS（Transactions Per Second）**
每秒交易处理量，衡量吞吐的指标。

## AI 与账户

**账户抽象（Account Abstraction）**
让账户本身可编程——可定义自己的验证逻辑、授权规则、恢复机制，而非「一把私钥定生死」。见 [3.7](../part3-architecture/3-7-account-abstraction.md)。

**会话密钥（Session Keys）**
主账户签发的、有界的、可撤销的临时密钥，可设定限额 / 限时 / 白名单 / 频率约束。把 AI 代理安全接进支付的钥匙。见 [5.2](../part5-ai/5-2-controlled-execution.md)。

**Paymaster（费用代付）**
允许第三方代付交易 gas，用户 / 代理无需持有原生 gas 代币即可支付。见 [3.7](../part3-architecture/3-7-account-abstraction.md)。

**可控支付执行（Controlled Payment Execution）**
AXON 对 AI 支付的定位：账户抽象 + 会话密钥 + 可验证策略 + 限额授权，让 AI「能付钱、跑不了路、超不了额」。见 [5.2](../part5-ai/5-2-controlled-execution.md)。

**Agentic Payments（代理支付）**
由 AI 代理自主发起的支付。真正的难点是授权与安全，而非支付能力本身。见 [5.1](../part5-ai/5-1-agentic-payments.md)。

**M2M（Machine-to-Machine，机器对机器）微支付**
付款方与收款方都是软件的高频、微额、自动支付。见 [5.3](../part5-ai/5-3-x402-m2m.md)。

**x402**
让 HTTP 状态码 `402 Payment Required` 复活的协议，实现「服务按调用付费」的机器原生支付接口。见 [5.3](../part5-ai/5-3-x402-m2m.md)。

**AP2（等 agentic payment 授权标准）**
支付与科技巨头竞相定义的 AI 代理支付授权标准，本质是争夺机器经济的支付入口与信任边界。见 [5.1](../part5-ai/5-1-agentic-payments.md)。

## 合规与运营

**KYC / AML**
了解你的客户（Know Your Customer）/ 反洗钱（Anti-Money Laundering），支付合规的核心要求。AXON 在接入层可插拔挂载。见 [3.6](../part3-architecture/3-6-compliance-gateway.md)。

**地理围栏（Geofencing）**
按法域限制特定功能 / 资产可用性的合规手段。

**TPV（Total Payment Volume，总支付量）**
网络承载的支付总额，衡量真实业务量的核心指标。

**Design Target（设计目标）**
本白皮书对性能指标的标注，表示「设计与工程目标，非当前已实现的实测值」，最终值待测试网基准测试后公布。见 [附录 B](b-parameters.md)。
