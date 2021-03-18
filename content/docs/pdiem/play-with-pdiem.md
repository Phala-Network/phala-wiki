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

## Deposit Diem assets to pDiem

To send Diem assets to pdiem, just transfer it to the deposit address (`account 0`) in `diem-cli`. For example, we can send `10 XUS` to `account 0` like this:

```bash
diem% transfer 1 0 10 XUS
```

And that's all!

Behind the scene, `pdiem-relayer` picked up the deposit transaction, send a regular Substrate extrinsic to the `pdiem` confidential contract on Phala to sync the transaction. After the validation of the incoming transaction and the proof, the `pdiem` contract will accept it. Usually it takes 30 seconds for the relayer to pick up the transaction, and relay to `pdiem` side.

### Balances in pdiem

```bash
./phala-console.sh pdiem-balance
```


### Verified transactions

All the transactiosn received by `pdiem` contract are validated against the latest Diem ledger state, which is protected by a PoS-like consensus. The validated transactions are stored inside the `pdiem` contract storage, and can be checked by anyone:

```bash
./phala-console.sh pdiem-tx
```

## Withdraw pDiem assets to Diem

(WIP)
