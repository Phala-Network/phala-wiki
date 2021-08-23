---
title: "使用 Docker Compose 部署示例"
weight: 2
---

 `prb` 的Docker镜像会随着每次代码更新在Github及[Docker Hub](https://hub.docker.com/r/phalanetwork/prb)更新发布，您可以通过以下命令获取最新版本:
```bash
docker pull phalanetwork/prb
```

 `prb` 的设计非常简单，他只有简单、基本集成RPC的功能。使用Docker管理 `prb` 会比在Linux中集成它要容易得多。以下是使用Docker Compose部署 `prb` 的一个简单示例。

> **注意:** 此事例只介绍各个服务之间的连接关系，请您根据自己的需求进行配置。

## 系统要求
- Ubuntu LTS 20.04
- Docker 20.10 or newer
- Docker Compose 1.29 or newer

> Ubuntu 默认APT源中的Docker版本太旧，请您根据 https://docs.docker.com/engine/install/ and https://docs.docker.com/compose/install/ 安装最新的Docker Compose版本。

> 在生产环境中，请您使用Docker Hub中的预构建的镜像。

## 环境准备

1. 创建一个新的文件夹，然后用[这里](https://github.com/Phala-Network/runtime-bridge/blob/master/docker/testing/bridge/docker-compose.example.yml)的样例创建和编辑docker-compose.yml。
2. 运行 `docker-compose pull` 拉取最新镜像.
3. 在Worker机上部署 `pruntime` .
4. 在区块链上建立Stakepool.

## 基础服务

运行 `docker-compose up -d redis io` 开启基础服务。

在这个示例里Redis没有数据持久化，服务意外退出会破坏整个环境，请在生产环境中为Redis创建一个HA配置。公开Redis端口以方便RPC工作

RocksDB/LevelDB 数据将存在 `PHALA_DB_PREFIX` 指定的环境变量中。`0` 目录将用于区块数据存储。 `1` 目录用于保存矿池和Worker数据，包括私钥，请务必备份。

##  `fetch` 服务

运行 `docker-compose up -d fetch` 开启 `fetch` 服务。

此服务将至少消耗3个CPU内核的资源:
1. 从父链中获取区块头
2. 从平行链中获取区块头并存储改动
3. 将区块数据编码为`pruntime` 使用的格式
4. 组织并合并上述数据以实现快速同步

这所有的工作都是异步操作的，您可能看到这几个进程会占用大量的CPU. `fetch` 服务可以根据实际情况停止和启动。

##  `trade` 服务

运行 `docker-compose up -d trade` 开启 `trade` 服务。

它需要1个CPU核心来签名和发送消息队列中的消息。 他的密钥将保存在内存中，这是安全的。

每次更改pool的账户信息时 `trade` 都必须重启才能生效。

##  `lifecycle` 服务

运行 `docker-compose up -d lifecycle` 开启 `lifecycle` 服务。它控制着TEE工人的生命周期 。使用 [monitor](https://github.com/Phala-Network/runtime-bridge-monitor) 添加、编辑、删除和检查工作人员的状态。

他应该对Worker进行更改（在现在的调试和测试中）。
