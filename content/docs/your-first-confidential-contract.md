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

> More details will be developed soon.

### The contract

Hello World contract commit: <https://github.com/Phala-Network/phala-blockchain/commit/b9c576702ec1b9f0ee4a274b5d7aecc30e40fcb4>

- States
- Commands
- Queries

### The frontend

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
