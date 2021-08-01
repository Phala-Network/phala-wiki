---
title: "2.2 Set Commission"
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

1. Go to `Developer` -> `Extrinsics`.
2. Choose your `Controller` account -> `phalaModule` -> `setPayoutPrefs(payout_comission, payout_target)`
3. Fill in `commission` (from 0 to 100) and choose the account where you want to receive the mining rewards at `target`.

> The commission rate influences your final profits. For any mining reward you get, you always take the commission from it, and then distribute the rest propotionally to the mining stakers. For example, a `comission` of 100 means all the profits are tranferred to your specified account and a `comission` of 0 will share all the profits to your mining stakers. Of course you get all if it's all your own stake.
>
> A lower commission rate will share more of your profits, which means more lenders are likely to support you, thus lowering your staking amount requirement.

![](/images/docs/poc3-old/2.2.png)
