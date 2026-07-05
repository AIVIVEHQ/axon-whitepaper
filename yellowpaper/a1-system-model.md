---
title: A.1 系统模型、假设与安全目标
description: 网络模型、故障模型与安全性质
---

# A.1 系统模型、假设与安全目标

> **设计状态**：proposed design。本节确立全文的形式化前提。

## A.1.1 参与者

AXON 网络由以下角色构成。设参与者全集为 $\mathcal{P}$。

| 角色 | 记号 | 职责 |
| --- | --- | --- |
| 验证者 | $\mathcal{V} = \{v_1,\dots,v_n\}$ | 参与共识、生产与验证区块、维护状态 |
| 排序器 | $\mathsf{Seq}$ | 为交易分配全局序号、写入 Entry-Log（早期中心化，可验证；见 [B.4](b4-sequencing.md)） |
| 客户端 | $\mathcal{C}$ | 提交交易的用户、钱包、商户 |
| AI 代理 | $\mathcal{A} \subseteq \mathcal{C}$ | 持有受限会话密钥、代表用户发起支付的自动化主体（见 [C.2](c2-session-keys.md)） |
| 流动性提供方 | $\mathcal{L}$ | 向 PayFi 货币市场注入资金（见 [E.1](e1-money-market.md)） |
| 喂价节点 | $\mathcal{O}$ | 提供法币锚定价格数据（见 [D.2](d2-oracle.md)） |

每个验证者 $v_i$ 关联一个权益（stake）$s_i \geq 0$，总权益 $S = \sum_{i} s_i$。共识按**权益加权**，而非按节点计数。

## A.1.2 网络模型

AXON 采用 **部分同步（partial synchrony）** 网络模型（Dwork–Lynch–Stockmeyer）：

$$\exists\, \Delta,\ \mathrm{GST}: \quad t \geq \mathrm{GST} \implies \text{任意消息在 } \Delta \text{ 内送达}$$

其中 $\mathrm{GST}$（Global Stabilization Time）未知、$\Delta$ 有界。含义：网络可能经历任意时长的异步期（消息延迟无界），但最终进入同步期。这是生产级 BFT 系统的现实假设——它让协议在**安全性上不依赖时序假设**，仅在**活性上依赖** GST 之后的同步。

## A.1.3 故障模型

验证者可发生 **拜占庭故障（Byzantine fault）**：故障节点可任意偏离协议（崩溃、静默、发送矛盾消息、合谋）。设拜占庭节点持有的权益为 $S_f$。AXON 的安全性论证建立在标准 BFT 阈值上：

$$S_f < \tfrac{1}{3} S$$

即诚实权益严格大于 $\tfrac{2}{3} S$。**法定人数（quorum）** 定义为持有超过 $\tfrac{2}{3} S$ 权益的验证者集合；任意两个 quorum 的权益交集必含诚实节点，这是安全性的代数基础（见 [B.1](b1-consensus.md)）。

## A.1.4 安全目标

AXON 协议须满足三条性质。设 $\mathsf{commit}(b, h)$ 表示区块 $b$ 在高度 $h$ 被最终确认。

**安全性（Safety）** — 不会有两个诚实验证者在同一高度 $h$ 确认不同区块：

$$\forall h:\ \mathsf{commit}(b,h) \wedge \mathsf{commit}(b',h) \implies b = b'$$

在 $S_f < \tfrac{1}{3}S$ 下**无条件成立**（即使网络完全异步）。

**活性（Liveness）** — 由诚实客户端提交的有效交易，最终会被确认。仅在 $t \geq \mathrm{GST}$ 的同步期内保证。

**确定性最终性（Deterministic Finality）** — 一旦 $\mathsf{commit}(b,h)$ 成立，$b$ **永不被回滚**。不存在「等待 $N$ 个确认」的概率过程；确认即最终。

这三条共同构成支付所需的确定性地基。**安全性优先于活性**：在网络分区等极端情况下，AXON 宁可停止出块（牺牲活性）也不产生冲突确认（保住安全性）——对支付系统，「暂停」远优于「双花」。

## A.1.5 支付特化的附加目标

在通用 BFT 目标之上，支付场景要求：

* **反双花（No Double-Spend）**：同一笔资金不可被两次成功花费——由全局单调序号 + 确定性执行保证（[B.4](b4-sequencing.md)、[B.5](b5-finality.md)）。
* **可重放性（Replayability）**：给定 Entry-Log，任意节点可重建出完全相同的状态。
* **可恢复性（Recoverability）**：崩溃后可从预写日志恢复到一致状态，无资金丢失。
* **有界授权（Bounded Authorization）**：任何自动化主体的支付能力被链层强制约束在可撤销边界内（[C.2](c2-session-keys.md)）。

## A.1.6 信任假设小结

| 组件 | 信任假设 |
| --- | --- |
| 共识安全性 | $S_f < \tfrac{1}{3}S$，无时序假设 |
| 共识活性 | $S_f < \tfrac{1}{3}S$ 且 $t \geq \mathrm{GST}$ |
| 排序器（早期） | 中心化但**可验证**：须自证日志不可篡改、可完整重放（信任最小化，随 P2 去中心化，见 [B.4](b4-sequencing.md)） |
| 喂价 | 多数喂价源诚实 + 偏差熔断兜底（[D.2](d2-oracle.md)） |
| 密码学 | Ed25519 / BLS12-381 / 抗碰撞哈希的标准困难性假设（[A.2](a2-cryptography.md)） |

---

*下一节：[A.2 密码学基础与承诺](a2-cryptography.md)*
