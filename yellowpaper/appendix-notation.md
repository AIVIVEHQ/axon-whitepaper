---
title: 附录 I · 符号表
description: 全文形式化记号约定
---

# 附录 I · 符号表

全文形式化记号统一定义于此，供各节回指。

## 系统与共识

| 记号 | 含义 | 出处 |
| --- | --- | --- |
| $\mathcal{V} = \{v_1,\dots,v_n\}$ | 验证者集合 | [A.1.1](a1-system-model.md) |
| $s_i$ | 验证者 $v_i$ 的有效权益（自有 + 委托） | [A.1.1](a1-system-model.md)·[F.2.1](f2-staking-slashing.md) |
| $S = \sum_i s_i$ | 总权益 | [A.1.1](a1-system-model.md) |
| $S_f$ | 拜占庭节点权益，假设 $S_f < \tfrac13 S$ | [A.1.3](a1-system-model.md) |
| $\Delta,\ \mathrm{GST}$ | 消息延迟界、全局稳定时间（部分同步） | [A.1.2](a1-system-model.md) |
| $r$ | 视图（共识轮次） | [B.1.2](b1-consensus.md) |
| $\mathsf{QC}(b)$ | 区块 $b$ 的 quorum certificate（权益 $>\tfrac23 S$） | [B.1.2](b1-consensus.md) |
| $\mathsf{Leader}(r)$ | 视图 $r$ 的领导者（VRF 派生） | [B.2.3](b2-validators.md) |
| $\mathsf{commit}(b,h)$ | 区块 $b$ 在高度 $h$ 最终提交 | [A.1.4](a1-system-model.md) |
| $T_{\text{final}}$ | 最终性延迟 | [B.1.7](b1-consensus.md)·[G.2.2](g2-performance.md) |

## 密码学

| 记号 | 含义 | 出处 |
| --- | --- | --- |
| $H(\cdot)$ | 抗碰撞哈希（BLAKE3） | [A.2.1](a2-cryptography.md) |
| $\Sigma=(\mathsf{KeyGen},\mathsf{Sign},\mathsf{Vrf})$ | 签名方案（Ed25519） | [A.2.2](a2-cryptography.md) |
| $\sigma_{\mathsf{agg}}$ | BLS 聚合签名 | [A.2.3](a2-cryptography.md) |
| $\mathsf{root}(\mathsf{st})$ | 状态承诺根 | [A.2.4](a2-cryptography.md)·[B.3.3](b3-state.md) |
| $\mathsf{VRF}_{sk}(x)\to(y,\pi)$ | 可验证随机函数 | [A.2.5](a2-cryptography.md) |

## 状态与执行

| 记号 | 含义 | 出处 |
| --- | --- | --- |
| $\mathsf{st}:\mathsf{Addr}\to\mathsf{Account}$ | 全局状态 | [B.3.1](b3-state.md) |
| $\delta,\ \delta_{\mathsf{tx}}$ | 状态转换函数（区块 / 单交易） | [B.3.2](b3-state.md) |
| $\mathsf{seqNo}(\mathsf{tx})$ | 全局单调序号 | [B.4.1](b4-sequencing.md) |
| $\mathsf{Log},\ e_k,\ h_k$ | Entry-Log、日志条目、哈希链 | [B.4.2](b4-sequencing.md) |
| $\mathsf{Replay}(\mathsf{st}_0,\mathsf{Log})$ | 重放函数 | [B.4.3](b4-sequencing.md) |

## 授权与 AI

| 记号 | 含义 | 出处 |
| --- | --- | --- |
| $\mathsf{Verify}(\mathsf{Account},\mathsf{Tx})$ | 可编程验证器 | [C.1.2](c1-account-abstraction.md) |
| $P=\{c_1,\dots,c_k\}$ | 授权策略（约束集） | [C.2.2](c2-session-keys.md) |
| $\mathsf{Auth}_P(\mathsf{tx},\mathsf{ctx})=\bigwedge_c c$ | 授权谓词 | [C.2.3](c2-session-keys.md) |
| $L_{\text{total}},L_{\text{tx}},W,\mathcal{S},\rho$ | 总限额/单笔上限/白名单/资产范围/频率 | [C.2.2](c2-session-keys.md) |
| $\mathsf{spent},\mathsf{count}$ | 会话累计花费 / 窗口计数 | [C.2.3](c2-session-keys.md) |

## PayFi 与经济

| 记号 | 含义 | 出处 |
| --- | --- | --- |
| $U = B/D$ | 利用率（借出/供给） | [E.1.2](e1-money-market.md) |
| $r_{\text{borrow}}(U),\ r_{\text{supply}}(U)$ | 借款/存款利率曲线 | [E.1.3](e1-money-market.md) |
| $U^{*}$ | 最优利用率拐点 | [E.1.3](e1-money-market.md) |
| $H=\sum C_i\lambda_i/\text{Debt}$ | 健康因子 | [E.1.5](e1-money-market.md) |
| $\lambda_i$ | 抵押物 $i$ 的清算阈值 | [E.1.5](e1-money-market.md) |
| $\phi$ | 准备金因子 | [E.1.3](e1-money-market.md)·[E.2.4](e2-liquidation.md) |
| $\hat{p},\ \mathrm{MAD},\ z_i$ | 聚合价格 / 中位数绝对偏差 / 偏差得分 | [D.2](d2-oracle.md) |
| $g$ | 带单保底率（$\in[1\%,3\%]$） | [E.3.5](e3-copy-trading.md) |
| $\chi$ | 对冲后单场最大损失比例 | [E.4.2](e4-reserve-risk.md) |
| $R_{ct}$ | 带单准备金池 | [E.4.3](e4-reserve-risk.md) |
| $\Xi=R_{ct}/\sum\text{MaxLoss}$ | 带单准备金覆盖率（$\geq\Xi_{\min}=150\%$） | [E.4.3](e4-reserve-risk.md) |
| $\phi_{ct}$ | 带单准备金注入费率 | [E.4.3](e4-reserve-risk.md) |
| $\Delta_{\text{chal}}$ | 结算争议窗口 | [E.3.5](e3-copy-trading.md) |
| $\mathsf{gas}(\mathsf{tx})$ | 交易 gas | [F.1.1](f1-gas-fees.md) |
| $T_{\text{unbond}}$ | 解绑期 | [F.2.4](f2-staking-slashing.md) |
| $\Theta,\ \text{fee}$ | 吞吐 / 单笔费用 | [G.2](g2-performance.md) |

---

*下一节：[附录 II · 协议参数表](appendix-parameters.md)*
