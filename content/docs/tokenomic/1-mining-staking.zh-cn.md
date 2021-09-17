---
title: "1 计算节点的抵押机制"
---

# 简介

Phala Network为了实现计算节点的安全性，除了给计算节点设置安全等级之外，还在挖矿行为中引入了staking概念。每个计算节点通过抵押与自己CPU得分匹配的PHA代币数量，才能获得V值，从而进入挖矿系统开始挖矿。

如果计算节点表现与系统要求不符，则会被惩罚V值，从而引导计算节点网络状态符合规则。

不同于类似于 Polkadot 或其他 Proof-of-Stake 区块链，Phala区块链管理的计算节点数量可以达到100万个CPU核心的量级，即10万台节点以上。因此 Phala Network 抵押机制需要超越现有的Proof-of-Stake 设计，才能在性能和效率上满足需求 —— 对此我们提出了委托抵押（Delegate）设计，通过引入 StakePool 角色，我们允许任何人创建抵押池来连接计算节点和抵押者，抵押者将 $PHA 委托给 Stakepool，由 StakePool 对与池相关联的计算节点进行抵押和管理。

## 角色介绍

| 角色      | 介绍                                                                                                                     |
| --------- | ------------------------------------------------------------------------------------------------------------------------ |
| Worker    | 计算节点，以CPU为单位。在 Phala Network 负责链下计算（隐私环境内）                                                             |
| Operator  | 由 Worker 授权的管理员，负责管理该 Worker 的挖矿行为                                                                     |
| PoolOwner | 创建 StakePool 的地址，负责对 Pool 和 Pool 绑定的 Worker 进行管理                                                        |
| StakePool | 抵押池，作为矿机与抵押者之间资金流动的中间层，服务于挖矿抵押场景的链上操作 （pid 是 StakePool 的标识符，由系统自动生成） |
| Delegator | 持有 $PHA 币的地址，可以通过**委托抵押**参与 Phala 挖矿行为中                                                              |

## 角色关系

|           | Operator                                                                               | Owner                                                                                  | Delegator                                                               |
| --------- | -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Worker    | 每个 Worker 只能授权给一个 Operator ；一个 Operator 可以拥有多个 Worker                | 当某个 Worker 的 Operator 是某个 Pool 的 Owner 的时候，这个 Pool 可以抵押币给该 Worker | /                                                                       |
| StakePool | 当某个 Pool 的 Owner 是某个 Worker 的 Operator 的时候，这个 Pool 可以抵押币给该 Worker | 一个 Owner 可以创建多个 Pool ；每个 Pool 只能绑定一个 Owner                            | 一个 Delegator 可以给多个 Pool 抵押；每个 Pool 也可以拥有多个 Delegator |


### 计算节点与抵押池的关系

![](/images/docs/tokenomic/staking.001.png)
<center>图 1</center>

如图1，

* Owner-A 创建了计算节点 {A, B, C, D}；
* Owner-A 创建了 Pool-1；
* Owner-B 创建了 Pool-2。

![](/images/docs/tokenomic/staking.002.png)
<center>图 2</center>

如图2，如果计算节点的 Operator 地址与 Pool 的 Owner 地址一样，则 Pool 可以添加和管理这些计算节点。

* 因为 Owner-A 既是 **Pool-1** 的 Owner，又是 Worker {A, B, C, D} 的 Operator，因此 Pool-1 可以添加 Worker {A, B, C, D} 产生绑定关系，Pool-1 中的已抵押 $PHA 可以分配给 Worker {A, B, C, D} 进行抵押操作；
* Owner-B 是 Pool-2 的所有者，但是 Owner-B 不能添加和管理 Worker {A, B, C, D}。


其他情况

> - Worker-A 已经授权给了 Owner-A 之后，因此它无法同时授权给 Owner-B 。如果 Worker-A 想要转移控制权给B，则它必须中止挖矿；
> - Pool 目前不支持转移所有权。


### 抵押者与机器的关系

![](/images/docs/tokenomic/staking.003.png)
<center>图 3</center>

