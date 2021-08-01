---
title: "1.2 Bind Stash and Controller accounts"
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

1. **Create two Phala accounts**

    * Reference https://hackmd.io/1U7uIEb-RD6bakEKmBlJzg to create a Phala account
    * To help identify the relationship between accounts later, we recommend that you add "Stash" and "Controller" to the account names.

2. **Get Test Currency**

    Join our [testnet faucet Telegram group](https://t.me/phalafaucet) to get some free test coins.

3. **Set the binding relationship between stash and controller**

    Developer Tab → Extrinsics (transaction) → select your Stash account → select phalaModule → choose setStash(controller) -> select your Controller account.

4. Click **Submit Transaction**, sign and wait for broadcast.

    ![](/images/docs/poc3/1-2.jpg)
