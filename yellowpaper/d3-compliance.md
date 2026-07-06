---
title: D.3 可插拔合规网关
description: 地基层的合规 hook 架构
---

# D.3 可插拔合规网关

> **设计状态**：proposed design。合规落地按加密友好辖区分阶段推进，具体牌照/服务商待定。

## D.3.1 为什么做进地基

支付基础设施绕不开合规（KYC/AML、制裁名单、地理管辖、Travel Rule）。问题不是「要不要合规」，而是「合规放在哪一层」。应用层各自实现 → 口径不一、可绕过；AXON 把合规做成**地基层的可插拔网关（pluggable gateway）**，位于五层架构最顶层（白皮书 [3.6](../part3-architecture/3-6-compliance-gateway.md)），是交易进入排序层（[B.4](b4-sequencing.md)）前的第一道门。

## D.3.2 Hook 架构

网关是一组在交易定序前执行的**合规钩子（hook）** 的可组合管线：

$$\mathsf{Screen}(\mathsf{tx}) = \bigwedge_{i} \mathsf{hook}_i(\mathsf{tx}, \mathsf{attest})\ \to\ \{\text{Pass}, \text{Reject}\}$$

```text
接入网关：
  tx 到达 → 加载适用策略集（按资产/场景/法域）
  for hook in policy.hooks:
      if hook.eval(tx, attestations) == REJECT:
          return Rejected(reason)     # 明确拦截（B.5.1）
  return Pass → 交排序层分配 seqNo
```

hook 作为链层挂载点，第三方合规服务商可按需接入；不同业务场景配置不同的 hook 组合。所有合规决策在统一入口发生，**可审计、可追溯、不被应用层绕过**。

## D.3.3 核心 hook

| Hook | 职责 |
| --- | --- |
| `Auth` | 验证发起方身份凭证 |
| `KYC/AML` | 对接第三方合规服务，按需身份与反洗钱检查 |
| `Sanctions` | 制裁名单筛查（地址/实体） |
| `Geofence` | 按法域限制特定功能/资产可用性 |
| `TravelRule` | 跨机构转账的发起方/受益方信息合规传递 |
| `RiskPreScreen` | 交易执行前的风险评估与拦截 |
| `RateLimit` | 防滥用与异常流量 |

## D.3.4 隐私保留的合规验证

合规不应以把用户 PII（个人身份信息）暴露上链为代价。AXON 的设计方向（proposed）：**链上只承载合规凭证（attestation），不承载原始 PII**。

* 合规服务商在链下完成 KYC，签发一份**认证（attestation）**：如「地址 $a$ 已通过 $L$ 级 KYC、非制裁实体、辖区 $J$」。
* 交易携带对该 attestation 的引用/证明；hook 验证 attestation 的签发方与有效性，而不接触原始身份数据。
* 更强隐私可用**零知识证明**：证明「满足合规谓词」（如「年满 18 且非制裁辖区」）而不泄露具体身份——合规与隐私兼得。

$$\mathsf{hook}(\mathsf{tx}) : \mathsf{Vrf}_{\text{attest}}\big(pk_{\text{issuer}},\ \text{claim},\ \pi\big) \to \{0,1\}$$

同一 attestation 机制也是带单引擎链下**结算结果证明**的格式底座（[E.3.5](e3-copy-trading.md)）：量化台对「本场结算结果」出具带签名的 claim，由 $\mathsf{Vrf}_{\text{attest}}$ 验证后方可分账。

## D.3.5 分阶段落地

合规能力随可插拔架构演进：

* **首发**：聚焦稳定币合规口径——KYC/AML + 地理围栏 + 加密友好辖区，服务相对清晰的稳定币支付/结算场景。
* **面向 TradFi 的扩展**（白皮书 P3+）：走更严格的合规路径，因传统金融资产监管门槛远高于稳定币支付。
* **随生态挂载**：进入新法域/新场景时，灵活挂载对应 hook。

把合规做进地基，不是为了「更严」，而是让**合规成为支付体验的一部分，而非事后的摩擦**——这是 PayFi 对接实体商业的前提。任何对外披露、募集或产品上线均以届时适用法域的合规审查结论为准。

---

*下一节：[E.1 PayFi 货币市场](e1-money-market.md)*
