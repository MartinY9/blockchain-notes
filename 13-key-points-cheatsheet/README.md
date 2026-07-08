# 第十三篇 核心知识点速览

> 全书要点的一页速查表，用来快速回顾和自检。
> 每条给「一句话结论 + 关键词」，需要展开就回到对应章节。

---

## 一、基础与演进

- **比特币如何解决双花**：全网维护唯一的时间排序账本 + PoW + 最长链规则；攻击需 51% 算力，作恶成本 > 收益。属**概率最终性**（6 确认约 1 小时）。→ [第一篇](../01-blockchain-basics-and-evolution/)
- **UTXO vs 账户模型**：UTXO = 未花费输出，天然并行/安全/隐私好、无需 nonce；Account = 地址→状态映射，表达力强/易写合约但难并行、需 nonce。UTXO 为「安全并行」，Account 为「可编程」。→ [第一篇](../01-blockchain-basics-and-evolution/)
- **以太坊为什么用账户模型**：为支持有状态的智能合约（合约需要持久可变状态，账户模型天然是 key-value）。→ [第一篇](../01-blockchain-basics-and-evolution/)
- **区块为什么不可篡改**：区块头存前一区块哈希 + Merkle Root，改任一历史数据会导致后续所有哈希失效，还要重做后面所有共识。→ [第二篇](../02-blockchain-architecture/)
- **Merkle Tree 的作用**：把交易汇总成一个根，轻节点用 Merkle Proof 验证「某交易是否在块中」而无需下载全部交易。→ [第一篇](../01-blockchain-basics-and-evolution/)

## 二、架构

- **分层**：存储 → 网络 → 共识 → 执行 → 合约 → 应用。→ [第二篇](../02-blockchain-architecture/)
- **交易传播**：先本地验证再 Gossip 广播（`inv/getdata`），防垃圾交易/DoS。→ [第二篇](../02-blockchain-architecture/)
- **以太坊三个 Root**：State / Transaction / Receipt Root，分别验证状态、交易是否入块、事件是否发生。→ [第二篇](../02-blockchain-architecture/)
- **三种状态模型**：UTXO（安全并行）/ Account（可编程）/ Object（两者兼顾，靠预声明读写集并行）。→ [第二篇](../02-blockchain-architecture/)
- **状态膨胀**：全局状态越来越大；缓解用状态过期 / Verkle Tree / 无状态客户端。→ [第二篇](../02-blockchain-architecture/)

## 三、共识

- **为什么需要共识 / FLP**：拜占庭环境下对唯一顺序达成一致；FLP 说明异步下 Safety 与 Liveness 不可兼得，都要取舍。→ [第三篇](../03-consensus-algorithms/)
- **PoW 为什么安全**：作恶需持续掌握 51% 算力，成本巨大且成功后自身资产贬值。缺点是能耗高、TPS 低。→ [第三篇](../03-consensus-algorithms/)
- **PoS 如何抵御攻击**：出块需质押，作恶被 Slashing 没收本金，解决 Nothing-at-Stake；叠加 Casper FFG 实现不可逆。→ [第三篇](../03-consensus-algorithms/)
- **PBFT 为什么 3f+1**：在「f 作恶 + f 掉线」下仍能拿到诚实多数（quorum = 2f+1），保证不冲突提交。三阶段 Pre-prepare/Prepare/Commit。→ [第三篇](../03-consensus-algorithms/)
- **HotStuff 优势**：通信从 O(n²) 降到 O(n)（leader 聚合签名）+ 流水线，能扩展到大规模验证者。→ [第三篇](../03-consensus-algorithms/)
- **PoH**：不是共识，是「预先排好序的时钟」，让节点不通信就对先后顺序达成一致（Solana）。→ [第三篇](../03-consensus-algorithms/)
- **Narwhal + Bullshark**：把 Mempool（数据传播）与共识解耦，大幅提高吞吐。→ [第三篇](../03-consensus-algorithms/)
- **两类最终性**：概率最终性（PoW/PoS）vs 确定性最终性（BFT）。→ [第三篇](../03-consensus-algorithms/)

## 四、合约与虚拟机

- **EVM**：栈式虚拟机，要求确定性执行（禁随机/时间/浮点）。→ [第四篇](../04-smart-contracts-and-vm/)
- **为什么要 Gas**：防死循环（绕开停机问题）+ 资源定价 + 反 DoS；EIP-1559 = base fee 销毁 + priority fee。→ [第四篇](../04-smart-contracts-and-vm/)
- **WASM vs Move**：WASM 卖点是跨语言 + 高性能；Move 卖点是从类型系统层面保证资源安全 + 天然并行。→ [第四篇](../04-smart-contracts-and-vm/)

