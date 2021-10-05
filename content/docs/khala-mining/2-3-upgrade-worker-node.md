---
title: "2.3 Upgrade Worker Node"
---

To upgrade your worker requires it to stop first. Note that you may not want to stop mining on the blockchain (via Phala App). This command only stops the software rather than the on-chain state.

```bash
sudo phala stop
```

The worker node can be updated in place:

```bash
sudo phala update
```

Finally, restart your worker with:

```bash
sudo phala start
```

### Update and wipe data 

This command will update the docker images AND removes all the saved data.

{{< tip "warning" >}}
This operation is dangerous because it removes the worker identity, causing the worker pubkey changed. When the identity key is lost, your miner will become unresponsive and bear the risk of slashing. You will need to manually stop the original worker, wait for a full cool down period, and then withdraw your stake and start over. This command also removes all the blockchain history, which takes a few days to sync. Unless you understand what you are doing, please don't run a clean update.
{{< /tip >}}

```bash
sudo phala update clean
```