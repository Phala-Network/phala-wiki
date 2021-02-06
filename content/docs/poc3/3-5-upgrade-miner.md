---
title: "3.5 Upgrade Miner"
---

## Stop mining

> If your miner is not in mining status, feel free to skip this step.

1. Refer to [this link]({{< relref "docs/poc3/3-3-stop-mining" >}}) to stop mining.
2. Wait until the end of the current mining period (1 hour per mining period). If you stop the miner too early, you may get slashed.
3. Run the following commands to kill the original containers:

    ```bash
    sudo phala stop
    ```

## Update the Phala miner and clean the local files

Run the commands below to remove cached data.

```bash
sudo phala update clean
```

## Update the Phala miner without clean the local files

```bash
sudo phala update
```

## Restart the Phala miner

Please re-do [2-1 Start the Phala miner]({{< relref "docs/poc3/2-1-start-the-phala-miner" >}})

### Miner Community
[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zjdJ7d844d) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)
