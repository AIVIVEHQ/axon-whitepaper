---
title: B.2 验证者集合与区块生产
description: 权益、领导者选举与集合演化
---

# B.2 验证者集合与区块生产

> **设计状态**：proposed design。质押门槛、epoch 长度、验证者规模为待定参数（[附录 II](appendix-parameters.md)）。

## B.2.1 权益与 epoch

系统时间划分为 **epoch**（固定视图数或时长的区间）。验证者集合 $\mathcal{V}_e$ 在 epoch $e$ 内**冻结**——集合变更（加入/退出/权益调整）在 epoch 边界统一生效。这给共识一个稳定的投票权重基准，也让轻客户端只需跟踪 epoch 级的验证者集合变化。

验证者 $v_i$ 的投票权重正比于其**有效权益** $s_i$（自有质押 + 委托，见 [F.2](f2-staking-slashing.md)）。所有 quorum 判定按权益，而非节点数。

## B.2.2 准入与退出

$$\text{加入}:\ s_i \geq s_{\min} \ \Rightarrow\ \text{进入激活队列} \xrightarrow{\text{epoch 边界}} \mathcal{V}_{e+1}$$

* **激活队列**：新验证者排队，按速率限制逐 epoch 激活，防止集合被瞬间灌入而失稳。
* **退出队列 + 解绑期**：验证者退出后，其质押进入**解绑期（unbonding period）** $T_{\text{unbond}}$ 才可提取。解绑期覆盖可能的 slashing 追责窗口（[F.2](f2-staking-slashing.md)）——防止作恶者立即撤资逃避罚没。
* **上限**：验证者数量设软上限 $n_{\max}$（控制共识通信成本），超出者按权益竞争入选，其余作为候选。

## B.2.3 领导者选举（VRF，权益加权）

每个视图 $r$ 的领导者由 VRF（[A.2.5](a2-cryptography.md)）确定，被选中概率正比于权益：

$$\Pr[\mathsf{Leader}(r) = v_i] = \frac{s_i}{S}$$

流程：以随机信标 $\mathsf{beacon}_e$（由上一 epoch 的 VRF 输出聚合派生，不可被单方操纵）为种子，$x = (\mathsf{beacon}_e, r)$，验证者本地计算 $\mathsf{VRF}_{sk_i}(x)$ 并按权益阈值判定是否中选。

VRF 选举的三个性质：

* **不可预测**：领导序列在信标确定前无法提前得知——压缩审查与 MEV 的预谋空间。
* **可验证**：中选者出示 $\pi$，任何人可验证其合法性——无法冒充领导者。
* **无偏**：中选概率严格按权益，无稀释。

## B.2.4 区块生产

领导者 $\mathsf{Leader}(r)$ 从**排序层交付的有序交易流**（[B.4](b4-sequencing.md)）中取一批，打包为区块：

```text
ProduceBlock(r):
  parent  ← 本地最高 QC 指向的区块
  txs     ← Sequencer 交付的 [seqNo_start .. seqNo_end] 连续区间
  header  ← { parent_hash, height, view=r, state_root_prev,
              tx_root = Merkle(txs), timestamp, leader_vrf_proof }
  b       ← { header, txs, QC_just = parent 的 QC }
  签名并广播 b
```

关键约束：区块内交易的 `seqNo` 必须是**连续区间**，且严格衔接父块——领导者无法跳过、重排或注入序号（排序权与出块权分离，见 [B.4](b4-sequencing.md)），这限制了领导者的排序操纵能力。

## B.2.5 委托

持币者可将权益**委托（delegate）**给验证者，分享其共识奖励并共担 slashing 风险，无需自行运行节点。委托是纯共识/安全机制；其数学与奖励分配见 [F.2](f2-staking-slashing.md)。委托降低了验证者集合的资本门槛，提升去中心化程度，同时不增加共识的投票主体数量（委托聚合到验证者名下）。

## B.2.6 渐进去中心化

与 [A.1.6](a1-system-model.md) 的信任假设一致，验证者集合按路线图（白皮书 [6.1](../part6-roadmap/6-1-roadmap.md) 的 P2）逐步扩大：早期规模较小以保证性能与快速迭代，通过 slashing、解绑期与 VRF 无偏选举建立公信力，再逐步放开准入、扩大 $n_{\max}$，将排序与验证的去中心化程度推高。

---

*下一节：[B.3 状态模型与状态承诺](b3-state.md)*
