---
title: "Run a full node"
date: 2020-07-06T22:30:41+08:00
draft: false
---

If you're building dapps or products on a Phala Network,
you probably want the ability to run a node-as-a-back-end. After all, it's
always better to rely on your own infrastructure than on a third-party-hosted one in this brave new
decentralized world.

This guide will show you how to connect to [Phala Network PoC-2](https://poc2.phala.network). First, let's
clarify the term _full node_.

### Types of Nodes

A blockchain's growth comes from a _genesis block_, _extrinsics_, and _events_.

When a Gatekeeper seals block 1, it takes the blockchain's state at block 0. It then applies all
pending changes on top of it, and emits the events that are the result of these changes. Later, the
state of the chain at block 1 is used in the same way to build the state of the chain at block 2,
and so on. Once two thirds of the Gatekeepers agree on a specific block being valid, it is finalized.

An **archive node** keeps all the past blocks. An archive node makes it convenient to query the past
state of the chain at any point in time. Finding out what an account's balance at a certain block
was, or which extrinsics resulted in a certain state change are fast operations when using an
archive node. However, an archive node takes up a lot of disk space - around Kusama's 1.6 millionth
block this was around 15 to 20GB. When running a Gatekeeper, this requirement doubles as
[the sentry node]([maintain-guides-how-to-setup-sentry-node](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-setup-sentry-node)) in front of a Gatekeeper should be an
archive node too.

A **full node** is _pruned_, meaning it discards all information older than 256 blocks, but keeps
the extrinsics for all past blocks, and the genesis block. A node that is pruned this way requires
much less space than an archive node. In order to query past state through a full node, a user would
have to wait for the node to rebuild the chain up until that block. A full node _can_ rebuild the
entire chain with no additional input from other nodes and become an archive node. One caveat is
that if finality stalled for some reason and the last finalized block is more than 256 blocks
behind, a pruned full node will not be able to sync to the network.

Archive nodes are used by utilities that need past information - like block explorers, council
scanners, discussion platforms like [Polkassembly](https://polkassembly.io), and others. They need
to be able to look at past on-chain data. Full nodes are used by everyone else - they allow you to
read the current state of the chain and to submit transactions directly to the chain without relying
on a centralized infrastructure provider.

Another type of node is a **light node**. A light node has only the runtime and the current state,
but does not store past extrinsics and so cannot restore the full chain from genesis. Light nodes
are useful for resource restricted devices. An interesting use-case of light nodes is a Chrome
extension, which is a node in its own right, running the runtime in WASM format:
<https://github.com/paritytech/substrate-light-ui>

### Fast Install Instructions (Linux)

> Not recommended if you're a Gatekeeper. Please see
> [secure Gatekeeper setup](https://wiki.polkadot.network/docs/en/maintain-guides-secure-validator)

For the most recent binary please see the
[release page](https://github.com/Phala-Network/phala-blockchain/releases/) on the phala-blockchain repository. The URL
in the code snippet below may become slightly out-of-date.

Also please note that the nature of pre-built binaries means that they may not work on your
particular architecture or Linux distribution. If you see an error like
`cannot execute binary file: Exec format error` it likely means the binary is not compatible with
your system. You will either need to compile the [source code yourself](#clone-and-build) or use
[docker](#using-docker).

- Download Phala Network binary by running:
  `curl -sL https://github.com/Phala-Network/phala-blockchain/releases/download/poc2-2.0/phala-node -o phala-node`
- Run the following: `sudo chmod +x phala-node`
- Run the following: `./phala-node --chain poc2 --name "Your Node Name Here"`
- Find your node at <https://telemetry.polkadot.io/#list/Phala%20PoC-2>

### Fast Install Instructions (Mac)

> Not recommended if you're a Gatekeeper. Please see
> [secure Gatekeeper setup](https://wiki.polkadot.network/docs/en/maintain-guides-secure-validator)

- Type terminal in the macOS searchbar/searchlight to open the 'terminal' application
- Install Homebrew within the terminal by running:
  `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`
- Then run: `brew install openssl cmake llvm`
- Install Rust in your terminal by running:
  `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
- Once Rust is installed, run the following command to clone and build the Phala Network code:
  ```
  git clone https://github.com/Phala-Network/phala-blockchain
  cd phala-blockchain
  ./scripts/init.sh
  cargo build –-release
  ```
- Run the following command to start your node: `./target/release/phala-node --chain poc2 --name "My node's name"`
- Find your node at <https://telemetry.polkadot.io/#list/Phala%20PoC-2>

## Get Substrate

Follow instructions as outlined
[here](https://substrate.dev/docs/en/knowledgebase/getting-started) - note that Windows users will
have their work cut out for them. It's better to use a virtual machine instead.

Test if the installation was successful by running `cargo --version`.

```bash
$ cargo --version
cargo 1.41.0 (626f0f40e 2019-12-03)
```

## Clone and Build

The [Phala-Network/phala-blockchain](https://github.com/Phala-Network/phala-blockchain) repo's master branch contains the
latest Phala Network code.

```bash
git clone https://github.com/Phala-Network/phala-blockchain
cd phala-blockchain
./scripts/init.sh
git submodule update --init
cargo build --release
```

## Run

The built binary will be in the `target/release` folder, called `phala-node`.

```bash
./target/release/phala-node --chain poc2 --name "My node's name"
```

Use the `--help` flag to find out which flags you can use when running the node. For example, if
[connecting to your node remotely](https://wiki.polkadot.network/docs/en/maintain-wss), you'll probably want to use `--ws-external` and
`--rpc-cors all`.

The syncing process will take a while depending on your bandwidth, processing power, disk speed and
RAM. On a \$10 DigitalOcean droplet, the process can complete in some 36 hours.

Congratulations, you're now syncing with Phala Network. Keep in mind that the process is identical when
using any other Substrate chain.

## Running an Archive Node

When running as a simple sync node (above), only the state of the past 256 blocks will be kept. When
validating, it defaults to [archive mode](#types-of-nodes). To keep the full state use the
`--pruning` flag:

```bash
./target/release/phala-node --chain poc2 --name "My node's name" --pruning archive
```

It is possible to almost quadruple synchronization speed by using an additional flag:
`--wasm-execution Compiled`. Note that this uses much more CPU and RAM, so it should be turned off
after the node is in sync.

## Using Docker

Finally, you can use Docker to run your node in a container. Doing this is a bit more advanced so
it's best left up to those that either already have familiarity with docker, or have completed the
other set-up instructions in this guide.

The reference docker file is located [here](https://github.com/Phala-Network/phala-blockchain/blob/master/Dockerfile).
Please note that by default the docker file only spawn a two nodes local testnet node, not a single
node connecting to the public blockchain. This is mainly used for development. You can create your
own docker file for connecting to Phala Network PoC2.

## Participate Testnet PoC-2 promotion events

To participate the PoC-2 events, it's required to set your node name in the format below:

```bash
./phala-node --name "Name | Controller account address" <...other arguments>
```

where "Controller account address" is the address of your Controller account for staking (not Stash account). Please make sure there are two spaces close to the vertical bar symbol.

> Example: the command below brings up an archive node with the name "PhalaMiner" and its controller account
>
> ```bash
> ./phala-node --name "PhalaMiner | 5Ea32SkcVaEmBVFNeMycjuAQKNzHzwosFrhEhwUFmawsEtkt" --pruning archive
> ```
