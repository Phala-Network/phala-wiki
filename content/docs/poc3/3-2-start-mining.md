---
title: "3.2 Start Mining"
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

1. Go to `Develoepr` -> `Extrinsics`
2. Choose your controller account，click `phalaModule` -> `startMiningIntention()` ，and `Submit Transaction`.

![](/images/docs/poc3-old/3.2-1.png)

Then you will wait for the next mining period (every 1 hour) to get started.

## Mining States Verification

1. Go to `Developer` → `Chain State`
2. Choose `phalaModule` → `workerState`, and choose your stash account
3. If there's a `Mining` with block height after `state`, congratulations, your miner is working. If there's a `Miningpending`, it means your miner will start mining from the next round (600 blocks later).

![](/images/docs/poc3-old/3.2-2.png)

## Mining Status References

- "Empty": null -> a rare tag to see if you have registered your miner successfully
- "Free": null -> your miner is successfully registered but haven't start mining
- "Gatekeeper": null -> your Gatekeeper role is enabled
- "MiningPending": null -> your miner will start mining in the next round
- "Mining": "BlockNumber" -> your miner is mining; the number aside is the blockheight where you start
- "MiningStopping": null -> your miner will stop mining in the next round (status changed into `Free`). Please go offline only after your miner status is tagged as `Free`, or else you will get slashed.