## 五、扩展技术

- **L1 vs L2 扩容**：L1（并行/共识/状态优化，代价是去中心化）；L2（Rollup，把执行搬链下、安全锚定 L1）。→ [第五篇](../05-scaling-technologies/)
- **Optimistic vs ZK Rollup**：前者用欺诈证明 + 挑战期（慢但 EVM 兼容好），后者用有效性证明（快且更安全但成本高）。→ [第五篇](../05-scaling-technologies/)
- **数据可用性（DA）**：Rollup 的隐藏命门——数据必须公开可得；DAS 让轻节点不下载全部数据也能确信数据可得；EIP-4844 blob 降低 L2 成本。→ [第五篇](../05-scaling-technologies/)

## 六、跨链

- **跨链为什么难**：链间无共享信任根，A 无法直接验证 B 的状态。→ [第六篇](../06-cross-chain/)
- **四种路线**：中心化中继 → 多签公证人 → 轻客户端 → IBC（信任假设由强到弱）。→ [第六篇](../06-cross-chain/)
- **IBC**：轻客户端 + 标准化四层（Client/Connection/Channel/Packet），Relayer 无需信任。→ [第六篇](../06-cross-chain/)
- **桥为什么被盗最多**：把安全外包给少数外部验证者或有 bug 的验证代码（Ronin 私钥、Wormhole 验证 bug）；防御靠最小化信任 + 限额熔断 + 监控。→ [第六篇](../06-cross-chain/)

## 七、Web3 与产业趋势

- **NFT 本质与价值**：链上唯一所有权凭证（图片常在 IPFS）；价值从收藏走向凭证化。ERC-721 vs ERC-1155。→ [第七篇](../07-web3-applications/)
- **DeFi 三大件**：DEX（AMM 定价 + 无常损失）、借贷（超额抵押 + 清算）、稳定币（三类，算法型有死亡螺旋风险）。→ [第七篇](../07-web3-applications/)
- **RWA 为什么重要 + 难在哪**：现实资产通证化提升流动性；最大难点是「链上↔链下可信映射 + 合规」（信任的最后一公里）。→ [第八篇](../08-rwa/)
- **国内外 RWA 路线**：海外「公链 + 金融资产」，国内「联盟链 + 强合规 + 实体产业」。→ [第八篇](../08-rwa/)
- **x402**：复用 HTTP 402，让 AI Agent 用稳定币按次原生支付 API/数据/算力，支撑机器经济。→ [第九篇](../09-x402-and-ai-blockchain/)

## 八、生态与系统设计

- **公链 vs 联盟链**：信任假设不同（无准入 vs 许可制），进而决定共识、身份、性能、合规。联盟链不是阉割版公链。→ [第十篇](../10-ecosystem-comparison/)
- **企业为什么要联盟链**：高 TPS + 确定性最终性、合规隐私、可控可追责、成本可预期。→ [第十篇](../10-ecosystem-comparison/)
- **区块链为什么慢 + 提 TPS**：全局冗余换无需信任；从共识/执行/存储/网络四瓶颈 + 分层 + 分片优化，代价是去中心化。→ [第十一篇](../11-system-design/)
- **稳定性设计**：区块链特性（分叉/最终性/活性）+ 传统 SRE（多副本/监控/限流/降级）。→ [第十一篇](../11-system-design/)

## 九、安全

- **合约三大漏洞**：重入（Checks-Effects-Interactions + 重入锁）、整数溢出（0.8+ / SafeMath）、访问控制（RBAC + 多签）。→ [第十二篇](../12-blockchain-security/)
- **闪电贷 + 预言机操纵**：一个区块内借巨额资金操纵 AMM 现货价套利；防御用 TWAP、去中心化预言机。→ [第十二篇](../12-blockchain-security/)
- **共识/网络攻击**：51% / Sybil / Eclipse / 长程攻击。→ [第十二篇](../12-blockchain-security/)

---

## 贯穿全场的「三问框架」

理解任何一个新技术，都用这三个问题去组织：

1. **它从哪来？** —— 解决上一代的什么瓶颈？（演进逻辑）
2. **代价是什么？** —— 在「去中心化 / 安全 / 性能」不可能三角上牺牲了什么、换来什么？（权衡）
3. **往哪去？** —— 在产业趋势里它的位置和局限？（判断价值）

> 核心不是知道多少名词，而是能像一个懂取舍的系统架构师那样思考。

---

## 建议阅读顺序

```
区块链历史 → 架构 → 共识 → 跨链 → RWA/Web3 → 分布式系统 → 系统设计
```
