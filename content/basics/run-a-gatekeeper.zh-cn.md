---
title: "运行 Gatekeeper"
date: 2020-07-10T01:16:05+08:00
draft: false
---

这份教程将指导您如何安装 Phala 验证人节点。

## 什么是守门人

守门人（Gatekeeper）是对 Phala 网络至关重要的角色。守门人负责区块的打包和密钥管理，是意外情况下保证网络可用性的重要途径。守门人需要使用性能较好的设备、在网络情况良好的环境登录，且必须时刻保持在线。因此，**守门人可以获得可观的收益**，但同时需要对自己和提名人的PHA抵押额负责。如果因为频繁掉线或其他不良行为导致被惩罚，则无论是名誉还是PHA损失都会是巨大的。

运行守门人节点，安全是第一要素。你可以查看[搭建安全Polkadot验证人节点](https://wiki.polkadot.network/docs/en/maintain-guides-secure-validator)查看可能影响守门人安全运行的因素。Web3 基金会也维护了一个你可以自行部署的[验证人节点参考实现](https://github.com/w3f/polkadot-secure-validator)（[此处](https://www.youtube.com/watch?v=tTn8P6t7JYc)有视频教程)。在配置守门人节点的过程中，可以把这个代码库当作 _一个初始模版_ ，在上面自行修改、裁剪。

如果您有任何疑问，可扫码添加 Phala 小助手，回复**守门人**，加入守门人答疑群。

![](/images/phala-qr.jpg)

### 守门人需要抵押多少 PHA

在测试网上做守门人需要抵押 **10** 个测试币。

[点此查看如何领取测试币](https://www.yuque.com/docs/share/e016c4c3-bc63-4689-920e-6f7c0434e4c0?#)

您可以在 [这个页面](https://wiki.polkadot.network/docs/en/faq#what-are-the-ways-to-find-out-the-minimum-stake-necessary-for-the-validators) 找到估算所需 PHA 数额的方法。
守门人将根据 [Phragmen算法](https://wiki.polkadot.network/docs/en/learn-phragmen) 进行选举。要想入选，您和[提名人](https://wiki.polkadot.network/docs/en/maintain-nominator)的保证金之和必须不低于其他守门人。赢得提名人的支持非常重要。此外，也要注意在您的 Stash 账号和 [Controlloer 账号](https://wiki.polkadot.network/docs/en/learn-keys)上都要留有足够的 PHA。

> 例：
> - 守门人A抵押了 10 PHA 作为保证金，他的提名人为他抵押了 100 PHA。则守门人A的总保证金为 110 PHA
> - 守门人B抵押了 100 PHA，但没有提名人为他抵押
> - 入选的会是A

需要注意，一旦将您的一部分资金设为保证金，</br>

**它们可能会因为您的不良行为或操作不当而被罚没**。</br>
**它们可能会因为您的不良行为或操作不当而被罚没**。</br>
**它们可能会因为您的不良行为或操作不当而被罚没**。</br>

因此再次建议您仔细阅读此篇教程的操作。

此外，

**请妥善保管您的助记词**。</br>
**请妥善保管您的助记词**。</br>
**请妥善保管您的助记词**。</br>

## 运行守门人节点之前

### 配置要求

以下所有教程均基于 Ubuntu 18.04，但理论上其他操作系统也遵循类似的要求与操作规范。

交易的权重取决于 Phala Network 在标准硬件上的性能基准测试得来。因此我们推荐在标准硬件或更好配置的机器上运行守门人节点，以满足性能要求。下面描述的并不是运行节点的 _最低需求_，但更低的配置不能保证稳定的运行。

#### 标准硬件

完整的细节可以参考[这里](https://github.com/paritytech/substrate/pull/5848)。

- **CPU**：英特尔6代或更新的CPU，需支持SGX，至少双核心
- **内存**：8GB（最低 2 G）
- **磁盘空间**：40-80 GB（越大越好，建议采用 NVMe 固态硬盘，每六个月需要重新评估节点磁盘占用大小）

此外，**网络条件**非常重要，其他参数次之。我们强烈建议您在网络良好的地方运行守门人节点。关于如何确定自己的电脑是否支持 SGX ，Phala 为您准备了详细的[SGX测试教程](https://shimo.im/docs/GVc6vTykhrVcYD9P/)，可点击链接观看。

### 安装 Intel SGX 驱动与平台软件

可以从 [官方下载页面](https://01.org/intel-software-guard-extensions/downloads) 找到最新的 Linux SGX 相关软件下载。确保系统中安装了如下软件：

- SGX Linux DCAP 驱动 (安装至 `/opt` 目录)
- SGX Linux SDK
- SGX PSW (平台软件)

具体安装方法请参见 Intel 官方指南。也可以参考 Teaclave SGX SDK 提供的 [Dockerfile](https://github.com/apache/incubator-teaclave-sgx-sdk/blob/253b3ac982b2d09d32f5fa5a2011e3c36bcbed1e/dockerfile/Dockerfile.1804.nightly) 来安装驱动套件。

### 安装 `phala-node` 可执行程序

你可以直接从 Phala Network 的 [Github 发布页面](https://github.com/Phala-Network/phala-blockchain/releases) 下载 `phala-node` 可执行程序。

也可以直接从 [Phala-Network/phala-blockchain](https://github.com/Phala-Network/phala-blockchain) 代码库的 **master分之** 上直接编译 `phala-node`。你可以参照 [运行全节点]({{< relref "basics/run-a-full-node" >}}) 教程中的步骤编译节点：

> 注意：如果你倾向于用SSH链接Github，也可以把以下第一条命令替换为
> `git clone git@github.com/Phala-Network/phala-blockchain.git`.

```sh
  git clone https://github.com/Phala-Network/phala-blockchain
  cd phala-blockchain
  ./scripts/init.sh
  git submodule update --init
  cargo build –-release
```

这一步会比较消耗时间（大约 10 - 40 分钟，取决于硬件配置）。

> 如果你在编译的时候遇到编译错误，就可能需要切换到一个稍旧一点的 Nightly Rust 版本，可以通过以下命令实现：
>
> ```sh
> rustup install nightly-2020-05-15
> rustup override set nightly-2020-05-15
> rustup target add wasm32-unknown-unknown --toolchain nightly-2020-05-15
> ```

如果你希望在本地生成密钥，也可以在同一目录下安装 `subkey`。如果想进一步提高安全性，则可以把编译好的 `subkey` 程序发送到一台全新且断网的电脑上进行操作。

```sh
cargo install --force --git https://github.com/paritytech/substrate subkey
```

### 同步链上数据

> **注意**：默认情况下，Gatekeeper节点处于归档模式(Archive)。 如果您以非归档模式同步了链上数据，需要先使用 `phala-node purge-chain` 删除数据库，然后启用 `--pruning=archive` 命令行选项。
>
> 您可以使用以下指令在非归档模式下运行 Gatekeeper 节点：`-unsafe-pruning --pruning <块数>`。但是请注意，归档节点和非归档节点的数据库彼此不兼容。要进行切换，需要清除链上数据。

如果你不想立即以守门人模式启动节点，可以运行以下指令同步节点：

```sh
./phala-node --pruning=archive
```

`--validator` 和 `--sentry` 选项中已经包含了 `--pruning=archive` 选项。所以，只有在没有启用上述选项的情况下，才需要特殊指定该选项。如果你没有运行存档节点，或未以守门人、哨兵身份运行节点，当你切换的时候，需要重新同步数据库。

> **注意**：守门人需要用 RocksDb 后端进行同步。RocksDb 是一个默认设置，但可以用 `--database RocksDb` 选项来明确声明。将来，我们建议使用更快和更高效的 ParityDb。在不同数据库后端之间切换，同样也需要重新同步。
> 
> 如果你现在就想测试 ParityDB，可以开启 `--database paritydb` 参数。

同步时间跟区块链的大小有关，可能需要几分钟到几小时不等。

如果你想知道同步具体需要多少时间，可以查看服务器日志，上面记录了最新被打包和验证的区块编号。将其与 [Telemetry](https://telemetry.polkadot.io/#list/Phala%20PoC-2) 和 [Phala Web App](https://app.phala.network/#/explorer) 上的最新区块高度做一个对比即可推算出大致需要的同步时间。

> **注意：** 在主网“软启动”的过程中（测试网络不受此影响），如果你没有 PHA 代币，基本上就只能跟着教程做到这一步了。你可以运行一个节点，但接下来的操作必须有一定的 PHA 才可以继续。由于在“软启动”过程中，PHA 的转账还是禁用状态，你无法接受别人的转账从而得到 PHA 代币。此时，即使拥有 PHA 且参与守门人抵押，也只是表明了自己成为守门人的 _意愿_，知道 NPoS 阶段启用才会真正开始选举。

## 抵押 PHA

我们强烈建议将 Controllor 账号和 Stash 账号设置成两个账号。所以，请创建**两个**账号，并在两个账号上都留有一定资金用以支付交易手续费。而后将大部分资金存入 Stash 账号。

请务必注意留一点资金用作交易手续费，不要绑定所有的资金。

现在可以开始设置守门人。进行如下操作：

- 绑定 Stash 账号的 PHA 。**这个账号将负责保管你的保证金。**
- 选择 Controllor 账号。**这个账号将用于开启或暂停守门人的验证行为。**

首先，点击 `质押` > `Account Actions` > `Stash` （按照截图进行操作）。

![dashboard bonding](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-bonding.jpg)

- **Stash account**：选择你的 Stash 账号。这个账号上需要至少留有 100 milli PHA。当然，你也可以多留一些。
- **Controller account**：选择之前创建的 Controller 账号，这个账号上也需要保留一点 PHA 用于后续支付手续费。
- **Value bonded**：在这里输入你想抵押的金额。再次提醒，**请不要绑定你所有的PHA**。之后，你随时可以追加押金，但取回押金有一定冷冻期。冷冻期在 Phala Network 上是 7 天。
- **Payment destination**: 选择你打算用来收取收益的账号。详情可 [点此](https://wiki.polkadot.network/en/latest/polkadot/learn/staking/#reward-distribution) 了解更多。

然后点击 `Bond`，使用你的 Stash 账号签名并广播交易。

稍候片刻，页面中就会跳出 `ExtrinsicSuccess` 的通知。刷新后，页面上会出现一个新卡片，在这里你可以看见自己抵押的 PHA 数量。

## 设置 Session 密钥

> 注意：Session 密钥对于共识至关重要。如果不确定节点是否有密钥的话，可以使用两种方法来检查：
> - [hasKey](https://polkadot.js.org/api/substrate/rpc.html#haskey-publickey-bytes-keytype-text-bool) 来检查是否有某一个 Session 密钥，或者
> - [hasSessionKeys](https://polkadot.js.org/api/substrate/rpc.html#hassessionkeys-sessionkeys-bytes-bool) 来查看所有的 Session 公钥

待节点完全同步后，按Ctrl+C停止同步。现在，你可以开始以守门人身份运行节点。你的节点将使用命令行选项启用 Unsafe RPC 来进行一些高级操作。

```sh
./phala-node --validator --name "你的节点名"
```

你可以给自己的守门人节点起一个独特又有个性的名字，这个名字将展示在 Telemetry 上。由于可能参与者比较多，因此可以选择一个独特的名字以避免重复。

### 生成 Session 密钥

#### 方法1: PolkadotJS-APPS

你可以在 App 的 PRC 模块中生成 [Session 密钥](https://wiki.polkadot.network/en/latest/polkadot/learn/keys/#session-key)。请确保你已经在设置页面中把 PolkadotJS-Apps 指向你的守门人节点。 你可以在“设置”中自定义守门人节点的地址与端口号。但如果你连接的是官方提供的公用节点，就无法使用此功能，因为它会把 RPC 发送到 _公共节点_，而不是 _你的节点_。

连上节点后，设置 Session 密钥的最简单方法就是调用 `author_rotateKeys` RPC，在守门人节点的本地密钥库中创建新密钥。选择“Toolbox” > “PRC Calls”，然后选择 “Author” > “ rotateKeys（）”，会返回一个如图所示的字符串。记下好返回的密钥即可。

![Explorer RPC call](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-explorer-rotatekeys-rpc.jpg)

#### 方法2: CLI

如果你在远端服务器上运行守门人节点，可能运行这个指令会更简单（假设你没有修改默认 HTTP PRC 端口号）：

```sh
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9933
```

它会返回一个十六进制编码的 “result” 字段，由4个公钥串联而成。记下即可。

现在，你可以重启节点并且去掉 `--unsafe-rpc-expose` 选项，此后就不再需要这个选项。

### 提交 `setKeys` 交易

你需要提交一个交易并签名，来向区块链提交你的 Session 密钥（公钥），他实现了守门人节点和你的 Controller 账号的关联。

- 点击 [质押 > Account Actions](https://app.phala.network/#/staking/actions)
- 然后在之前生成的绑定帐户上单击 `Set Session Key`
- 输入 `author_rotateKeys` 返回的结果
- 点击 `Set Session Key` 。

![staking-change-session](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/set-session-key-1.jpg)
![staking-session-result](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/set-session-key-2.jpg)

现在，你可以开始运行守门人身份了。

## 注册 TEE 设备

守门人利用 TEE 管理 Phala Network 中的密钥。在开始启用验证前，你需要在为守门人账号注册 TEE 硬件。在守门人运行的整个生命周期之中，TEE 设备都需要时刻保持与区块链相连，来支持网络上的各种活动。

TEE 模块由 `pHost` 与 `pRuntime` 构成。可以在我们的 Github [发布页](https://github.com/Phala-Network/phala-blockchain/releases) 中的 `tee-release.zip` 文件中找到最新的预编译程序。当然，也可以自行编译。

假设你已经解压了预编译的程序，并且已经运行了一个同步好的 `phala-node`，就可以采用以下步骤在链上注册你的 TEE 设备：

1. 启动 pRuntime: `./app`
2. 设好命令行参数并启动 pHost:
  ```
  ./phost \
        --mnemonic '<你的 Controller 账号私钥助记词>' \
        --no-sync \
        --no-write-back \
        --remote-attestation \
        --substrate-ws-endpoint 'ws://localhost:9944'
  ```

这一步需要指定 Controller 的私钥助记词，因为 `phost` 会以 Controller 的身份发送一笔交易到链上，来注册 TEE 设备。

预编译的发布包中也包含了一个脚本文件 `bridge.sh`，作为以上方法的替代，可以较为方便的启动 `phost`。你可以在脚本中设置私钥，就不用每次启动都输入一次。

> 注意：在 Phala Network Testnet PoC-2 中，TEE 只需要进行一次注册即可。`phost` 会在成功注册后自动退出。但是在未来的正式主网上线后，只要节点还在以守门人身份运行， TEE 组件就需要时刻保证在线，因为在完整的生命周期中 TEE 都需要对网络提供不间断的支持。

## 开启验证

可以通过 [Telemetry](https://telemetry.polkadot.io/#list/Phala%20PoC-2) 查看自己的节点是否在线且完成同步。Telemetry 将显示所有在线的节点，因此，有一个个性的名字会帮助您更快找到您的节点 :)

如果一切正常，请在 Phala Network UI 中点击 `Validate` 。

![dashboard validate](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-validate.jpg)
![dashboard validate](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-validate-modal.jpg)

- 收款偏好设置：在此设置你想保留的奖励百分比作为手续费，而剩余的部分将由您和提名人共享。

点击 `Validate` 。

> 注意: 如果你没有遵循上面的步骤注册 TEE 设备，或者注册没有成功，这一步就会执行失败。请仔细阅读 [注册 TEE 设备](#注册-tee-设备) 一节，确保 Controller 账号已经与 TEE 成功关联。

进入 `质押` 页面就可以看到所有正在运行的守门人。同时也可以在页面的顶端看到，目前守门人席位还有多少，以及多少节点正在申请成为守门人。点击 `Waiting` 可以查看您的节点是否仍在候选列表之中。

![staking queue](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-staking.jpg)

被选中的守门人不是一成不变的。每一个区块 Era 都会刷新。如果你已经通过投票算法入选守门人，但目前没有空位但，而下个 Era 有了空位，你将在下个 Era 自动成为守门人。在那之前，你将处于 _候选状态_。如果这一轮没有入选，则你将处于 _候选状态_。可以试试抵押更多 PHA 或招募更多提名人以提高排名。

**恭喜!** 如果你已经按照以上步骤操作并入选，你就成功成为了一个 Phala Network 守门人! 如果需要任何帮助，可以加入我们的
[Phala Network Telegram 群组](https://t.me/phalanetwork).

## 参与测试网络 PoC-2 活动

为了参加测试网络 PoC-2 的活动，我们要求运行节点的时候按照以下格式设置自己的名字：

```bash
./phala-node --name "名字 | Controller地址" <...其他参数>
```

详情请见[运行全节点]({{< relref "basics/run-a-full-node" >}}) 教程中的同名章节。

## FAQ

### 为什么我同步的时候一直显示 0 peers？

![zero-peer](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-zero-peer.jpg)

请您检查是否启用 `30333` libp2p 端口。启用后，需要再等待一段时间来发现新的节点。

### 怎么清除链上数据？

```sh
./phala-node purge-chain
```

### 有没有 Docker 部署教程

正在准备中。