图3介绍了 计算节点、 StakePool 和 Delegator 的基本关系：

- StakePool 添加和管理计算节点；
- 委托抵押人将 $PHA 委托给 Stakepool；
- StakePool 将委托人的 $PHA 分配给池中的计算节点进行抵押。

请注意，委托人的 $PHA 始终在自己的地址中，其资产在委托抵押过程中不会发生任何转账交易！

![](/images/docs/tokenomic/staking.004.png)
<center>图 4</center>

如图4，Phala 计算节点挖矿的全生命流程是：
1. Pool-1 被创建，添加了 Worker {A, B, C, D}；
2. 持币者 将 $PHA 委托给 Pool-1；
3. Pool-1 将委托人的 $PHA 分配给池中的计算节点；
4. 计算节点开始挖矿；
5. 每个区块 Phala 产出奖励时，将会根据 Share （算力）分配给 Stakepool；
6. Pool-1的奖励将根据 Commission 被分为两笔，一笔是分给委托人，一笔是分给 StakePool 的 Owner。

#### 案例1：抵押不足

![](/images/docs/tokenomic/staking.005.png)
<center>图 5</center>

假设 Worker-A 和 Worker-B 需要 2000 个 $PHA， Worker-C 和 Worker-D 需要 3000 个 $PHA。委托人给 StakePool 抵押了 7000 个 $PHA，那么：
- 由于 Pool-1 中的总体最小抵押额是 10000 个 $PHA，因此池中的 $PHA 总量无法达到启动所有计算节点的要求；
- Pool-1 可以选择只开启 Worker {A, B, C}进行挖矿。

### Commission

**为了避免 Worker 大规模发生因最小抵押额不足而无法工作的情况，我们创造了 StakePool 协议，允许委托抵押者将代币委托到 StakePool 中。** 而针对于奖励分配，Stakepool 需要设置两个参数：

| 参数       | 参数名 | 作用                                           |
| ---------- | ------ | ---------------------------------------------- |
| Commission | 佣金率 | StakePool 的佣金比例，作为计算节点运行者的抽成 |
| Cap        | 上限值 | StakePool 接受的最大委托上限                   |

StakePool Owner 和 委托人的实际结算公式是：

* Owner奖励 = Stakepool奖励数量 × Commission比例；
* 委托人奖励 = Stakepool奖励数量 ×（1 - Commission比例）× 该委托人抵押额 / Stakepool全部抵押额。


> - 当 Owner 自己也抵押的时候，Owner 的抵押部分视同委托人；
> - Pool 全部抵押额包含 deposit（已抵押）和 free（未抵押）的总和。


在委托抵押者进行委托之后，会产生两个参数：

| 参数            | 参数名   | 作用                                                                      |
| --------------- | -------- | ------------------------------------------------------------------------- |
| Cap Gap         | 剩余额度 | Cap 与实际抵押量的差额，代表持币者还可以在该池委托多少 $PHA                |
| Free Delegation | 闲置资金 | StakePool 中资产总量与实际给计算节点抵押的差额，代表 StakePool 的灵活资金 |

当 StakePool 存在闲置资金时，Owner 可以有使用三个策略：
- 扩大计算节点规模；
- 对存量计算节点进行超额抵押（改变计算节点抵押额需要中止挖矿，V值累计优势将被回收）；
- 适当保留闲置资金，避免委托人随时抽取委托资金。

#### 案例2：佣金设置与奖励分配

![](/images/docs/tokenomic/staking.006.png)
<center>图 6</center>

