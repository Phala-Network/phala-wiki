---
title: Overview
weight: 1
draft: false
---

## Lack of Confidentiality in Blockchains

Blockchain is a kind of distributed ledger which records the transactions in an ever-growing list of blocks. Its nature of trust comes from the fact that the execution of every transaction can be verified by any user of the blockchain. Existing blockchains, such as BTC and ETH, live up to this promise in an intuitive way: they make everything public, including the transactions and the internal states of smart contracts. This brings the problem: confidential data cannot be processed by these blockchains.

Phala Network tries to tackle this challenging problem. It seeks to provide strong guarantee of *confidentiality* without sacrificing *cross-contract interoperability*, which means the confidential contracts on Phala Network can interact with other confidential contracts freely. Further, as a Polkadot parachain, Phala Network enables *cross-chain interoperability* of smart contracts to confidentially operate assets on another blockchain.

## What is a Confidential Contract

```rust
pub struct HelloWorld {
  counter: u32,
}

fn handle_command(&mut self, origin: &chain::AccountId, txref: &TxRef, cmd: Command) -> TransactionStatus {
  match cmd {
    Command::Increment { value } => {
      self.counter += value;
      TransactionStatus::Ok
    },
  }
}
```

> **Note**: Some boilerplate code was removed for simplicity

A confidential contract is nothing more than an ordinary smart contract, but with confidentiality. The above code is a snippet of a sample contract where it stores a counter and anyone can increment it but only authorized user can read it.

The Phala contracts are written in Rust, a programming language that can do anything on the blockchain. You can make full use of your favourite package manager Cargo and libraries at [crates.io](https://crates.io).

So far, we have a HelloWorld contract with a counter, but it's only the first half! Now you may wonder what is the difference between confidential contracts and other smart contracts. The second half revels the secret.

```rust
fn handle_query(&mut self, origin: Option<&chain::AccountId>, req: Request) -> Response {
  match req {
    Request::GetCount => {
      if origin != Some(ROOT_ACCOUNT) {
        Response::Error(Error::NotAuthorized)
      } else {
        Response::GetCount { count: self.counter })
      }
    }
}
```

Unlike traditional smart contracts, the states in a confidential contract is not accessible outside the contract in any way, unless via "queries", by which you can define who has the permission to access which part of the data. In this example, we only allow a special account `ROOT_ACCOUNT` to read the counter, otherwise the requester will get a `NotAuthorized` error.

In fact, in addition to the contract states, the inputs and outputs of the contract are also end-to-end encrypted. The contract developer can design how the data can be accessed in a fully flexible way.

> **Side notes: Private variable are not private on Ethereum. **
>
> Though you can define variables with "private" attribute, the data is still public on the blockchain. According to [the solidity doc](https://solidity.readthedocs.io/en/v0.7.3/contracts.html):
> > Everything that is inside a contract is visible to all observers external to the blockchain. Making something private only prevents other contracts from reading or modifying the information, but it will still be visible to the whole world outside of the blockchain.


## The Root of Trust: TEE

Phala provides confidentiality guarantee based on trusted hardwares, or *Trusted Execution Environment*, which means your code and data are safe even if your operating system is compromised. A contract executing in the TEE is just like the priest in the confessional room: You know who he is, you can tell he what you want and he will reply, but only God knows what's going on there. The most important thing is: All your secrets are safe.

Phala adopts one of the most popular implementations of TEE, i.e., Intel SGX. Intel SGX introduces a small set of instructions to encrypt the data in memory, and attackers cannot decrypt it without cracking the CPU and extracting the secret key in it. Unlike existing blockchains in which all contract states are public on chain, the states of confidential contracts are encrypted and sealed in SGX.

## Phala Network in Detail

Phala Network is consisted of three components: phala-nodes which make up of Phala blockchain, pRuntime and phost. Among them, only pRuntime lives in TEE. Although attackers cannot peek into TEE, they can trick the contracts in TEE by forging transactions or replaying/reordering valid transactions. It it important to ensure that confidential contracts only accept valid transactions and process transactions in an expected order. That's why we introduce Phala blockchain and phost.

Phala blockchain serves as a canonical source of valid transactions. Only submitted transactions can be accepted by pRuntime, and they will be processed in the same order as on blockchain. We implement the Simplified Payment Verification protocal in pRuntime so it is able to determine whether valid transactions are accepted in expected order. Also a key rotation mechanism will be introduced to prevent the replay of historical transactions. The great thing is that pRunime hides all these complicated implementation details from you so you can just implement confidential contracts like developing ordinary programs.

phost works as the bridge between Phala blockchain and pRuntime. It ensures that all the transactions on blockchain are faithfully forwarded to pRuntime and all the TEE instances are running unmodified version of pRuntime.
