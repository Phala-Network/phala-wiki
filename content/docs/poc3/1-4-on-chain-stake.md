---
title: "1.4 On-Chain Staking"
---

## Over Staking for Computing Task


### Process of Staking


![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_8cdb37eca35da85848516b8277137c38.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532366&Signature=9QPP%2BYrpUQht%2FVOyuRdFP2Ru9S0%3D)


1. Stake
Transfer to your stath acount , nominator your or other gatekeeper , the token will be locked in this round and become effective in next round.
2. Withdraw Stake
The token will be pre-unlocked , and be unlocked in next round.


### How to Operating


1. Ready to work

Open the following two pages and select the corresponding transaction or query



- Open [https://poc3.phala.network/polkadotjs/?rpc=wss%3A%2F%2Fhashbox-lan.corp.phala.network%2Fws1#/extrinsics](https://poc3.phala.network/polkadotjs/?rpc=wss%3A%2F%2Fhashbox-lan.corp.phala.network%2Fws1#/extrinsics) ，select the stath account，select “miningStaking” in "submit the following extrinsic"。this page is used for extrinsics.

![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_523d31eb814054719eb67e403c188932.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532416&Signature=AvnelAcqtLnxoyNBXeN7U4PNwQo%3D)

- Open [https://poc3.phala.network/polkadotjs/?rpc=wss%3A%2F%2Fhashbox-lan.corp.phala.network%2Fws1#/chainstate](https://poc3.phala.network/polkadotjs/?rpc=wss%3A%2F%2Fhashbox-lan.corp.phala.network%2Fws1#/chainstate) ，select “miningStaking” in "selected state query"，and select the stath account。this page is used for inquire state.

![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_9c53cf4efb0ce86261dd6b511d3ac22c.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532347&Signature=Udwm9vr7oTDrQ1rQvtkbbap8dVM%3D)


1. Deposit



- 2.1. Extrinsics
   - deposit(value)
   - value: BalanceOf：Input the balance of staking
   - Submit Translation
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_d5d98fc003c499800fd946bb3452d5be.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532444&Signature=8sFzatfOfN1fzEl%2FkDTAlAHIYAs%3D)
- 2.2. Inquire
   - wallet(AccountId): BalanceOf
   - Make sure the acount is deposit account
   - press "+" 
   - Check the balance
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_75fd544792243ca014c1d088b0584155.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532462&Signature=3URFHjNDw2YpSeare7iZtCJdmIs%3D)



1. Staking



- 3.1. Translation
   - stake(to, value)
   - to: AccountId：You can stake for yourself or other
   - value: BalanceOf：Input he balance of stake 
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_66bff68d8f2822be3eeb063cf4f6f261.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532477&Signature=AiaVybFcq7OB8t7DQaGQW0%2FrVbQ%3D)
- 3.2. Inquire
   - walletLocked(AccountId): BalanceOf
   - Make sure the acount is deposit account
   - Press "+"
   - Check the locked token , it is the stake token
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_bff1c899b4826641b0b26ba4006a9562.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532499&Signature=wq52GtaG0EwNhv79lUbh0igzomE%3D) 
It will be stake in next round

   - stakeReceived(AccountId): BalanceOf
   - Make sure the acount is stake account
   - Press “+” 
   - Check the staking blance
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_3236638962cbb72eb49c888e47c73c19.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532511&Signature=7u3%2FVTokwem1cT%2FV50B6roLVsks%3D)



4. Release stake

- 4.1. Translation
   - unstake(to, value)
   - to: AccountId：Make sure the account
   - value: BalanceOf：Input the release balance
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![image.png](https://cdn.nlark.com/yuque/0/2021/png/696808/1612511021298-3a8549a5-c674-4621-87cd-5ab1852a0995.png#align=left&display=inline&height=430&margin=%5Bobject%20Object%5D&name=image.png&originHeight=430&originWidth=1427&size=229237&status=done&style=none&width=1427)
- 4.2. Inquire
   - pendingUnstaking(AccountId, AccountId): BalanceOf
   - Make sure the account
   - Press “+”
   - Check the balance
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_247b26da595668e26c2d97ca5110b963.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532528&Signature=tPnkIK%2B0mmowG80oiS6I3316NfY%3D)
It will be disstake in next round (less than 1 hour)
   - wallet(AccountId): BalanceOf
   - Make sure the account
   - Press “+”
   - Check the balance in wallet
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_04fba27f7514433fee19377e9ff15d37.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532547&Signature=cQY4lOuNDOCAm8hQ5pPT%2BDbgC%2Bs%3D)



5. Withdraw



- 5.1. Translation
   - withdraw(value)
   - value: BalanceOf：Input the balance you want withdraw
   - Sign and Submit
   - After a few seconds in the upper right corner, it shows "in block", which means success
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_674aae01ffb6657cd678c48154f377c1.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532593&Signature=cOKQVJrIo%2FvQ6q8wsVGJWUGryEk%3D)
- 5.2. Inquire
   - wallet(AccountId): BalanceOf
   - Make sure the account is the withdraw account
   - Press “+”
   - Check the blance
![image.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_75fd544792243ca014c1d088b0584155.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1612532583&Signature=nbbGg8%2B8p3eb3B2bvFUBKO9lopY%3D)
