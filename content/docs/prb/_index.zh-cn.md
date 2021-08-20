---
title: "矿池协议"
weight: 4
---
在单节点挖矿场景中，[pherry](https://github.com/Phala-Network/phala-blockchain/tree/master/standalone/pherry)的功能是从区块链全节点获取区块信息并发送到TEE运行时。他很好用，但是在矿池协议等应用场景里会有很多限制和性能不足。因此，Phala [Runtime Bridge](https://github.com/Phala-Network/runtime-bridge) （我们也叫它“矿池协议”或者 `prb` ）被引入，以利用新经济模型（提案）中引入的StakePool机制，并降低使用独立 pherry 的复杂性。
`prb` 为云原生的Linux Shell环境部署，它提供了一个RPC接口，通过Redis来管理Worker的生命周期，他可以在矿池协议场景中替换pherry。

## 系统需求

因为Substrate上的JavaScript工具支持最为完善，`prb` 使用Node.js编写。但由于Node.js不是单线程模式运行的，所以你需要超过8个核心的处理器。

`prb` 可以使用RocksDB和LevelDB作为后端存储，他需要足够强大的随机I/O性能来支持大量的随机读写操作。因此，选用有大缓存的PCI-E 3.0/4.0 Gen4 的SSD是非常必要的。根据测试，可以给出一个基本的平均每个区块的存储使用量参考：
- 50 KiB using LevelDB,
- 40 KiB using RocksDB.

在`prb` 运行时，需要一个完整的本地网络内的Phala（带有中继链的）全节点

## 功能

`prb` 提供一下服务:
- `io`: 一个简单的内部I/O服务，用于连接RocksDB/LevelDB. 因为RocksDB/LevelDB是进程安全的，你只能同时运行其中一个。
- `fetch`: 从过区块链全节点中获取数据，同步并处理数据。
- `trade`: 向区块链发送交易
- `lifecycle`: 处理Worker的生命周期状态，用于TEE和区块链间消息队列同步

## Redis, RPC 和监视器

Redis 用于维护内部的消息队列和组建之间的通信, **不要把它暴露在公共网络中**。

 `prb` 中使用的Protobuf定义的RPC接口, [定义](https://github.com/Phala-Network/runtime-bridge-proto/blob/main/message.proto).

[监视器](https://github.com/Phala-Network/runtime-bridge-monitor)是一个简单Web界面的Worker管理工具。**请注意，它只是一个示例**

## Linux 内核优化

您无需在开发和测试期间对内核进行调优。

当Prb中的Worker量超过300台时应该调整lifecycle组建的最大TCP连接数, [请参照这里的示例](https://github.com/Phala-Network/runtime-bridge/tree/master/system/bridge) **请您注意，我们不提供Linux内核级别的调优支持**
