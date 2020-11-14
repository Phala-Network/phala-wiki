---
title: "2.2 Set Commission"
---

1. Go to `Developer` -> `Extrinsics`.
2. Choose your `Controller` account -> `phalaModule` -> `setPayoutPrefs(payout_comission, payout_target)`
3. Fill in `commission` (from 0 to 100) and choose the account where you want to receive the mining rewards at `target`. 

> The commission rate influences your final return. For any mining reward you get, you always take the commission from it, and then distribute the rests propotionally to the mining stakers. Of course you get all if it's all your own stake.
>
> A lower commission rate will share more of your profits, which means more lenders are likely to support you and lower your staking amount.

![](/images/docs/poc3/2.2.png)
