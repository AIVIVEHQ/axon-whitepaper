---
title: A.2 密码学基础与承诺
description: 哈希、签名、聚合与状态承诺
---

# A.2 密码学基础与承诺

> **设计状态**：proposed design。密码学原语为标准选型，可随安全评审调整。

本节定义全文使用的密码学原语。所有原语均为经充分分析的标准构造，AXON 不引入自研密码学。

## A.2.1 哈希函数

设抗碰撞哈希函数

$$H : \{0,1\}^* \to \{0,1\}^{256}$$

AXON 采用 **BLAKE3** 作为主哈希（高吞吐、可并行、树形结构天然适配 Merkle 承诺），对需与外部 EVM 生态互操作的场景保留 **Keccce-256** 兼容路径。哈希用于：交易/区块标识、Merkle 承诺、随机信标派生。

## A.2.2 交易签名 — Ed25519

客户端交易采用 **Ed25519**（Curve25519 上的 EdDSA）。相比 ECDSA，Ed25519 具有确定性签名（无需安全随机数）、快速验证、无签名可延展性（malleability）——最后一点对反双花至关重要：交易哈希不会因签名重编码而改变。

签名方案 $\Sigma = (\mathsf{KeyGen}, \mathsf{Sign}, \mathsf{Vrf})$：

$$(pk, sk) \gets \mathsf{KeyGen}(1^\lambda), \quad \sigma \gets \mathsf{Sign}(sk, m), \quad \{0,1\} \gets \mathsf{Vrf}(pk, m, \sigma)$$

账户抽象（[C.1](c1-account-abstraction.md)）允许账户自定义验证逻辑，Ed25519 是默认方案，但账户可声明其它验证器（如多签、passkey/secp256r1、门限签名）。

## A.2.3 共识投票聚合 — BLS12-381

共识层的验证者投票采用 **BLS 签名（BLS12-381 曲线）**，其核心优势是**签名聚合**：$n$ 个验证者对同一消息的签名可聚合成单个常数大小签名，验证一次即可。

设 $\sigma_i = \mathsf{Sign}_{\mathsf{BLS}}(sk_i, m)$，聚合：

$$\sigma_{\mathsf{agg}} = \prod_{i \in Q} \sigma_i, \qquad \mathsf{VrfAgg}\big(\{pk_i\}_{i\in Q},\, m,\, \sigma_{\mathsf{agg}}\big) \to \{0,1\}$$

这使得一个 **quorum certificate（QC）**——超过 $\tfrac{2}{3}S$ 权益对某区块的投票——可被压缩为「聚合签名 + 参与者位图」，大小与验证者数量无关。QC 是共识的核心凭证（[B.1](b1-consensus.md)），聚合签名让区块头能高效携带最终性证明。

## A.2.4 状态承诺 — Merkle 化状态树

全局状态 $\mathsf{st}$ 承诺为一个 **Merkle 化的 authenticated 键值树**，其根 $\mathsf{root}(\mathsf{st})$（32 字节）写入区块头。默认结构为 **Merkle Patricia Trie（MPT）** 的优化变体，并按 **Verkle-ready** 设计——保留未来迁移到 Verkle Tree（基于向量承诺，证明体积大幅缩小）的接口。

承诺提供两类证明：

* **包含证明（inclusion）**：证明 $(k, v) \in \mathsf{st}$，即 $\exists\,\pi:\ \mathsf{Vrf}_{\mathsf{trie}}(\mathsf{root}, k, v, \pi) = 1$。
* **不存在证明（exclusion）**：证明 $k \notin \mathsf{st}$。

轻客户端与跨链桥凭状态根 + 证明即可验证任意账户余额或支付结果，无需信任全节点——这是「可证明状态根」的技术含义（[B.3](b3-state.md)）。

## A.2.5 可验证随机 — VRF

领导者选举与反可预测排序需要**不可操纵的随机性**。AXON 采用 **VRF（Verifiable Random Function）**：

$$(y, \pi) \gets \mathsf{VRF}_{sk}(x), \qquad \mathsf{VrfProof}(pk, x, y, \pi) \to \{0,1\}$$

输出 $y$ 对不知 $sk$ 者不可预测，但任何人可凭 $\pi$ 验证 $y$ 由 $x$ 与 $pk$ 唯一确定。用途：每轮以 $x = (\text{epoch}, \text{round})$ 派生领导者（[B.2](b2-validators.md)），使领导序列不可被提前操纵，压缩审查与 MEV 空间。

## A.2.6 原语小结

| 原语 | 方案 | 用途 |
| --- | --- | --- |
| 哈希 $H$ | BLAKE3（Keccak-256 兼容路径） | 标识、承诺、随机派生 |
| 交易签名 | Ed25519（账户可自定义验证器） | 交易授权 |
| 共识签名 | BLS12-381 聚合 | Quorum Certificate |
| 状态承诺 | Merkle 树（Verkle-ready） | 状态根、包含/不存在证明 |
| 随机 | VRF | 领导者选举、反操纵排序 |

具体曲线、哈希与参数的最终选型以安全评审结论为准（[附录 II](appendix-parameters.md)）。

---

*下一节：[B.1 BFT PoS 共识协议](b1-consensus.md)*
