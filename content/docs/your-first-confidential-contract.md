---
title: "Hello World: your first confidential contract"
weight: 3
---

> Basic understanding of Rust language programming and smart contract development knowledge is necessary to follow this tutorial.

## Overview

In this tutorial, we are going to continue on the development environment we have set up in the previous chapter, and explore how a confidential smart contract is made. By the end of this tutorial, you will:

- Learn how to develop a confidential contract
- Interact with the contract from the Web UI
- Build your own confidential contract

For a high-level overview of Phala Network, please check the previous chapters.

## Environment and Build

Please set up a development environment by following the previous chapter [Run a Local Development Network](Run-a-Local-Development-Network). Make sure you are at the `helloworld` branch on both `phala-blockchain` and `apps-ng` repo.

## Walk-through

<!-- > More details will be developed soon. -->

### Contract

The HelloWorld contract commit is available at <https://github.com/Phala-Network/phala-blockchain/commit/b9c576702ec1b9f0ee4a274b5d7aecc30e40fcb4>. HelloWorld contract stores a counter which can be incremented by anyone, but only authorized user can read it. The typical model of the confidential contracts in Phala Network is consisted of the following three components which we will discuss in detail.

- States
- Commands
- Queries

The **States** of a contract is described by certain variables. In this case, we define a 32-bit unsigned variable as the counter, while you are free to use variables of any types in your contracts.

```rust
pub struct HelloWorld {
    counter: u32,
}
```

There are two kinds of requests which can be used to interact with confidential contracts: **Commands** and **Queries**. The most significant difference between them is whether or not they change the states of the contracts, and we explain them separately.

The **Commands** are supposed to change the states of contracts. They are also called **Transactions**, and they are just like the transactions on traditional smart contract platforms like Ethereum: they must be posted to blockchain first before their executions. In our HelloWorld contract, we define a `Increment` command which changes the value of counter.

```rust
pub enum Command {
    /// Increments the counter in the contract by some number
    Increment {
        value: u32,
    },
}
```

It is worth noting that you can define more than one commands for a contract. For example, we can add a `Decrement` command to decrease the counter as follow.

```rust
pub enum Command {
    /// Increments the counter in the contract by some number
    Increment {
        value: u32,
    },
    /// Decrements the counter in the contract by some number
    Decrement {
        value: u32,
    },
}
```

All the commands are processed by the `handle_command` method which must be implemented. In this case, we allow any user to use this command, so we just increase the counter without checking the `_origin`.

```rust
fn handle_command(&mut self, _origin: &chain::AccountId, _txref: &TxRef, cmd: Command) -> TransactionStatus {
    match cmd {
        // Handle the `Increment` command with one parameter
        Command::Increment { value } => {
            // Simply increment the counter by some value.
            self.counter += value;
            // Returns TransactionStatus::Ok to indicate a successful transaction
            TransactionStatus::Ok
        },
    }
}
```

Opposed to commands, **Queries** shall not change the states of contacts. Queries are one of the innovations of Phala Network. They are designed to allow a quick examination of the states of contracts. To define a query, you need to define both the `Request` and the according `Response`.

```rust
pub enum Request {
    /// Ask for the value of the counter
    GetCount,
}

/// Query responses.
pub enum Response {
    /// Returns the value of the counter
    GetCount {
        count: u32,
    },
    /// Something wrong happened
    Error(Error)
}
```

`handle_query` method is supposed to handle all the queries. Unlike commands, Queries go directly to the contracts without the necessity to be posted to blockchain. That's why we recommend to introduce an authority check here to avoid potential Denial-of-Service attack with a huge amount of query requests.

```rust
fn handle_query(&mut self, _origin: Option<&chain::AccountId>, req: Request) -> Response {
    let inner = || -> Result<Response, Error> {
        match req {
            // Handle the `GetCount` request.
            Request::GetCount => {
                // Respond with the counter in the contract states.
                Ok(Response::GetCount { count: self.counter })
            },
        }
    };
    match inner() {
        Err(error) => Response::Error(error),
        Ok(resp) => resp
    }
}
```


### Frontend

- Web UI commit: <https://github.com/Phala-Network/apps-ng/commit/4a806c8e49edb8f12bd5ed54d1700edf81a6af56>

Interact with the contract: how to send command and queries.

## Implement a secret notebook

### Contract

1. Add an item to contract state: `map<account, string>`
2. Command: `SetNote`
3. Query: `GetNote` with `Error::NotAuthorized`

### Frontend

1. Set note UI
2. Query UI
3. Handle error

## Put everything together

## Summary

### Submit your work

This tutorial is a part of [Polkadot "Hello World" virtual hackathon challenge](https://gitcoin.co/hackathon/polkadot/onboard) at gitcoin.co. In order to win the task, please do the followings:

1. Fork [the core blockchain](https://github.com/Phala-Network/phala-blockchain/tree/helloworld) and [the Web UI](https://github.com/Phala-Network/apps-ng/tree/helloworld) repo (helloworld branch) into your own GitHub account
2. Develop your own contract on the templates at “helloworld” branch
3. Launch your full development stack and take screenshots of your dapps
4. Push your work to your forked repos. They must be open source
5. Make a tweet with the link to your repos, the screenshots, and describe what you are building on Twitter
6. Join our [Discord server](https://discord.gg/zQKNGv4) and submit the the link to your tweet
