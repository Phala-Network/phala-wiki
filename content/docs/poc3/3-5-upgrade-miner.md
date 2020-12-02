---
title: "3.5 Upgrade Miner"
---

## Stop mining

> If your miner is not in mining status, feel free to skip this step.

1. Refer to [this link]({{< relref "docs/poc3/3-3-stop-mining" >}}) to stop mining.
2. Wait until the end of the current mining period (1 hour per mining period). If you stop the miner too early, you may get slashed.
3. Run the following commands to kill the original containers:

    ```bash
    docker kill phala-phost
    docker kill phala-pruntime
    docker kill phala-node
    ```

## Purge the docker containers and local files

Run the commands below to remove cached data. By default, the Phala node data is stored under `$HOME/phala-node-data` and the pruntime cache is stored at `$HOME/phala-pruntime-data`. If the path was changed in registration, please replace it with your path.

```bash
sudo rm -r $HOME/phala-node-data
sudo rm -r $HOME/phala-pruntime-data
sudo docker image prune -a
```

## Pull the new docker image

```bash
sudo docker pull phalanetwork/phala-poc3-node
sudo docker pull phalanetwork/phala-poc3-pruntime
sudo docker pull phalanetwork/phala-poc3-phost
```

## Bring up the full stack

Please re-do [2-1 Deploy the Full Stack]({{< relref "docs/poc3/2-1-deploy-the-full-stack" >}})

### Tech Support
[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zjdJ7d844d) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)
