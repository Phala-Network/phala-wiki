---
title: "Play with pDiem"
weight: 2
draft: false
---

![](/images/docs/pdiem/docker-compose-structure.png)

The pdiem full stack includes:

- A local Diem testnet: `diem`, `diem-cli`
- A Phala Network local testnet with the pdiem contract: `phala-node`, `phala-pruntime`, `phala-phost`
- A relayer to connect the both networks: `pdiem-relayer`

With the pdiem full stack running, we are going to send some Diem transactions to the pdiem deposit account, and check if the pdiem contract has received it.

## Create Diem accounts

First, start the diem-cli from the same directory:

```bash
docker-compose run --rm --entrypoint /bin/bash -- diem-cli
```

It spawns a shell at `/root`. Then enter the Diem console inside the shell by:

```bash
./diem-cli.sh
```

![](/images/docs/pdiem/diem-cli.png)

Then run the following commands to prepare the accounts:

```bash
# Create two accounts
diem% account create
diem% account create
# Mint some funds
diem% account mint 0 100 XUS
diem% account mint 1 100 XUS
```

The `diem-cli` tool is a light wallet connecting to the diem node via RPC. It can be used to manage accounts, transfer tokens, and query the blockchain. In the above steps, we created two accounts (indexed by 0 & 1), and mint some test `XUS` token to the account.


{{< tip >}}
Among them, the `account 0` is a special account that hard-coded in the pdiem-m3 demo contract as the deposit address. Usually `diem-cli` generates random accounts. But in the pdiem-m3 demo, the seed to generate the wallets is pinned with a special file [client.mnemonic](https://github.com/Phala-Network/phala-docker/blob/pdiem-m3/dockerfile.d/client.mnemonic) so that everytime it generates the same hard-coded account.
{{< /tip >}}

## Simple diem-to-pdiem transfer

Simple transfer refers to the transfer between the admin address (`account 0`, hardcoded in the pdiem-m3 contract).

To send Diem assets to pdiem, just transfer it to the admin address (`account 0`) in `diem-cli`. For example, we can send `10 XUS` to `account 0` like this:

```bash
diem% transfer 1 0 10 XUS
```

And that's all!

Behind the scene, `pdiem-relayer` picked up the deposit transaction, sent a regular Substrate extrinsic to the `pdiem` confidential contract on Phala to sync the transaction. After the validation of the incoming transaction and the proof, the `pdiem` contract will accept it. Usually it takes 30 seconds for the relayer to pick up the transaction, and relay to `pdiem` side.

### Balances in pdiem

```bash
./phala-console.sh pdiem-balances
```

### Verified transactions

All the transactiosn received by `pdiem` contract are validated against the latest Diem ledger state, which is protected by a PoS-like consensus. The validated transactions are stored inside the `pdiem` contract storage, and can be checked by anyone:

```bash
./phala-console.sh pdiem-tx
```
## Prepare subaccounts for two-way transfer

pdiem-m3 uses Diem's subaccount feature to create deposit accounts. Any user can create its Diem deposit subaccount by sending a `NewAccount` command:

```bash
./phala-console.sh pdiem-new-account '//Alice'
```

The above command will send `Command::NewAccount` to the `pdiem` contract from Substrate "Alice". You can create as many accounts as you want. It takes 2-3 blocks to process the command. Then you can get a full list of the Diem accounts by a query:

```bash
./phala-console.sh pdiem-accounts
```

> A screenshot...

## Deposit (diem → pdiem)

Get your deposit address from the last step. And now you can deposit some `XUS` on the Diem side in diem-cli:

```bash
diem% transfer 1 0xDEADBEEFDEADBEEFDEADBEEFDEADBEEF 10 XUS
```

The corss-chain transfer takes around 4-6 blocks to complete. Check the balance by:

```bash
./phala-console.sh pdiem-balances
```

## Withdraw (pdiem → diem)

You can withdraw assets from `pdiem` back to diem by `pdiem-withdraw`:

```bash
./phala-console.sh pdiem-withdraw 0xDEADBEEFdeadbeefDEADBEEFdeadbeef '10 XUS' '//Alice'
```

In the command above the arguments are:

- `0xDEADBEEFdeadbeefDEADBEEFdeadbeef`: The Diem address to withdraw
- `'10 XUS'`: The amount of the assets
- `'//Alice'`: The key of the Substrate account to withdraw

{{< tip >}}
The `pdiem-withdraw` command sends a `Command::TransferXUS` transaction to the `pdiem` contract via the Phala blockchain. The contract then creates a Diem transaction from the subaccount to the withdrawal target, sign it, and push it to a queue. Once the `pdiem-relayer` noticed new transactions in the queue, it relays them to the Diem blockchain.

The withdrawal transactions are queued until it gets confirmed on Diem. When confirmed, the outgoing transactions are observed by the `pdiem-relayer`, and an evidence is sent back to the `pdiem` contract. Finally the `pdiem` contract will confirm the withdrawal transactions are executed.
{{< /tip >}}

You can check your `pdiem` balance at any point:

```bash
./phala-console.sh pdiem-balances
```

You can still see the withdrawal amount under the "locked" balances until transaction is confimed on Diem.
