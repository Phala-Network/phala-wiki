---
title: "了解TEE挖矿经济模型"
weight: 3
draft: false
---

<script>
  MathJax = {
    tex: {
      inlineMath: [['$', '$'], ['\\(', '\\)']],
      displayMath: [['$$','$$'], ['\\[', '\\]']],
      processEscapes: true,
      processEnvironments: true
    },
    options: {
      skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre']
    }
  };
  window.addEventListener('load', (event) => {
      document.querySelectorAll("mjx-container").forEach(function(x){
        x.parentElement.classList += 'has-jax'})
    });
</script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

> 本文是对于Konstantin Shamruk博士的《Phala经济模型白皮书V0.9》的总结预览，供Phala社区提前了解和研究。它也将会被提交至Khala Network议会和公投进行民主投票，在公投通过之后将会把该模型对应的配置文件和代码上线。


## 设计目标

1. 支持Phala Network去信任化云服务的计算架构
    - 共识和计算分离
    - 可不断线性增长的计算节点（10万台量级的计算节点）
2. 激励矿工加入网络
    - 分别引导需求端和供给端的冷启动
    - 初始供应量的 70% 的补贴池
    - 类似比特币的预算减半时间表
3. 给应用支付合理定价
4. 链上性能表现

## 设计概要

### Value Promise 价值承诺模型 ($V$)

- V是每个矿工的虚拟分数，用于保证系统的安全
- V等于矿工通过在云平台贡献获得的收入（最小值）的预期总和
- V会根据矿工的行为和奖励的偿还动态变化
    - 诚实的挖矿行为: $V$ 会根据矿工在系统内的时间线性增长
    - 对系统有害的行为: 通过削减 $V$ 来达到惩罚目的

### 初始 $V$ 值

矿工必须运行**性能测试**并质押一些代币才能获得初始 $V$:

$$V^e = f(R^e, \text{ConfidenceScore}) \times (S + C)$$


- $R^e > 1$ 是可变的 ***抵押乘数***, 由网络决定乘数常量（Khala 或 Phala）
- $S$ 是机器的抵押额;  ***最小抵押额*** 是开始挖矿的最低要求。**挖矿启动后，抵押额不可以再进行调整（减少或增加）**
- $C$ 是矿机成本, 由 ***性能测试分*** 来进行预估
- $ConfidenceScore$是***信任等级分***
- $f(R^e, \text{ConfidenceScore}) = 1 + (\text{ConfidenceScore} \cdot (R^e - 1))$

模拟实验结果建议:

