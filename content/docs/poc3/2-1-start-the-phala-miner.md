---
title: "2.1 Start the Phala miner"
---

## Start the Phala miner

```bash
sudo phala start
```
> The scripts will run for a long time.The pruntime and phost will start until the full node got the highest block.

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
