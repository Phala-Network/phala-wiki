---
title: "1.3 Set Miner Stash"
---

### Connecting to Testnet Vendetta

Open the Phala Network PoC-3 Web UI at: <https://poc3.phala.network>

### Creating Phala Accounts

You need to create two accounts: a stash account and a controller account. You may refer to [this tutorial](https://forum.phala.network/t/how-to-create-a-phala-account-on-testnet-vendetta/1253) about how to create a single account.

Tip: to better manage your stash accounts and according controller accounts, we recommend to name an account pair with the same prefix. E.g.,

> Emily Stash / Emily Controller

### Obtaining tPHA

> Please be aware that you will need to burn ERC20 PHA to obtain tPHA on testnet Vendetta. 1 ERC20 PHA = 1000 tPHA. tPHA has no value and **CANNOT** be exchanged back to ERC20 or mainnet PHA.

You may refer to [this tutorial](https://forum.phala.network/t/how-to-obtain-tpha-on-testnet-vendetta/1254) for how to obtain tPHA.

### Setting Miner Stash

1. Go to `Developer` → `Extrinsics` (or click [here](https://poc3.phala.network/#/extrinsics))
2. Choose `phalaModule`  →  `SetStash(controller)`
3. Select your STASH account in the first line, and your CONTROLLER account in the third line.
4. `Submit Transaction`, sign it and wait for the broadcast.

![](/images/docs/poc3/1.3.png)
