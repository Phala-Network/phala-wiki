---
title: "2.3 Verify the Miner State"
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

Go to `Developer` -> `Chain state` :

- Check miner score: `phalaModule` - >`workerState` -> choose your stash account
    - `score.overallScore`: the total points that a miner scores
    - `features`: number of enabled cores, and the level of your cores（1-4）.
- Check stash state: `stashState`
- Confirm the binding between stash and controller addresses: `phalaModule.stash(controller)`
- An example of all the details of mining state is shown below:

![](/images/docs/poc3-old/2.3.png)

## Mining State References

- `Empty`: A rare tag to see if you have registered your miner successfully
- `Free`: Your miner is successfully registered but haven't start mining
- `Gatekeeper`: Your Gatekeeper role is enabled
- `MiningPending`: Your miner will start mining in the next round
- `Mining: BlockNumber`: Your miner is mining; the number aside is the blockheight where you start
- `MiningStopping`: Your miner will stop mining in the next round (state changed into `Free`). Please go offline only after your miner state is tagged as `Free`, or else you may get slashed.
