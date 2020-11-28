---
title: "3.2 Start Mining"
---

1. Go to `Develoepr` -> `Extrinsics`
2. Choose your controller account，click `phalaModule` -> `startMiningIntention()` ，and `Submit Transaction`.

![](/images/docs/poc3/3.2-1.png)

Then you will wait for the next mining period (every 1 hour) to get started.

## Mining States Verification

1. Go to `Developer` → `Chain State`
2. Choose `phalaModule` → `workerState`, and choose your stash account
3. If there's a `Mining` with block height after `state`, congratulations, your miner is working. If there's a `Miningpending`, it means your miner will start mining from the next round (600 blocks later).

![](/images/docs/poc3/3.2-2.png)

## Mining Status References

- "Empty": null -> a rare tag to see if you have registered your miner successfully
- "Free": null -> your miner is successfully registered but haven't start mining
- "Gatekeeper": null -> your Gatekeeper role is enabled
- "MiningPending": null -> your miner will start mining in the next round
- "Mining": "BlockNumber" -> your miner is mining; the number aside is the blockheight where you start
- "MiningStopping": null -> your miner will stop mining in the next round (status changed into `Free`). Please go offline only after your miner status is tagged as `Free`, or else you will get slashed.
