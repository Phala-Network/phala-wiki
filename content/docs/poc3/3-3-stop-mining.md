---
title: "3.3 Stop Mining"
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

1. Go to `Developer` → `Extrinsics` 
2. Choose your controller account，click`phalaModule` → `stopMiningIntention()` → `Submit Transaction`

Then you will wait for the next mining period (every 1 hour) to get stopped.

![](/images/docs/poc3-old/3.3.png)


## Miner State Verification onchain

1. Go to`Developer` → `Chain State`
2. `phalaModule` → `workerState` → Stash account
3. `phalaModule` → `miningState` → Stash account

## Miner State Verification

```bash
sudo phala status
```

## Stop the Phala miner

You can stop your phala miner as this command:
```bash
sudo phala stop
```
## Uninstall the Phala scripts

You can Uninstall the Phala scripts as this command:
```bash
sudo phala uninstall
```