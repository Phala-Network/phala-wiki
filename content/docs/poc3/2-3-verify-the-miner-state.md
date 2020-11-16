---
title: "2.3 Veirfy the Miner State"
---

Go to `Developer` -> `Chain state` :

- Check miner score: `phalaModule` - >`workerState` -> choose your stash account
    - `score.overallScore`: the total points that a miner scores
    - `features`: number of enabled cores, and the level of your cores（1-4）.
- Check stash state: `stashState`
- Confirm the binding between stash and controller addresses: `phalaModule.stash(controller)`
- An example of all the details of mining state is shown below:

![](/images/docs/poc3/2.3.png)
