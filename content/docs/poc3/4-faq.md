---
title: "4 Frequently Asked Questions"
---

### Miner Community

Please don't hesitate to ask us if you have any questions. You can find us at:

[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zjdJ7d844d)[![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)

## I. Glossary

- **SGX**. [Software Guard Extensions](https://en.wikipedia.org/wiki/Software_Guard_Extensions).
- **TEE**. [Trusted Execution Environment](https://en.wikipedia.org/wiki/Trusted_execution_environment).
- **Full-node**. A **full node** is *pruned*, meaning it discards all information older than 256 blocks, but keeps the extrinsics for all past blocks, and the genesis block. A node that is pruned this way requires much less space than an archive node. In order to query past state through a full node, a user would have to wait for the node to rebuild the chain up until that block. A full node *can* rebuild the entire chain with no additional input from other nodes and become an archive node. One caveat is that if finality stalled for some reason and the last finalized block is more than 256 blocks behind, a pruned full node will not be able to sync to the network.
    > There's no token incentives for a full node on Phala Network.
- **phala-node**. The Substrate blockchain node.
- **pRuntime**. The TEE runtime that runs confidential contracts.
- **pHost**. Also called "the bridge relayer". The Substrate-TEE bridge relayer. Connects the blockchain and pRuntime.
- **Runtime**. The state transition function of a blockchain. It defines a valid algorithm for determining the state of the next block given the previous state.
- **Miner**. An SGX-ready device that has successfully installed on Phala Network to gain PHA rewards by renting its computing resources to the network.
- **Worker**. A mainboard/PC/laptop/device where there runs a miner.
- **Round**. Also called "mining round". With the current PoC-3 setup, 1 round = 600 blocks. You may check the round changes via [chain state](https://poc3.phala.network/#/chainstate) -> phalaModule ->  round(): RoundInfo

## II. Setting up Multiple Miners

As Phala is an SGX-based network, each miner node has to be SGX-enabled, which also means one SGX-enabled rig shall only run one Phala miner node.  Still, you can set up multiple miners if you have enough devices or enough time/efforts to maintain.

### Q: Shall I set up multiple full-nodes as well if I have several miner workers?

You can bond them with one full-node. One full-node can support up to 20 miners. If you need to set up more than 20 miners, you will need to set up more full-nodes as well.

### Q: Can I use the same IP address if I have multiple computer?

Yes.

### Q: Is a public Internet IP address necessary and will it gain more points in mining?

No.

### Q: Can I use the same Phala account for multiple miners?

You can set the same Recipient Account while setting miner payout perference (`phalaModule.setPayoutPrefs`). But please note that one miner should only be bonded with an unique Stash-Controller account pair. Say, if you have 20 miners, you will need 20 Phala Stash-Controller account pairs.

### Q: Will there be a solution to registering multiple miner accounts in a short time?

There will be in the near future.

## III. Linux Updates & SGX Driver

### Q: Got error: `apt install --fix-broken`

Type in `sudo apt install –fix-broken`.

### Q: Got error: `need install curl `

Run the commands below:

```shell
sudo apt-get update
sudo apt-get install curl
```

### Q: It got stuck after `docker pull...` commands

Please kindly check your network.

## IV. Issues related to phala-node, pRuntime, pHost

### Q: Got error "no such command" after I typed `docker pull docker.pkg.github.com/phala-network/phala-docker/phala-node:pre-test-4`

Please check the installation of Docker-CE.

### Q: Got error `permission denied`

Add `sudo` in front of your commands.

### Q: Got error `docker: Error response from darmon: Conflict. The container name "/phala-node" is already in use by container`

It means you already the containers of your last step (running phala-node/pRuntime/pHost) has installed the containers correctly and you don't need to run the same container again.

### Q: Got errors as blow:

``` shell
Starting pHost with extra opts '-r'
Connected to substrate at: ws://192.168.0.9:9944
thread 'main' panicked at 'Controller not registered; qed', phost/src/main.rs:512:13
note: run with 'RUST_BACKTRACE=1' environment variable to display a backtrace
```

1. Please kindly check whether your node is working well and synchronizing blocks normally at [Polkadot Telemetry](https://telemetry.polkadot.io/#list/Phala%20PoC-3). You can only start the next step after its blockheight is the same with other nodes.
2. Please make sure the corresponding account you specified as the mnemonic is registered as a Controller account.
3. Finally make sure you entered a corrent mnenonic.

### Q: Got error: `/dev/sgx/enclave No such file or directory` or `/dev/isgx No such file or directory`

You may need to check your SGX driver. Also, this error happens when you used a wrong command (e.g. run the SGX driver command with a DCAP driver).

### Q: How to check if my installed driver is SGX or DCAP?

- Run `ls /dev/isgx` and if the file exists: SGX-driver
- Run `ls /dev/sgx` and if the directory exists: DCAP-driver

### Q: I suspect some docker container dies / hangs / doesn't run correctly but how can I debug?

Remove `-d` in the `docker run` command and run it again to get the output in the foreground. You can screenshot the log and send it to the community for help. To dump a full log from a hanging docker, please run:

```bash
docker logs phala-phost > phost.log
```

### Q: How can I dump the logs from the containers?

When they are running:

- phala-node: `sudo docker logs phala-node`
- pRuntime: `sudo docker logs phala-pruntime`
- pHost: `sudo docker logs phala-phost`

You can check if the container is running by:

```bash
docker ps
```

If the container dies right after the launch, consider to remote `-d` from the startup commandline to get the output.

### Q: What if my Machine ID is empty after I've run all the commands?

Upgrade your miner(s) following [3.5 Upgrade Miner]({{< relref "docs/poc3/3-5-upgrade-miner" >}}). If it doesn't help, a walk-around is to create a new stash and controller account pair, transfer the token to the new accounts, and then start over.


## V. Operations On-chain

### Q: Can I bond the controller account with multiple stash accounts?

Nope. But you can set the stash account as its own controller.

### Q: Why my balance didn't change after I have started mining?

After you signaled the mining intention, the miner will start mining from the next round. As for what is `round`, please refer to Glossary. You can check your mining status from Chain States -> `phalaModule.workerState` and search by your stash account. After signaling the mining intentil, the state of the miner should switch from `Free` to `MiningPending`. In the next round, it should switch to `Mining` automatically.

Phala Network adopts a probabilistic mining reward algorithm. So you may need to wait for some blocks until hit a block reward prize. For more details, please check `Probabilistic Reward Distribution` in the Glossary.

### Q: Got error `Not Controller` while setting commission

Please make sure the controller is registerd on the blockchain by `phalaModule.setStash(controller)`.

## VI. Restart your pHost

### Q: It reads `Err(BlockHeaderMismatch)` after I restarted the pHost.

It could happen to some type of hardwares. You may have to kill your pRuntime first then to restart your pHost. 

### Q: Can I run the `restart` command directly instead of using `kill` and `run` ?

You can give it a try if you are have been familiar with docker codes.  


## VII. Other

### Q: I can't claim test tokens after clicking `Click to burn` or `Click to claim`

1. Go to Etherscan or any Ethereum explorer
2. Search your ETH account and find the burning tx hash
3. Copy and paste it into `Claim Tokens` -> `ETH TXID`
4. Copy and paste your Phala account into `PHA RECIPIENT ADDRESS`
5. `Click To Claim`

### Q: Error happens when I tried to run the self-examination program

It might be caused by 2 reasons:

1. The docker has not been completely downloaded. Please pull it again following the given steps.
2. The SGX driver is not enabled. Please try to re-install your SGX driver following the given steps.

