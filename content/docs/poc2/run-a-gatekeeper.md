---
title: "Run a Gatekeeper"
date: 2020-07-10T01:16:05+08:00
draft: false
---

### Before Starting

Running a Gatekeeper (GK) in Phala Network means a lot of responsibility. You will be accountable for not only your own stake but also the stake of your current nominators. Numbers of Gatekeepers can vary in different phases; and when Phala launches its mainnet, there will be 100 Gatekeepers and they share the inflation return of Phala economic system.

Once you've raised enough stakes (nominations) in current [Era](https://wiki.polkadot.network/docs/en/glossary#era), you will automatically be enrolled as a Gatekeeper when the next Era starts. Otherwise, you will stay in the [waiting list](https://poc3.phala.network/#/staking/waiting). You may try to stake more PHA or call for more nominator stakings.

> In testnet Vendetta, you would need stakings from Phala team to be elected as a Gatekeeper. You may consult @ylyantonia on Telegram or Antoniaiaiaiaiaia#2727 on Discord for more detail.

Nominators would share the 5% inflation return with the commission deducted by according Gatekeeper. The higher the commission rate is, the less the nominators can share.

The table below may clarify the rewards mechanism. For more details, please refer to [https://wiki.polkadot.network/docs/en/learn-staking](https://wiki.polkadot.network/docs/en/learn-staking).

|               | **A - Validator Pool** |                             |         |
| :-----------: | :--------------------: | :-------------------------: | :-----: |
| Nominator (4) |      Stake (600)       | Fraction of the Total Stake | Rewards |
|      Jin      |          100           |            0.167            |  16.7   |
|    **Sam**    |           50           |            0.083            |   8.3   |
|     Anson     |          250           |            0.417            |  41.7   |
|     Bobby     |          200           |            0.333            |  33.3   |

<br>

|               | **B - Validator Pool** |                             |         |
| :-----------: | :--------------------: | :-------------------------: | :-----: |
| Nominator (4) |      Stake (400)       | Fraction of the Total Stake | Rewards |
|     Alice     |          100           |            0.25             |   25    |
|     Peter     |          100           |            0.25             |   25    |
|     John      |          150           |            0.375            |  37.5   |
|   **Kitty**   |           50           |            0.125            |  12.5   |

Please do be aware that if you are not a productive Gatekeeper (e.g., constantly being offline), you might get slashed and lose a certain amount of your staked PHA as well as your nominators'. It would leave [a permanent record](https://poc3.phala.network/#/staking/query) on the blockchain which might affect your reputation as a Gatekeeper.

<br>

### How many PHA do I need?

You can have a rough estimate on that by using the methods listed
[here](faq#what-is-the-minimum-stake-necessary-to-be-elected-as-an-active-validator). Gatekeepers are
elected based on [Phragmen's algorithm](https://wiki.polkadot.network/docs/en/learn-phragmen). To be elected into the set, you need a
minimum stake behind your Gatekeeper. This stake can come from yourself or from
[nominators](maintain-nominator). This means that as a minimum, you will need enough PHA to set up
Stash and Controller [accounts](https://wiki.polkadot.network/docs/en/learn-keys) with the existential deposit, plus a little extra for
transaction fees. The rest can come from nominators.

**Warning:** Any PHA that you stake for your Gatekeeper is liable to be slashed, meaning that an insecure or improper setup may result in loss of PHA tokens! If you are not confident in your ability to run a Gatekeeper node, it is recommended to nominate your PHA to a trusted Gatekeeper node instead.

## Initial Set-up

### Requirements

You will likely run your Gatekeeper on a server with TEE hardware running Linux. For this guide we will be using Ubuntu 18.04, but the instructions should be similar for other platforms.

The transaction weights in Phala Network were benchmarked on standard hardware. It is recommended that Gatekeepers run at least the standard hardware in order to ensure they are able to process all blocks in time. The following requirements are not necessarily _minimum requirements_. But if you try to run with less than this, beware that you might have performance issue.

#### Standard Hardware

For the full details of the standard hardware please see
[here](https://github.com/paritytech/substrate/pull/5848).

- **CPU** - 2 cores, with Intel SGX capability.
- **Storage** - A NVMe solid state drive. Should be reasonably sized to deal with blockchain growth.
  Starting around 80GB - 160GB will be okay for the first six months of Phala Network, but will need to
  be re-evaluated every six months.
- **Memory** - 2GB - 8GB. 2GB is really the minimum memory you should operate your Gatekeeper with, anything less than this make build times too inconvenient. For better performance you can bump it up to 4GB or 8GB, but anything more than that is probably over-kill. In order to compile the binary yourself you will likely need ~8GB.

The specs posted above are by no means the minimum specs that you could use when running a
Gatekeeper, however you should be aware that if you are using less you may need to toggle some extra
optimizations in order to be equal to other Gatekeepers that are running the standard.

### Install Intel SGX driver & platform software

You can find the latest Linux SGX driver from the [official download page](https://01.org/intel-software-guard-extensions/downloads). Make sure to install:

- SGX Linux DCAP Driver
- SGX Linux SDK (Under `/opt`)
- SGX Platform Swoftware

The [dockerfile](https://github.com/apache/incubator-teaclave-sgx-sdk/blob/253b3ac982b2d09d32f5fa5a2011e3c36bcbed1e/dockerfile/Dockerfile.1804.nightly) offered by Teaclave SGX SDK is a good reference of how to install the SGX SDK and platform software. Though it's possible to run SGX apps inside Docker, we don't have a guide for it so far. The driver must be installed on host machine natively.

### Install the `phala-node` Binary

Download the latest Phala Network binary from the Github [release page](https://github.com/Phala-Network/phala-blockchain/releases).

You can also build the `phala-node` binary from the
[Phala-Network/phala-blockchain](https://github.com/Phala-Network/phala-blockchain) repository on GitHub using the source
code available in the **master** branch. You will need to prepare the rust build environment described in the [Run a Full Node]({{< relref "docs/poc2/run-a-full-node" >}}) tutorial.

> Note: If you prefer to use SSH rather than HTTPS, you can replace the first line of the below with
> `git clone git@github.com/Phala-Network/phala-blockchain.git`.

```sh
  git clone https://github.com/Phala-Network/phala-blockchain
  cd phala-blockchain
  ./scripts/init.sh
  git submodule update --init
  cargo build --release
```

This step will take a while (generally 10 - 40 minutes, depending on your hardware).

> Note if you run into compile errors, you may have to switch to a less recent nightly. This can be
> done by running:
>
> ```sh
> rustup install nightly-2020-11-10
> rustup override set nightly-2020-11-10
> rustup target add wasm32-unknown-unknown --toolchain nightly-2020-11-10
> ```

If you are interested in generating keys locally, you can also install `subkey` from the same
directory. You may then take the generated `subkey` executable and transfer it to an air-gapped
machine for extra security.

```sh
cargo install --force --git https://github.com/paritytech/substrate subkey
```

### Synchronize Chain Data

> **Note:** By default, Gatekeeper nodes are in archive mode. If you've already synced the chain not
> in archive mode, you must first remove the database with `phala-node purge-chain` and then ensure
> that you run Phala Network with the `--pruning=archive` option.
>
> You may run a Gatekeeper node in non-archive mode by adding the following flags:
> `-unsafe-pruning --pruning OF BLOCKS>`, but note that an archive node and non-archive node's
> databases are not compatible with each other, and to switch you will need to purge the chain data.

You can begin syncing your node by running the following command:

```sh
./phala-node --pruning=archive
```

if you do not want to start in Gatekeeper mode right away.

The `--pruning=archive` flag is implied by the `--validator` and `--sentry` flags, so it is only
required explicitly if you start your node without one of these two options. If you do not set your
pruning to archive node, even when not running in Gatekeeper and sentry mode, you will need to
re-sync your database when you switch.

> **Note:** Gatekeepers should sync using the RocksDb backend. This is implicit by default, but can
> be explicit by passing the `--database RocksDb` flag. In the future, it is recommended to switch
> to using the faster and more efficient ParityDb option. Switching between database backends will
> require a resync.
>
> If you want to test out ParityDB you can add the flag `--database paritydb`.

Depending on the size of the chain when you do this, this step may take anywhere from a few minutes
to a few hours.

If you are interested in determining how much longer you have to go, your server logs (printed to
STDOUT from the `phala-node` process) will tell you the latest block your node has processed and
verified. You can then compare that to the current highest block via
[Telemetry](https://telemetry.polkadot.io/#list/Phala%20PoC-2) or the
[Phala Web App](https://app.phala.network/#/explorer).

> **Note:** If you do not already have PHA, this is as far as you will be able to go until the end
> of the soft launch period. You can still run a node, but you will need to have a minimal amount of
> PHA to continue, as balance transfers are disabled during the soft launch. Please keep in mind
> that even for those with PHA, they will only be indicating their _intent_ to validate; they will
> also not be able to run a Gatekeeper until the NPoS phase starts.

## Bond PHA

It is highly recommended that you make your controller and stash accounts be two separate accounts.
For this, you will create two accounts and make sure each of them have at least enough funds to pay
the fees for making transactions. Keep most of your funds in the stash account since it is meant to
be the custodian of your staking funds.

Make sure not to bond all your PHA balance since you will be unable to pay transaction fees from
your bonded balance.

It is now time to set up our Gatekeeper. We will do the following:

- Bond the PHA of the Stash account. These PHA will be put at stake for the security of the
  network and can be slashed.
- Select the Controller. This is the account that will decide when to start or stop validating.

First, go to the [Staking](https://app.phala.network/#/staking/actions) section. Click on
"Account Actions", and then the "Stash" button.

![dashboard bonding](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-bonding.jpg)

- **Stash account** - Select your Stash account. In this example, we will bond 100 milliPHA - make
  sure that your Stash account contains _at least_ this much. You can, of course, stake more than
  this.
- **Controller account** - Select the Controller account created earlier. This account will also
  need a small amount of PHA in order to start and stop validating.
- **Value bonded** - How much PHA from the Stash account you want to bond/stake. Note that you do
  not need to bond all of the PHA in that account. Also note that you can always bond _more_ PHA
  later. However, _withdrawing_ any bonded amount requires the duration of the unbonding period. On
  Phala Network, the unbonding period is 7 days.
- **Payment destination** - The account where the rewards from validating are sent. More info
  [here](https://wiki.polkadot.network/en/latest/polkadot/learn/staking/#reward-distribution).

Once everything is filled in properly, click `Bond` and sign the transaction with your Stash
account.

After a few seconds, you should see an "ExtrinsicSuccess" message. You should now see a new card
with all your accounts (note: you may need to refresh the screen). The bonded amount on the right
corresponds to the funds bonded by the Stash account.

## Set Session Keys

> **Note:** The session keys are consensus critical, so if you are not sure if your node has the
> current session keys that you made the `setKeys` transaction then you can use one of the two
> available RPC methods to query your node:
> [hasKey](https://polkadot.js.org/api/substrate/rpc.html#haskey-publickey-bytes-keytype-text-bool)
> to check for a specific key or
> [hasSessionKeys](https://polkadot.js.org/api/substrate/rpc.html#hassessionkeys-sessionkeys-bytes-bool)
> to check the full session key public key string.

Once your node is fully synced, stop the process by pressing Ctrl-C. At your terminal prompt, you
will now start running the node in validator mode with a flag allowing unsafe RPC calls, needed for
some advanced operations.

```sh
./phala-node --validator --name "name on telemetry"
```

You can give your Gatekeeper any name that you like, but note that others will be able to see it, and
it will be included in the list of all servers using the same telemetry server. Since numerous
people are using telemetry, it is recommended that you choose something likely to be unique.

### Generating the Session Keys

You need to tell the chain your Session keys by signing and submitting an extrinsic. This is what
associates your Gatekeeper node with your Controller account on Phala Network.

#### Option 1: PolkadotJS-APPS

You can generate your
[Session keys](https://wiki.polkadot.network/en/latest/polkadot/learn/keys/#session-key) in the
client via the apps RPC. If you are doing this, make sure that you have the PolkadotJS-Apps explorer
attached to your Gatekeeper node. You can configure the apps dashboard to connect to the endpoint of
your Gatekeeper in the Settings tab. If you are connected to a default endpoint hosted by Parity of
Web3 Foundation, you will not be able to use this method since making RPC requests to this node
would effect the local keystore hosted on a _public node_ and you want to make sure you are
interacting with the keystore for _your node_.

Once ensuring that you have connected to your node, the easiest way to set session keys for your
node is by calling the `author_rotateKeys` RPC request to create new keys in your Gatekeeper's
keystore. Navigate to Toolbox tab and select RPC Calls then select the author > rotateKeys() option
and remember to save the output that you get back for a later step.

![Explorer RPC call](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-explorer-rotatekeys-rpc.jpg)

#### Option 2: CLI

If you are on a remote server, it is easier to run this command on the same machine (while the node
is running with the default HTTP RPC port configured):

```sh
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9933
```

The output will have a hex-encoded "result" field. The result is the concatenation of the four
public keys. Save this result for a later step.

You can restart your node at this point, omitting the `--unsafe-rpc-expose` flag as it is no longer
needed.

### Submitting the `setKeys` Transaction

You need to tell the chain your Session keys by signing and submitting an extrinsic. This is what
associates your validator with your Controller account.

Go to [Staking > Account Actions](https://app.phala.network/#/staking/actions), and click "Set
Session Key" on the bonding account you generated earlier. Enter the output from `author_rotateKeys`
in the field and click "Set Session Key".

![staking-change-session](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/set-session-key-1.jpg)
![staking-session-result](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/set-session-key-2.jpg)

Submit this extrinsic and you are now ready to start validating.

## Register TEE worker

Gatekeepers utilize TEE to manage the secret keys in Phala Network. Before starting validating, you need to attach the TEE hardware to your Gatekeeper accounts. As long as the Gatekeeper is running and validating the blockchain, the TEE worker is always connected to the blockchain and support the network.

The TEE worker is handled by two components: `pHost` and `pRuntime`. The latest prebuilt binaries can be found from the Github [release page](https://github.com/Phala-Network/phala-blockchain/releases) in `tee-release.zip`. Instead, you can also compile it on your own.

Assuming you have extracted the prebuilt binares in a directory and have a running and fully synced `phala-node`, you can take the following steps to attach the TEE components to the blockchain:

1. Start pRuntime: `./app`
2. Start pHost with proper flags:
  ```
  ./phost \
        --mnemonic '<the-mnenomic-of-your-controller-account>' \
        --no-sync \
        --no-write-back \
        --remote-attestation \
        --substrate-ws-endpoint 'ws://localhost:9944'
  ```

The mnenomic of your controller account is needed because `phost` will send transactions to register your TEE hardware on behalve of your controller account.

The prebuilt release also includes `bridge.sh`, a convinient script to bring up `phost` in the same way described above. With the script file, you can save the mnemonic permanently, avoding typing it everytime you run it.

> Note: In Phala Network Testnet PoC-2, the TEE worker registration is an one-shot job. `phost` will exit right after a successful registeration. However in future version, since TEE supports the TEE network in the full life-time of a Gatekeeper, it must be always up and running until the Gatekeeper is retired.

## Validate

To verify that your node is live and synchronized, head to
[Telemetry](https://telemetry.polkadot.io/#list/Phala%20PoC-2) and find your node. Note that this
will show all nodes on the Phala Network, which is why it is important to select a unique name!

If everything looks good, go ahead and click on "Validate" in Phala Network UI.

![dashboard validate](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-validate.jpg)
![dashboard validate](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-validate-modal.jpg)

- **Payment preferences** - You can specify the percentage of the rewards that will get paid to you.
  The remaining will be split among your nominators.

Click "Validate".

> Note: This step will fail if you haven't successfully registered a TEE worker on your controller account. Please double check [Register TEE worker](#register-tee-worker) to make sure your TEE hardware is registered.

If you go to the "Staking" tab, you will see a list of active Gatekeepers currently running on the
network. At the top of the page, it shows the number of Gatekeeper slots that are available as well
as the number of nodes that have signaled their intention to be a Gatekeeper. You can go to the
"Waiting" tab to double check to see whether your node is listed there.

![staking queue](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-dashboard-staking.jpg)

The Gatekeeper set is refreshed every era. In the next era, if there is a slot available and your
node is selected to join the Gatekeeper set, your node will become an active Gatekeeper. Until then,
it will remain in the _waiting_ queue. If your Gatekeeper is not selected to become part of the
Gatekeeper set, it will remain in the _waiting_ queue until it is. There is no need to re-start if
you are not selected for the Gatekeeper set in a particular era. However, it may be necessary to
increase the number of PHA staked or seek out nominators for your Gatekeeper in order to join the
Gatekeeper set.

**Congratulations!** If you have followed all of these steps, and been selected to be a part of the
Gatekeeper set, you are now running a Phala Network Gatekeeper! If you need help, reach out on the
[Phala Network Telegram group](https://t.me/phalanetwork).


## FAQ

### Why am I unable to synchronize the chain with 0 peers?

![zero-peer](https://wiki.polkadot.network/docs/assets/guides/how-to-validate/polkadot-zero-peer.jpg)

Make sure to enable `30333` libp2p port. Eventually, it will take a little bit of time to discover
other peers over the network.

### How do I clear all my chain data?

```sh
./phala-node purge-chain
```

<br>

### Tech Support
[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/FUtZzYH) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/joinchat/PDXFHFI9RXcOKMaumhTTvw)
