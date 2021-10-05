---
title: "2.3 Worker升级"
---

升级程序之前需要先停止所有的进程。注意，这条命令只会关闭正在运行的挖矿软件，而不会影响链上状态。通常的升级可以在几分钟之内完成，不需要到链上（或Phala App）停止挖矿。

```bash
sudo phala stop
```
# 清空节点数据并升级

```bash
sudo phala update clean
sudo phala start
```

可以通过如下命令更新 Docker 镜像，并重启挖矿程序。这条命令不会清空区块历史，也不会删除 pruntime 的密钥：

```bash
sudo phala update
sudo phala start
```

### 清空节点数据并升级 

如下命令会清空节点数据并升级：

{{< tip "warning" >}}
注意，这条命令比较危险。清空节点数据的同时会删除密钥（Worker Pubkey），密钥是挖矿的凭证，如果被清除就会视为掉线，必须手动停止旧密钥下的机器，并使用新密钥完全重来挖矿流程。与此同时，旧的挖矿抵押还要等待一个完整的冷却周期。清除节点数据还会同时删除区块历史。一次从头的同步可能需要几天时间。因此如果在不确定的情况下，请不要用这种方式升级。
{{< /tip >}}

```bash
sudo phala update clean
```