---
title: "3.2 Start Mining"
---

1. Go to `Develoepr` -> `Extrinsics` 
2. Choose your controller account，click `phalaModule` -> `startMiningIntention()` ，and `Submit Transaction`.

Then you will wait for the next mining period (every 1 hour) to get started.

![](https://imgur.com/f0mahMF.png)


## Mining States Verification

1. Go to `Developer` → `Chain State`
2. Choose `phalaModule` → `workerState`, and choose your stash account
3. If there's a `Mining` with block height after `state`, congratulations, your miner is working. If there's a `Miningpending`, it means your miner will start mining from the next round (600 blocks later). 

![](https://imgur.com/N8V73i6.png)
