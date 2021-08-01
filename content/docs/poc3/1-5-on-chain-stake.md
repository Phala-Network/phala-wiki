---
title: "1.5 On-Chain Staking"
---

{{< tip "warning" >}}

**⚠️⚠️ Out-of-date Warning ⚠️⚠️**

The Phala team has published the new [Tokenomics v0.9](https://medium.com/phala-network/reading-phala-network-economic-paper-preview-5f33b7019861) in July 2021. We are working on massive refactor and upgrade of the core blockchain and the mining protocol to adopt the latest Tokenomic v0.9 and the Polkadot parachain architecture. When the development is ready, we are going to launch a new testnet (Para1).

The running testnet (poc4-dev) and runtime-bridge are still based on the old mining protocol. The difference between the new one and the old one is significant. Some frequentely asked questions are already no-issue on the new testnet. Therefore we storngly suggest you to wait for the new testnet launch. The runtime-bridge will be available shortly after that.

You can find the latest development progress from the links below:

1. [Phala/Khala Core Protocol Progress](https://github.com/orgs/Phala-Network/projects/9)
2. [Mining Protocol Progress](https://github.com/orgs/Phala-Network/projects/8)
3. [runtime-bridge Github repo](https://github.com/Phala-Network/runtime-bridge)

{{< /tip >}}

## Over Staking for Computing Task

### Process of Staking

![](/images/docs/poc3-old/1.4-1.png)

1. Stake
Transfer to your stash account, nominator your or other miner, the token will be locked in this round and become effective in next round.
2. Withdraw Stake
The token will be pre-unlocked, and be unlocked in next round.

### How to Operating

1. Ready to work

Open the following two pages and select the corresponding transaction or query

- Select the stath account，select “miningStaking” in "submit the following extrinsic".this page is used for extrinsics.

![](/images/docs/poc3-old/1.4-2.png)

- Select “miningStaking” in "selected state query"，and select the stath account. This page is used for inquire state.

![](/images/docs/poc3-old/1.4-3.png)

1. Deposit

- 2.1. Extrinsics
   - deposit(value)
   - value: BalanceOf：Input the balance of staking
   - Submit Translation
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![](/images/docs/poc3-old/1.4-4.png)
- 2.2. Inquire
   - wallet(AccountId): BalanceOf
   - Make sure the acount is deposit account
   - press "+" 
   - Check the balance
![](/images/docs/poc3-old/1.4-5.png)

1. Staking

- 3.1. Translation
   - stake(to, value)
   - to: AccountId：You can stake for your or other's stash account.
   - value: BalanceOf：Input he balance of stake 
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![](/images/docs/poc3-old/1.4-6.png)
- 3.2. Inquire
   - walletLocked(AccountId): BalanceOf
   - Make sure the acount is deposit account
   - Press "+"
   - Check the locked token, it is the stake token
![](/images/docs/poc3-old/1.4-7.png)
It will be stake in next round
   - stakeReceived(AccountId): BalanceOf
   - Make sure the acount is stake account
   - Press “+” 
   - Check the staking blance
![](/images/docs/poc3-old/1.4-8.png)

4. Release stake

- 4.1. Translation
   - unstake(to, value)
   - to: AccountId：Make sure the account
   - value: BalanceOf：Input the release balance
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![](/images/docs/poc3-old/1.4-9.png)
- 4.2. Inquire
   - pendingUnstaking(AccountId, AccountId): BalanceOf
   - Make sure the account
   - Press “+”
   - Check the balance
![](/images/docs/poc3-old/1.4-10.png)
It will be disstake in next round (less than 1 hour)
   - wallet(AccountId): BalanceOf
   - Make sure the account
   - Press “+”
   - Check the balance in wallet
![](/images/docs/poc3-old/1.4-11.png)

5. Withdraw

- 5.1. Translation
   - withdraw(value)
   - value: BalanceOf：Input the balance you want withdraw
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![](/images/docs/poc3-old/1.4-12.png)
- 5.2. Inquire
   - wallet(AccountId): BalanceOf
   - Make sure the account is the withdraw account
   - Press “+”
   - Check the blance
![](/images/docs/poc3-old/1.4-13.png)
