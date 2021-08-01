---
title: "2.1 Start the Phala miner"
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

## Start the Phala miner

```bash
sudo phala start
```
> The scripts will run for a long time. The pruntime and phost will start until the full node got the highest block.

It costs a tiny amount of tPHA as well. Before deploying pHost, please make sure there are a certain amount of tPHA in your Stash account and controller account. [Click here to learn how to obtain tPHA.](https://forum.phala.network/t/how-to-obtain-tpha-on-testnet-vendetta/1254)

## Check the phala miner status

You can check your miner status as this command：

```bash
sudo phala status
```

There will be 3 dockers running. You can restart the docker as this command:
```bash
sudo phala start phost
sudo phala start pruntime
sudo phala start node
```
