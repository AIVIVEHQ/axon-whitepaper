---
title: 附录 III · 核心数据结构
description: 区块、交易、账户与证明的定义
---

# 附录 III · 核心数据结构

> **设计状态**：proposed design。以下为说明性定义，字段与编码随实现调整；类型记号仅表意（`u64` = 64 位无符号整数等）。

## 区块 Block

```text
Block := {
  header : BlockHeader
  txs    : [ Tx ]              // seqNo 连续区间（B.2.4）
}

BlockHeader := {
  parent_hash  : Hash         // 父块哈希
  height       : u64          // 区块高度
  view         : u64          // 共识视图 r
  tx_root      : Hash         // Merkle(txs)
  state_root   : Hash         // 执行后状态承诺（B.3.3）
  log_head     : Hash         // Entry-Log 哈希链头 h_k（B.4.5）
  timestamp    : u64
  leader       : ValidatorId
  leader_vrf   : VrfProof     // 领导者合法性证明（B.2.3）
  justify_qc   : QuorumCert   // 对 parent 的 QC（B.1.2）
}
```

## Quorum Certificate

```text
QuorumCert := {
  block_hash : Hash           // 被认证的区块
  height     : u64
  view       : u64
  agg_sig    : BlsAggSig      // 聚合投票签名（A.2.3）
  signers    : Bitmap         // 参与验证者位图（用于恢复权益）
}
// 有效性：signers 对应权益 > (2/3)·S 且 agg_sig 验证通过
```

## 交易 Tx

```text
Tx := {
  sender    : Addr
  nonce     : u64             // 反重放（B.3.2）
  calls     : [ Call ]        // 批量原子调用（C.1.3）
  auth      : AuthData        // 验证器所需授权（签名 / 会话密钥证明）
  gas       : GasParams
  paymaster : PaymasterRef?   // 可选费用代付（C.3.2）
}

Call := { target: Addr, asset: AssetId?, amount: u128?, data: bytes? }

GasParams := { limit: u64, price_params: FeeParams }
```

## 账户 Account

```text
Account := {
  nonce        : u64
  balances     : Map<AssetId, u128>   // 多资产一等余额（D.1.2）
  verifier     : VerifierRef          // 可编程验证逻辑（C.1.2）
  policy_root  : Hash?                // 会话密钥/授权策略承诺（C.2.2）
  code         : Hash?                // 可选：WASM 策略/合约承诺（C.3）
  storage_root : Hash?                // 可选：账户存储子树根
}
```

## 会话密钥策略 SessionPolicy

```text
SessionPolicy := {
  session_id : Hash
  pubkey     : PubKey                 // 会话密钥公钥
  scope : {
    L_total   : u128                  // 总限额（累计）
    L_tx      : u128                  // 单笔上限
    window    : (t0: u64, t1: u64)    // 时间窗
    allowlist : [ Addr ]              // 白名单 W
    assets    : [ AssetId ]           // 资产范围 S
    rate      : (max: u32, per: u64)  // 频率 ρ
    calls     : [ CallPattern ]       // 调用范围 F
  }
  state : {                           // 链上认证的会话状态（C.2.3）
    spent : u128
    count : u32
    revoked : bool
  }
}
```

## 状态证明 Proof

```text
InclusionProof := {                   // 包含证明（A.2.4 / B.3.4）
  key   : Key
  value : bytes
  path  : [ Node ]                    // Merkle 路径
}
// 验证：Vrf_trie(state_root, key, value, path) → {0,1}

ExclusionProof := { key: Key, path: [Node] }   // 不存在证明
```

## Entry-Log 条目

```text
LogEntry := {
  seqNo : u64                         // 全局单调序号（B.4.1）
  tx    : Tx
  hash  : Hash                        // h_k = H(h_{k-1} ‖ seqNo ‖ tx)（B.4.2）
}
```

---

> **文档结束。** 本黄皮书描述 AXON Finance 的 proposed design；具体实现、参数与选型以测试网、安全评审与治理结论为准。返回：[黄皮书导言](README.md) · [白皮书首页](../README.md)
