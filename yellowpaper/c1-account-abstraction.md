---
title: C.1 账户抽象与交易执行
description: 链层原生的可编程账户
---

# C.1 账户抽象与交易执行

> **设计状态**：proposed design。原生账户抽象，接口可随执行层设计调整。

## C.1.1 为什么原生，而非中继式

以太坊的账户抽象（ERC-4337）是**应用层**方案：靠链下 bundler、独立 mempool 与合约 EntryPoint 模拟，因为底层账户模型固化。AXON 从地基设计，把账户抽象做成**链层原生（native）**能力——无需中继者、无独立 mempool，账户抽象即协议本身。这对 AI 代理支付的「可控执行」是必要的（[C.2](c2-session-keys.md)）：授权约束必须由链**强制**，而非依赖链下诚实。

## C.1.2 可编程验证

传统账户 = 一把私钥，能做什么、如何授权全部写死。AXON 账户携带一个**验证器引用（`verifier`）**，将「什么是合法授权」变成账户可编程的谓词：

$$\mathsf{Verify} : (\mathsf{Account}, \mathsf{Tx}) \to \{0,1\}$$

* **默认验证器**：Ed25519 单签（[A.2.2](a2-cryptography.md)）——等价于普通外部账户。
* **自定义验证器**：账户可声明其它方案——多签门限、passkey/secp256r1（WebAuthn）、社交恢复、时间锁、或**会话密钥策略**（[C.2](c2-session-keys.md)）。

验证器逻辑在确定性沙盒（[C.3](c3-policy-paymaster.md)）中执行，其 gas 计入交易。

## C.1.3 交易与 UserOperation

AXON 统一的交易对象（原生，非二级 mempool）：

```text
Tx := {
  sender     : Addr
  nonce      : u64
  calls      : [ Call ]        // 一或多个调用（原生批量）
  auth       : AuthData        // 验证器所需的授权数据（签名/会话密钥证明/…）
  gas        : GasParams       // 上限、费用参数（F.1）
  paymaster  : PaymasterRef?   // 可选：费用代付（C.3）
}
Call := { target: Addr, asset: AssetId?, amount: u128?, data: bytes? }
```

相较普通链交易，AXON 的 `Tx` 原生支持：**批量调用**（一次授权多笔操作，原子执行）、**可插拔授权**（`auth` 由账户 `verifier` 解释）、**费用代付**（`paymaster`）。

## C.1.4 执行语义

交易执行严格遵循 [B.3.2](b3-state.md) 的 $\delta_{\mathsf{tx}}$，展开授权与执行两阶段：

```text
δ_tx(st, tx):
  # —— 验证阶段（Validation）——
  assert st[tx.sender].nonce == tx.nonce
  assert Verify(st[tx.sender], tx) == 1        # 可编程验证器
  assert Auth(st[tx.sender], tx) == 1          # 授权策略谓词（C.2）
  fee_payer ← resolve_paymaster(tx) or tx.sender
  assert can_afford(fee_payer, tx.gas)         # 费用可付（F.1）

  # —— 执行阶段（Execution）——
  for call in tx.calls:                        # 批量原子
      apply(st, call)                          # 转账/结算/合约调用
  charge_gas(fee_payer, actual_gas)
  st[tx.sender].nonce += 1
  return st'
```

**验证与执行分离**：验证阶段的任何失败使交易 `Rejected`（[B.5.1](b5-finality.md)），不进入执行；执行阶段的批量调用**原子**——全部成功或全部回滚，杜绝部分执行导致的资金不一致。

## C.1.5 账户类型

同一账户模型可表达一个能力谱系：

| 类型 | verifier | 典型用途 |
| --- | --- | --- |
| 基础账户 | Ed25519 单签 | 个人钱包 |
| 智能账户 | 多签 / passkey / 社交恢复 | 机构、可恢复钱包 |
| 会话账户 | 会话密钥策略（[C.2](c2-session-keys.md)） | 授权给 AI 代理、订阅、自动化 |
| 商户账户 | 收单策略 + 合规钩子 | 商户收单、结算 |

**关键**：这些不是不同的账户「种类」，而是同一可编程账户的不同 `verifier` 配置——统一模型，无特例。这为 [C.2](c2-session-keys.md) 的会话密钥授权提供了地基。

---

*下一节：[C.2 会话密钥与授权模型](c2-session-keys.md)*
