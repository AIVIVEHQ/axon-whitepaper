# Table of contents

* [AXON Finance 白皮书](README.md)

## Part I · 愿景与定位

* [引读：地基与切口](part1-vision/README.md)
  * [1.1 核心命题：L1 是地基，PayFi 是切口](part1-vision/1-1-thesis.md)
  * [1.2 AXON Finance 是什么：双层叙事](part1-vision/1-2-what-is-axon.md)
  * [1.3 设计哲学与第一性原理](part1-vision/1-3-design-principles.md)

## Part II · 市场与机会

* [引读：为什么是现在](part2-market/README.md)
  * [2.1 稳定币成为全球支付层](part2-market/2-1-stablecoin-era.md)
  * [2.2 PayFi 的诞生与货币时间价值](part2-market/2-2-payfi-thesis.md)
  * [2.3 跨境支付的结构性痛点](part2-market/2-3-crossborder-pain.md)
  * [2.4 AI 代理经济的兴起](part2-market/2-4-ai-agent-economy.md)
  * [2.5 竞争格局](part2-market/2-5-competitive-landscape.md)
  * [2.6 白空间与市场规模](part2-market/2-6-whitespace.md)

## Part III · 技术架构

* [引读：把确定性做进地基](part3-architecture/README.md)
  * [3.1 为什么必须自有 L1](part3-architecture/3-1-why-own-l1.md)
  * [3.2 五层架构总览](part3-architecture/3-2-layered-architecture.md)
  * [3.3 共识、亚秒最终性与性能目标](part3-architecture/3-3-consensus-finality.md)
  * [3.4 支付最终性与反双花](part3-architecture/3-4-payment-finality.md)
  * [3.5 稳定币与喂价：多源校验与熔断](part3-architecture/3-5-oracle-safety.md)
  * [3.6 可插拔合规网关](part3-architecture/3-6-compliance-gateway.md)
  * [3.7 账户抽象、会话密钥与 Paymaster](part3-architecture/3-7-account-abstraction.md)

## Part IV · PayFi 引擎

* [引读：四场景层层叠加价值](part4-payfi/README.md)
  * [4.1 稳定币即时结算 Rail](part4-payfi/4-1-settlement-rail.md)
  * [4.2 PayFi 货币市场：浮存金与链上信贷](part4-payfi/4-2-money-market.md)
  * [4.3 跨境 B2B 与商户收单](part4-payfi/4-3-crossborder-b2b.md)
  * [4.4 货币时间价值的金融学](part4-payfi/4-4-time-value-of-money.md)
  * [4.5 美股带单引擎：首个开放生态](part4-payfi/4-5-copy-trading-engine.md)

## Part V · AI 原生

* [引读：让 AI 安全地接进支付](part5-ai/README.md)
  * [5.1 Agentic Payments 的真问题：授权与安全](part5-ai/5-1-agentic-payments.md)
  * [5.2 可控支付执行](part5-ai/5-2-controlled-execution.md)
  * [5.3 x402 与 M2M 微支付](part5-ai/5-3-x402-m2m.md)
  * [5.4 诚实的 AI 定位](part5-ai/5-4-honest-ai.md)

## Part VI · 路线图与治理

* [引读：每阶段都有退出价值](part6-roadmap/README.md)
  * [6.1 路线图 P0 → P3+](part6-roadmap/6-1-roadmap.md)
  * [6.2 治理框架](part6-roadmap/6-2-governance.md)
  * [6.3 团队与资源网络](part6-roadmap/6-3-team-partners.md)
  * [6.4 生态启动与第一赛季](part6-roadmap/6-4-ecosystem-season.md)

## Part VII · 代币经济

* [引读：价值从哪里来](part7-tokenomics/README.md)
  * [7.1 供应与分配](part7-tokenomics/7-1-supply-allocation.md)
  * [7.2 解锁与流通曲线](part7-tokenomics/7-2-vesting-circulation.md)
  * [7.3 代币效用与通缩飞轮](part7-tokenomics/7-3-utility-flywheel.md)

## 附录

* [附录导航](appendix/README.md)
  * [附录 A · 术语表](appendix/a-glossary.md)
  * [附录 B · 参数汇总](appendix/b-parameters.md)
  * [附录 C · 品牌与商标](appendix/c-brand.md)
  * [附录 D · 参考与数据来源](appendix/d-references.md)

## 技术黄皮书 · Technical Yellowpaper

* [黄皮书导言](yellowpaper/README.md)

## 黄皮书 A · 系统模型与密码学

* [A.1 系统模型、假设与安全目标](yellowpaper/a1-system-model.md)
* [A.2 密码学基础与承诺](yellowpaper/a2-cryptography.md)

## 黄皮书 B · 共识与账本

* [B.1 BFT PoS 共识协议](yellowpaper/b1-consensus.md)
* [B.2 验证者集合与区块生产](yellowpaper/b2-validators.md)
* [B.3 状态模型与状态承诺](yellowpaper/b3-state.md)
* [B.4 排序层、Entry-Log 与确定性执行](yellowpaper/b4-sequencing.md)
* [B.5 支付最终性、反双花与恢复](yellowpaper/b5-finality.md)

## 黄皮书 C · 账户抽象与 AI 授权

* [C.1 账户抽象与交易执行](yellowpaper/c1-account-abstraction.md)
* [C.2 会话密钥与授权模型](yellowpaper/c2-session-keys.md)
* [C.3 策略沙盒与 Paymaster](yellowpaper/c3-policy-paymaster.md)

## 黄皮书 D · 结算 · 喂价 · 合规

* [D.1 稳定币结算引擎](yellowpaper/d1-settlement.md)
* [D.2 预言机与喂价安全](yellowpaper/d2-oracle.md)
* [D.3 可插拔合规网关](yellowpaper/d3-compliance.md)

## 黄皮书 E · PayFi 协议机制

* [E.1 PayFi 货币市场](yellowpaper/e1-money-market.md)
* [E.2 信贷风控与清算](yellowpaper/e2-liquidation.md)
* [E.3 美股带单引擎](yellowpaper/e3-copy-trading.md)
* [E.4 带单准备金与风控](yellowpaper/e4-reserve-risk.md)

## 黄皮书 F · 协议经济与安全

* [F.1 Gas 与费用市场](yellowpaper/f1-gas-fees.md)
* [F.2 质押、验证者经济与罚没](yellowpaper/f2-staking-slashing.md)
* [F.3 安全模型与威胁分析](yellowpaper/f3-security.md)

## 黄皮书 G · 网络与性能

* [G.1 网络层与交易传播](yellowpaper/g1-networking.md)
* [G.2 性能模型与设计目标](yellowpaper/g2-performance.md)

## 黄皮书 · 附录

* [附录 I · 符号表](yellowpaper/appendix-notation.md)
* [附录 II · 协议参数表](yellowpaper/appendix-parameters.md)
* [附录 III · 核心数据结构](yellowpaper/appendix-datastructures.md)