在图6中，我们可以看到佣金设置比例与奖励分配的全流程：
1. Pool-1 被创建，添加了 Worker {A, B, C, D} ，最小额抵押额总和是 10000 个 $PHA。Pool-1 的佣金率是 60% ；
2. 5 个持币者每人 将 1400 个 $PHA 委托给 Pool-1 ，Pool-1 共有 7000 个 $PHA；
3. Pool-1 将委托人的 7000 个 $PHA 分配给池中的计算节点 Worker {A, B, C} ，Worker-D 因为池中资产总量不足而暂时没有抵押分配；
4. 计算节点 Worker {A, B, C} 开始挖矿；
5. 根据算力占比，区块产出 10 个 $PHA 给 Pool-1；
6. Pool-1 的奖励根据 60% 的 Commission 被分为两笔，4 个 $PHA 平均分给 5 个委托人，每个委托人拿到 0.8 个 $PHA。另外 6 个 $PHA 分给 StakePool 的 Owner。

![](/images/docs/tokenomic/staking.007.png)
<center>图 7</center>

在图7中，我们可以看到当委托量超过 StakePool 实际抵押值时，会产生闲置资金，这时候奖励分配的情况：
1. Pool-1 被创建，添加了 Worker {A, B, C, D} ，最小额抵押额总和是 10000 个 $PHA。Pool-1 的佣金率是 60% ；
2. 5 个持币者每人将 1400 个 $PHA 委托给 Pool-1，还有 1 个持币者将 5000 个 $PHA 委托给 Pool-1 。Pool-1 一共有 12000 个 $PHA；
3. Pool-1 按照只满足最小抵押要求的策略，将委托人的 10000 个 $PHA 分配给池中的计算节点 Worker {A, B, C, D} 。此时 Pool-1 产生了 2000 个 $PHA 闲置资金；
4. 计算节点 Worker {A, B, C， D} 开始挖矿；
5. 根据算力占比，区块产出 10 个 $PHA 给 Pool-1；
6. Pool-1 的奖励根据 60% 的 Commission 被分为两笔，4 个 $PHA 被系统分配给委托人，其中 5 个委托了 1400 个 $PHA 的委托人每人拿到 0.47 个 $PHA 奖励，投入 5000 个的委托人拿到 1.6 个 $PHA 奖励；
7. 剩下 6 个 $PHA 分给 StakePool 的 Owner。

### 退出抵押结算

如果 Owner 希望取消挖矿或者提取全部抵押（不能提取委托人的存款），他可以通过中止挖矿操作实现。他在发起该类交易后，StakePool 内的存款将会进行7天的解冻期，解冻期后该余额可以被解冻，Owner 和 委托人的质押 $PHA 将会被全额退回。

![](/images/docs/tokenomic/staking.008.png)
<center>图 8</center>

如图8，如果委托人发起提款，且提取金额小于 StakePool 中闲置资金，则委托人可以立即获得提取资金。

但如果委托人发起提款，且提取金额大于StakePool中闲置资金，则委托人可以立即获得闲置状态的资金，但剩余资金需要排队等待：

![](/images/docs/tokenomic/staking.009.png)
<center>图 9</center>

如图9，委托人一共需要提取 4000 个 $PHA，但此时 Pool-1 中只有 10000 个 $PHA，那么委托人会立即获得 2000 个 $PHA，其余 2000 个 $PHA 需要等待。

此时Pool-1实际上产生了 2000 个币的缺口，Pool-1 的最小抵押额为 10000 个币，但还款给委托人后应该是 8000 个币。此时 Pool-1 的 Owner 有两个做法：

![](/images/docs/tokenomic/staking.010.png)
<center>图 10</center>

如图10，如果 Pool-1 在7天内一直没有补充委托质押的资金，则则该抵押池所有 Worker 都会进入7天的冷却期，此过程不能被任何人中止，7天过后该提款交易就可全部完成

![](/images/docs/tokenomic/staking.011.png)
<center>图 11</center>

如图11，如果有未成功提款资金，则委托人需要先等待一个7天的缓冲期，在此期间 StakePool 中如果加入了新的闲置资金或有 Worker 停止挖矿，那此部份闲置资金立即解锁给委托人发起的退出交易，以此类推直到完成全部提款。

总结：
- 任意委托人发起退出交易后，最多14天就可以成功退出抵押；
- StakePool Owner 应时常关注池中的闲置资金，不足时应立刻补充或减少运行状态的计算节点，否则将会导致所有池中计算节点被停机。
