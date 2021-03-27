---
title: "2.3 Verify the Miner State"
---

Go to `Developer` -> `Chain state` :

- Check miner score: `phalaModule` - >`workerState` -> choose your stash account
    - `score.overallScore`: the total points that a miner scores
    - `features`: number of enabled cores, and the level of your cores（1-4）.
- Check stash state: `stashState`
- Confirm the binding between stash and controller addresses: `phalaModule.stash(controller)`
- An example of all the details of mining state is shown below:

![](/images/docs/poc3/2.3.png)

## Mining State References

- `Empty`: A rare tag to see if you have registered your miner successfully
- `Free`: Your miner is successfully registered but haven't start mining
- `Gatekeeper`: Your Gatekeeper role is enabled
- `MiningPending`: Your miner will start mining in the next round
- `Mining: BlockNumber`: Your miner is mining; the number aside is the blockheight where you start
- `MiningStopping`: Your miner will stop mining in the next round (state changed into `Free`). Please go offline only after your miner state is tagged as `Free`, or else you may get slashed.
