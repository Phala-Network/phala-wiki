---
title: "运行全节点"
date: 2020-07-06T22:30:41+08:00
draft: false
---

这是一份关于如何在 [Phala PoC-2 测试网](https://app.phala.network/#/staking) 上安装全节点的教程。

如果你有节点相关疑问，请 [点此前往](https://forum.phala.network/c/9-category/9) Phala论坛中文区，参照此 [模板](https://forum.phala.network/t/topic/461/3) 进行提问。后等待Phala开发人员解答即可。



在开始跑节点之前，您可以先了解一下 Phala 网络上的节点类型。

### 节点类型

一条区块链的成长，由创世区块（genesis block）开始，由 交易（extrinsic） 和事件（event）推动。

Phala上的守门人节点（Gatekeeper）封装区块1时，会获取区块0的状态，将未确定的状态添加到新区块之上，并生成事件。随后，区块2也会如此进行封装，区块3同理。如果超过三分之二的守门人都认定一个区块是有效的，则此区块的封装完结（finalize）。如此循环往复。

这个过程将由以下几种节点参与。

- **归档节点（Archive Node）**：负责保存已经封装好的区块。因为有归档节点的存在，我们才可以很方便地查看过去的链上记录，如某个账号在某个时间点的余额，或哪个状态生成的交易是快速操作。不过，归档节点对存储空间有很高的要求。例，要归档 Kusama 网络上 160 万个区块大概需要 15 - 20 GB 的存储空间。如果你想运行一个守门人节点，则需要配置两倍于此要求的存储空间，因为每一个守门人节点同时都是归档节点。
- **全节点（Full Node）**：相比归档节点，全节点要精简许多。它不会保存第 256 个区块之前的信息，只保留包括创始区块在内的旧区块的交易。如此精简过后，一个全节点所需的存储空间就远远小于归档节点了。不过，要想在全节点上查询某个过去链上状态，用户需要等待区块链重建至某个需要查询的区块。全节点无需外部指令或变成归档节点即可重建整个链。需要注意的是，如果由于各种原因，区块的完结被阻塞在 256 个区块以前，精简过的节点就无法再与网络同步了。
- **轻节点（Light Node）**：轻节点上只有 Runtime 和实时状态，不保存任何过往交易，也不同步从创世区块开始的整条链。

归档节点对于需要经常查询过往链上信息的应用或功能来说更实用，如浏览器、议会记录（council scanner）、议事平台（如 [Polkassenbly](https://kusama.polkassembly.io/)）等。

全节点人人可用。有了自己的节点，你就可以随时查看链上状态，直接向链上提交交易，而不必经由某个中心化的节点商。

轻节点则适合设备不那么高级的用户。一种有趣的轻节点是 Google 浏览器扩展插件，一个插件就是一个节点，Runtime 以 WASM 格式运行。详情可戳链接：<https://github.com/paritytech/substrate-light-ui>

### 快速安装指南  (Linux)

> 不建议已经是守门人节点的设备继续此教程。详情请参见 [《如何安全搭建守门人节点》](https://wiki.polkadot.network/docs/en/maintain-guides-secure-validator)

你可以前往 phala-blockchain 的 [Github 发布页面](https://github.com/Phala-Network/phala-blockchain/releases/) 获取最新的可执行程序 。下方代码可能会有一点点古老。

此外，预编译的程序可能无法在老版本的 Linux 或特殊架构上运行。如果出现类似  `cannot execute binary file: Exec format error` 的错误，可能是由可执行程序版本和系统不兼容导致的。你可能需要[重新编译一遍](#克隆代码编译)，或使用 [Docker](#使用-docker-运行节点)。

- 运行 `curl -sL https://github.com/Phala-Network/phala-blockchain/releases/download/poc2-3.0-alpha1/phala-node -o phala-node` 来下载可执行程序
- 运行 `sudo chmod +x phala-node`
- 运行 `./phala-node --chain poc2 --name "你的节点名"` (如果你在参与[PoC-2活动](#参与测试网络-poc-2-活动)，请按标准格式设置名字)
- 现在，你就可以在 [Polkadot Telemetry](https://telemetry.polkadot.io/#list/Phala%20PoC-2) 上看见自己的节点了。

### 快速安装指南  (Mac)

> 不建议已经是守门人节点的设备继续此教程。详情请参见 [《如何安全搭建守门人节点》](https://wiki.polkadot.network/docs/en/maintain-guides-secure-validator)

- 在 macOS 的搜索框中输入“终端”或“terminal”来打开终端程序
- 运行 `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"` 以安装 Homebrew
- 运行 `brew install openssl cmake llvm`
- 运行 `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh` 以安装 Rust
- 然后运行以下代码以克隆 phala-blockchain 代码库并编译：
  ```
  git clone https://github.com/Phala-Network/phala-blockchain
  cd phala-blockchain
  ./scripts/init.sh
  git submodule update --init
  cargo build --release
  ```
- 运行 `./target/release/phala-node --chain poc2 --name "你的节点名"` 以启动节点 (如果你在参与[PoC-2活动](#参与测试网络-poc-2-活动)，请按标准格式设置名字)
- 现在，你就可以在 [Polkadot Telemetry](https://telemetry.polkadot.io/#list/Phala%20PoC-2) 上看见自己的节点了。

## 参与测试网络 PoC-2 活动

为了参加测试网络 PoC-2 的活动，我们要求运行节点的时候按照以下格式设置自己的名字：

```bash
./phala-node --name "名字 | Controller地址" <...其他参数>
```

其中 Controller 地址是创建抵押或者 Gatekeeper 账户时所设置的 Controller 地址（注意不是 Stash 账户）。请注意，名字与地址之间间隔的是“空格、半角竖线、空格”。如果没有按照此格式设置，可能会导致参与活动无法被统计。

> 例如：以下命令会运行一个归档节点，并设置自己的名字为 PhalaMiner
>
> ```bash
> ./phala-node --name "PhalaMiner | 5Ea32SkcVaEmBVFNeMycjuAQKNzHzwosFrhEhwUFmawsEtkt" --pruning archive
> ```

## 获取 Substrate

你可以点击 [此处](https://substrate.dev/docs/en/knowledgebase/getting-started) 查看官方教程。鉴于 Windows 系统暂时不支持 Substrate，我们建议在一个 Linux 虚拟机里运行全节点。

运行 `cargo --version` 指令以检测节点是否安装成功。

```bash
$ cargo --version
cargo 1.41.0 (626f0f40e 2019-12-03)
```

## 克隆代码、编译

[Phala-Network/phala-blockchain](https://github.com/Phala-Network/phala-blockchain) 的 Github 代码库已经包含了最新的 Phala Network 代码。

```bash
git clone https://github.com/Phala-Network/phala-blockchain
cd phala-blockchain
./scripts/init.sh
git submodule update --init
cargo build --release
```

## 运行

编译生成的程序位于 `target/release`，文件名为 `phala-node`.

```bash
./target/release/phala-node --chain poc2 --name "你的节点名"
```
你可以使用 `--help` 选项查看有哪些命令行选项可以在跑节点时使用。如果你想[连接到远程端的节点](https://wiki.polkadot.network/docs/en/maintain-wss)，可以开启 `--ws-external` 和 `--rpc-cors all` 选项。

根据网路带宽、CPU性能、硬盘读写速度、RAM 读写速度的不同，区块链的同步时间也会有所不同。

至此，你已经完成了和 Phala 网络的同步，恭喜 :)

## 运行归档节点

如果你只运行一个精简版的同步型节点（如上文所述），只有前 256 个区块的状态会被保留。作为验证人时，节点会默认设为[归档模式](#节点类型)。此时可以使用 `--pruning` 参数来强制保存全部链上状态。

```bash
./target/release/phala-node --chain poc2 --name "你的节点名" --pruning archive
```

你可以用`--wasm-execution Compiled` 加速同步，最高可能提升四倍速度。但这也会占用更多 CPU 运存和内存。所以记得在同步完后把它恢复到常态。

## 使用 Docker 运行节点

用 Docker 运行节点是可行的，但可能涉及到一些高级的操作。我们建议已经对 Docker 比较熟悉或已经部署过 Docker 的人继续这一步。

你可以在 [这里](https://github.com/Phala-Network/phala-blockchain/blob/master/Dockerfile) 找到 Docker 相关的材料。Docker 默认只产生两个本地测试网节点，更适用于开发。你也可以编写自己的 Docker 文件并连接到 Phala PoC-2 测试网。