- $R^e_{\text{Khala}} = 1.5$
- $R^e_{\text{Phala}} = 1.3$
- 不同等级的$\text{ConfidenceScore}$  [Confidence Levels](https://wiki.phala.network/en-us/docs/poc3/1-4-software-configuration/#confidence-level-of-a-miner)
  - $\text{ConfidenceScore}_{1,2,3} = 1$
  - $\text{ConfidenceScore}_{4} = 0.8$
  - $\text{ConfidenceScore}_{5} = 0.7$

### 性能测试分

CPU性能测试是衡量在单位时间内可以完成多少计算:

$$P = \frac{\text{Iterations}}{\Delta t}$$

以下是实验参考数据：
| 测试平台 | 核心数 | 得分 | 估价 |
| -------- | -------- | -------- | -------- |
| 低端赛扬 | 4 | 450 | $150 |
| 中端 i5 10-Gen | 8 | 2000 | $500 |
| 高端 i9 9-Gen | 10 | 2800 | $790 |

> 该表基于撰写本文档时的版本，可能会发生变化。

性能测试分数将会被用于输入:

1. 挖矿前: 决定 ***最小抵押额***
2. 挖矿中: 衡量动态表现

### 最小抵押额

$$S_{min}=k \sqrt{P}$$

- $P$ - ***性能测试*** 分数
- $k$ - 可调整的常数变量

模拟实验结果建议:

- $k_{\text{Khala}} = 50$
- $k_{\text{Phala}} = 100$

> 锁定状态的$PHA token也可以用于挖矿抵押，例如参与卡槽拍卖的Khala Crowloan奖励

### 挖矿成本估计

$$C=\frac{0.3 P}{\phi}$$

- $\phi$ 是当前 PHA/USD 价格输入, 由链上预言机动态输入
- $P$ 是 ***CPU性能测试*** 得分.
- 在早期阶段，系统以价值承诺来覆盖设备成本 $C$
- 未来我们计划转向补偿更高的摊销成本（将设备摊销成本添加到运行成本 $c^i$ 和 $c^a$），从而提高 Miner $V$ 的增长速度

### 一般挖矿流程

![](https://i.imgur.com/IpEnlGR.png)

![](https://i.imgur.com/zKWAI1S.png)

每个机器的 $V$ 会在每个区块更新:

-  $\Delta V_t$ 增长，只要计算节点持续工作
-  $w(V_t)$ 减少，如果矿机成功收到奖励支付
-  $w(V_t)$ 减少，如果根据 ***惩罚规则*** 矿机有负面行为

一旦矿工获得支付 $w(V_t)$，他将立即在他的 Phala 钱包中收到支付金额。支付遵循***支付时间表***并且必须满足***补贴预算***

最后，一旦矿工决定停止挖矿，他将等待一个冷却期 $\delta$ 之后，（有可能）获得一次性最终提款。

| 区块高度 | $t$ | $t+1$ | $\dots$ | $T$ | $\dots$ | $T+\delta$ |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|
|价值承诺|$V_t$|$V_{t+1}$|$\dots$|$V_T$|$\dots$|$\dots$|
|支付数量 | $w(V_t)$ | $w(V_{t+1})$ | $\dots$ | $w(V_T)$ | $0$ | $\kappa V_T$ |
|| 区块奖励 | ... | ... | 区块奖励 | 冷却 $\delta$ 个区块 | 最终支付 (Khala链上为0) |

模拟实验结果建议:

- $\delta = 7$ (或更低)

###  $V$ 值更新

假设没有支付行为和惩罚行为:

$$\Delta V_t = k_p \cdot \big(\rho^m V_t + c(s_t) + \gamma(V_t)h(V_t)\big)$$

- $\rho^m$ 是机器 $V$ 值的无条件增量系数
- $c(s_t)$ 是估算的运行机器的运维成本，如电费、网络费用、管理费用等
- $\gamma(V_t)h(V_t)$ 代表对诚实矿工意外惩罚的补偿因子（在模拟中被忽略）
- $k_p = \min(\frac{P_t}{P}, 120\\%)$，其中 $P_t$ 为性能测试的瞬时值， $P$ 为初始性能分数

模拟实验结果建议:

- $\rho^m_{\text{Khala}} = 1.00020$ (每小时)
- $\rho^m_{\text{Phala}} = 1.00020$ (每小时)

### 支付事件

为了满足补贴预算要求，每个区块会根据当前 ***矿工份额*** 按比例分配预算。与此同时，支付金额也受到 $V$ 增量的限制，以确保它不会随着支付而比上一次支付时更低：

$$w(V_t) = \min(B \frac{\text{share}}{\Sigma \text{share}}, V_t - V_\text{last})$$

其中，$B$ 是单位时间内的当前网络补贴预算，而 $V_\text{last}$ 是上一次支付事件时的 $V$ 值，或者当此次为第一次支付时，取初始 $V^e$。

{{< tip >}}
当支付额较高时，为了确保确保经济模型激励合理，鼓励矿工在网络中积累信用，它的上限必须被控制在合理范围。如果诚实矿工的V随着挖矿反而下降，经济模型就会鼓励矿工反复开关机，而干扰网络的运转。
{{< /tip >}}

每当 $w(V_t)$ 支付给矿工时，他的 $V$ 将相应更新:

$$\Delta V = -w(V_t)$$

Share（份额）代表我们将允许矿工从 $V$ 中提取多少。实际值应当在基准值上微调:

$$\text{share}_{\text{Baseline}} = V_t.$$

模拟实验结果建议:

- $\text{share}_{\text{Khala}} = \sqrt{V_t^2 + (2 P_t \cdot \text{ConfidenceScore})^2}$
- $\text{share}_{\text{Phala}} = \sqrt{V_t^2 + (2 P_t \cdot \text{ConfidenceScore})^2}$
- 其中 $P_t$ 为瞬时性能分数

### 补贴预算

| 参数/平行链  | 总和   |   Khala   |  Phala  |
|-----|:-------:|:---------:|:-------:|
| 中继链 | / |   Kusama   |   Polkadot    |
| 挖矿补贴预算 | 7亿 |   1000万   |   6.9亿  |
| 衰减周期 | /|   45 天   |   180 天 |
| 衰减比例 | / |   每次25%   |   每次25% |
| 税率 | /|   20%   |   20% |
| 首月奖励额 | / |   180万   |   2160万 |

### 在线心跳

如果在区块 $t$ 的矿工的 VRF 小于他当前的心跳阈值 $\gamma(V_t)$，该矿机必须将心跳交易发送到链上（自动），链将更新他的V值情况到链上记录，并发送一次支付奖励 $w(V_t)$ 到他的钱包：

$$\Delta V_t = - w(V_t).$$

如果他没有在挑战窗口（$\text{ChallengeWindow}$）规定的区块数之内将在线心跳交易发送到链上，他的V值将更新为：

$$\Delta V_t = - h(V_t),$$

并且他的状态更改为*无响应*，并在接下来的每个区块受到惩罚，直到矿工重新回到网络并回应心跳请求，或手动停止挖矿。削减的 $V$ 值 $h$ 在 ***惩罚*** 部分定义。

心跳设计的目标是让系统承受每个块处理大约 20 个心跳交易。心跳挑战概率 γ (V t ) 将根据这个挑战交易数量目标进行自动调整。

实验模拟结果建议：

- $\text{ChallengeWindow} = 10$ (区块)

### 惩罚

矿工的惩罚规则定义如下。请注意，目前系统仅实践了 1 级惩罚：

| 严重性 | 错误行为 | 惩罚 |
| -----|-------|------|
| 等级1	| 机器掉线	| 每小时惩罚 0.1% 的V (平摊到每区块) |
| 等级2	| 被动发生的错误行为	| 惩罚1%  的V |
| 等级3	| 恶意行为/大规模下线	| 10% 的V |
| 等级4	| 对系统产生严重危害	| 100% 的V |

### 退出支付

当矿工选择退出平台时，他可以发送一个退出交易并在冷却期 $\delta$ 后收到他的退出支付，我们建议冷却期 $\delta$ 为 7 天。

冷却期过后，矿工可以拿到最后一笔支付，取回最初的抵押额。当 $V_T$ 比最初的 $V^e$ 更低时，矿工受到的惩罚会影响到抵押额：

$$w(T + \sigma) = \min(\frac{V_T}{V^e}, 100\%) \cdot S$$

其中 $S$ 为初始抵押。
