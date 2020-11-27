---
title: "3.5 Upgrade Miner"
---

## Stop Mining
> If your miner is not in mining status, please start the upgrade from step 2.

1. Refer to [this link]({{< relref "docs/poc3/3-3-stop-mining" >}}) to stop mining.
2. Wait until the end of the current mining period (1 hour per mining period). If you stop the miner too early, you may get slashed.
3. Run the following commands to kill the original containers:

    ```bash
    docker kill phala-phost
    docker kill phala-pruntime
    docker kill phala-node
    ```

## Reset Docker Containers

1. Run the commands below to remove cached data. By default, the Phala node data is stored under `$HOME/phala-node-data`. If the path was changed in registration, please replace it with your path.

    ```bash
    sudo rm -r $HOME/phala-node-data
    sudo rm -r $HOME/phala-pruntime-data
    sudo docker image prune -a
    ```

## Reclaim test tokens
> From this step, the following operations are literally the same as when you register a new miner. You may refer to relevant pages for more details.

There's no needs to burn more PHA. You can claim test tokens with the TXHash before by following steps below:

1. Go to <https://etherscan.io/> (or any other Ethereum explorer);
2. Search your ETH account and find the TXHash of PHA swap before;
3. Go to <https://poc3-swap.phala.network/>
4. Click `Claim Tokens`, paste the TXHash at the first line, your testnet address at the second line;
5. Click `Click to Claim`.

You may also follow these steps if there's an error when you burn PHA to claim test tokens.

## Reset Miner Stash
Go to `Developer` → `Extrinsics` → choose your Stash account → choose `phalaModule` → choose `SetStash(controller)`. And click `Submit Transaction`.

## Pull and Run New Docker Containers
Pull:
```bash
sudo docker pull phalanetwork/phala-poc3-node
sudo docker pull phalanetwork/phala-poc3-pruntime
sudo docker pull phalanetwork/phala-poc3-phost
```
Run:
```bash
sudo docker run -ti --rm --name phala-node -d -e NODE_NAME=YOUR_NODE_NAME -p 9933:9933 -p 9944:9944 -p 30333:30333 -v $HOME/phala-node-data:/root/data phalanetwork/phala-poc3-node
sudo docker run -d -ti --rm --name phala-pruntime -p 8000:8000 -v $HOME/phala-pruntime-data:/root/data --device /dev/isgx phalanetwork/phala-poc3-pruntime
# wait for 30s after started the full node and pRuntime
sudo docker run -d -ti --rm --name phala-phost -e PRUNTIME_ENDPOINT="http://IP-ADDRESS:8000" -e PHALA_NODE_WS_ENDPOINT="ws://IP-ADDRESS:9944" -e MNEMONIC="THE-MNEMONIC-OF-YOUR-CONTROLLER" -e EXTRA_OPTS="-r" phalanetwork/phala-poc3-phost
```

## On-chain Operations
To set your mission and miner stash, check miner status, or start mining and check on-chain profits, please refer to relevant pages.

<br>

### Tech Support
[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zjdJ7d844d) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)
