---
title: "3.3 Stop Mining"
---

1. Go to `Developer` → `Extrinsics` 
2. Choose your controller account，click`phalaModule` → `stopMiningIntention()` → `Submit Transaction`

Then you will wait for the next mining period (every 1 hour) to get stopped.

![](/images/docs/poc3/3.3.png)


## Miner State Verification

1. Go to`Developer` → `Chain State`
2. `phalaModule` → `workerState` → Stash account
3. `phalaModule` → `miningState` → Stash account

## Stop the Phala miner

You can stop your phala miner as this command:
```bash
sudo phala stop
```
## Uninstall the Phala scripts

You can Uninstall the Phala scripts as this command:
```bash
sudo ./uninstall.sh
```