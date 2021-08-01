---
title: 1.6 Introduction of 1605 Race Privacy Computing Task
---

{{< tip "warning" >}}

**⚠️⚠️ Out-of-date Warning ⚠️⚠️**

The Phala team has published the new [Tokenomics v0.9](https://medium.com/phala-network/reading-phala-network-economic-paper-preview-5f33b7019861) in July 2021. We are working on massive refactor and upgrade of the core blockchain and the mining protocol to adopt the latest Tokenomic v0.9 and the Polkadot parachain architecture. When the development is ready, we are going to launch a new testnet (Para1).

The running testnet (poc4-dev) and runtime-bridge are still based on the old mining protocol. The difference between the new one and the old one is significant. Some frequentely asked questions are already no-issue on the new testnet. Therefore we storngly suggest you to wait for the new testnet launch. The runtime-bridge will be available shortly after that.

You can find the latest development progress from the links below:

1. [Phala/Khala Core Protocol Progress](https://github.com/orgs/Phala-Network/projects/9)
2. [Mining Protocol Progress](https://github.com/orgs/Phala-Network/projects/8)
3. [runtime-bridge Github repo](https://github.com/Phala-Network/runtime-bridge)

{{< /tip >}}


## Introduction of 1605 Race Privacy Computing Task
Who are familiar with Phala Tokenomics know that Phala's mining rewards are divided into online rewards and computing task rewards. In this 1605 Race V2, on the basis of online rewards, the emphasis is on the introduction of computing task rewards. This article is an introduction to the computing tasks of the Vendetta testnet.

### Background summary
1. The computing tasks in this testnet are generated virtually by the system, and 5 computing tasks are fixed for each Round (600 blocks). The amount of mining rewards is consistent with the white paper. Among them, 50% of the rewards for each block mined will be obtained by TEE workers that perform private computing tasks, 30% of the rewards will be obtained by all online TEE workers, and 20% will flow into the treasury through the on-chain parliament for democratic governance；
2. In Vendetta testnet, the system automatically stake the minimum stake amount for each CPU, that is, 1620 tPHA per core; miners can use tPHA as additional stake for their own CPU, and anyone can use their own tPHA as stake for other TEE workers, but At the time of settlement, it does not support automatic distribution to the nominator；
3. Within 1 Round, 1 TEE worker can divide up to 1 computing task；
4. The algorithm and parameters of computing task distribution in the testnet are experimental and will be upgraded after the mainnet goes online; the settlement method of rewards may also change.

Code reading：[Github](https://github.com/Phala-Network/phala-blockchain/blob/master/pruntime/enclave/src/system/comp_election.rs)
### Privacy computing task dispatch algorithm
#### Core logic
![](/images/docs/poc3-old/1.5-1.png)
- As with any dispatch algorithm, we need to assign the **appropriate** TEE worker to complete the computing based on the characteristics of the privacy computing task；
- Therefore, the Phala Network system will score according to the core requirements of computing characteristics and the characteristics of each TEE device to obtain the task score of each TEE worker；
- According to the task score of all online TEE workers, the assignment result is calculated by weighted random sampling formula;
- After the assigned TEE completes the privacy computing task, the system will automatically settle and issue rewards. To ensure system security, rewards will be frozen for a period of time. (The reward issued by the Vendetta testnet is **Fire II**, which is only used for the statistics of the prize pool settlement and cannot be transferred)
#### TEE worker task score
1. The probability that a TEE worker is assigned a computing task mainly depends on two characteristics of the TEE worker: computing power and security
    - The computing power is evaluated by the CPU performance score. In the future, it will be implemented through on-chain voting-no fork upgrade method；
    - Security is related to single CPU mortgage amount
2. The increase in computing power and security does not increase the probability of TEE workers obtaining private computing tasks linearly
3. According to the core indicators of TEE calculation and dispatch, we calculated the task score of each TEE assigned to the computing task
The formula for calculating task score is as follows：

![](/images/docs/poc3-old/1.5-2.png

- **Score** = TEE Calculation performance score
- **Stake** = Stake amount of TEE-Base Stake (1620*core)

#### Weighted random sampling without replacement
We use weighted random sampling without replacement，which means that each TEE worker can only be sampled once in each Round,Randomly select 5 TEE workers from all online TEE workers to perform computing tasks.

Weighting means that the task score of each TEE worker **w** is used as the basic value in the sampling. The higher **w** means the higher the probability of winning.

An example of weighted random sampling without replacement:
>Suppose there are three machines A, B, and C
>Among them, A's task score is 3, B's task score is 2, C's task score is 1
>Now draw one from 3 machines, the probability that A will be drawn is

![](/images/docs/poc3-old/1.5-3.png)

>If two sets are drawn, the probability of A being drawn is

![](/images/docs/poc3-old/1.5-4.png)

In the real environment, there may be 5 out of thousands or tens of thousands of TEEs, but the logic is consistent with the above example

In order to allow TEE miners to predict their own probability of winning and to simulate the relationship between the amount of stake tPHA and the probability of winning, we will provide a privacy computing probability calculator on Dashboard. Miners can fill in their own machine performance score and expected stake amount to simulate the probability of order dispatch.

## Data simulation of dispatching algorithm
#### The correlation between the amount of extra stake and the probability of selection

The algorithm is clear, but based on the algorithm alone, it is impossible to calculate the probability of a single mining machine being drawn, because the probability of drawing is related to the number of mining machines currently online and their task score. We simulated the increase in the probability of winning by the additional stake amount.

Assume that Rorschach has a 300 score TEE (red line in the figure). Assume that the other TEEs are 5000 420-score machines and have an additional 1000 tPHA stake. If 5 mining machines are selected from 5001 (including Rorschach's) to perform computing tasks, then as Rorschach adds additional stake, the probability of being drawn increases as follows
![](/images/docs/poc3-old/1.5-5.png)
    It can be seen from the figure:
    - The probability that 5 of more than 1,000 machines will be selected is very low, about 0.05%;
    - As the amount of stake increases, the probability increases rapidly at first, and then slowly;
    - But even if the mortgage amount increased to 20,000, the winning rate only increased to 0.17%.

#### Correlation between machine performance and selection probability

In the above figure, the light blue, red, yellow, and green machine points are 400, 300, 200, and 100 score respectively. It can be seen that the machine performance maintains a constant absolute advantage in the probability of winning

#### How to allocate additional mortgage for multiple miners

Assuming that Rorschach has two identical mining machines, when the total stake amount is the same, how to allocate the stake has little effect on the overall income. Even the split will be a bit more than just one of them.
![](/images/docs/poc3-old/1.5-6.png)

The above is all about the privacy computing tasks in 1605 Miner race V2, welcome to [forum](https://forum.phala.network/) to have more discussions with us~
