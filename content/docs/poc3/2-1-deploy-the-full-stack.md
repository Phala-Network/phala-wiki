---
title: "2.1 Deploy the Full Stack"
---

> **WARNING**: Operations at this step may reset your previous Phala blockchain data and you will need to re-deploy the docker containers. For more detail, refer to our [Dockerfile Github repo](https://github.com/Phala-Network/phala-docker#usage). You can find the latest Phala Network docker images from [the official repo](https://hub.docker.com/orgs/phalanetwork/repositories).
>
> PLEASE follow the order of deploying the three docker containers below.

## Deploying Phala full-node

A Phala full-node helps maintain the backbone of the network. We suggest you to run a Phala full-node on a device with 50+ GB storage.

> What is a [full-node](https://wiki.phala.network/en-us/docs/poc2/run-a-full-node/)

Open the Terminal, and use the commands as follow. Your node name will be set by the second line.

1. `sudo docker pull phalanetwork/phala-poc3-node` (to pull a docker file)
2. `sudo docker run -ti --rm --name phala-node -d -e NODE_NAME="YOUR_NODE_NAME" -p 9933:9933 -p 9944:9944 -p 30333:30333 -v $HOME/phala-node-data:/root/data phalanetwork/phala-poc3-node` (to run the docker file; please repalce `YOUR_NODE_NAME` with your own name, only alhpabets, digits and whitespace are supported)

![](/images/docs/poc3/2.1-1.png)

![](/images/docs/poc3/2.1-2.png)

> The docker container will start after these two commands. To stop or restart it, please refer to [this tutorial](https://github.com/Phala-Network/phala-docker#usage).
>
> You can stop the full node by `sudo docker kill phala-node`. Please avoid irregular turning down of the container （e.g., turn off or reboot your computer without stopping it). It may cause damage to the blockchain database, which can only be solved by re-sync of the entire blockchain.
>
> If you are planning to run multiple full-nodes on one device (which is not suggested), please set different database path for each node by changing `$HOME/phala-node-data` to some other directory.

## Deploying pRuntime

1. `sudo docker pull phalanetwork/phala-poc3-pruntime`
2. `sudo docker run -d -ti --rm --name phala-pruntime -p 8000:8000 -v $HOME/phala-pruntime-data:/root/data --device /dev/isgx phalanetwork/phala-poc3-pruntime`

![](/images/docs/poc3/2.1-3.png)

> If there is no output, please refer to [the FAQ]({{< relref "docs/poc3/4-faq" >}}).
>
> `$HOME/phala-pruntime-data` is the default path to the data of pRuntime. It's set in the `phala-pruntime-data` folder by default, which can be changed to another folder that the system admin has full access to. To run multiple pRuntime containers on one device (which is unnecessary and not suggested), please set different path for each container.
>
> If the data was deleted, please re-register the miner from the beginning.

## Deploying pHost

Please replace the `YOUR-CONTROLLER-MNEMONIC` in the second command with the mnemonic of your controller. If you didn't fill in mnemonic at this step, you would receive "**NotController**" error when submitting transactions related to `phalaModule: setStash(controller)`.

It costs a tiny amount of tPHA as well. Before deploying pHost, please make sure there are a certain amount of tPHA in your Stash account and controller account. [Click here to learn how to obtain tPHA.](https://forum.phala.network/t/how-to-obtain-tpha-on-testnet-vendetta/1254)

1. `sudo docker pull phalanetwork/phala-poc3-phost`
2. `sudo docker run -d -ti --rm --name phala-phost -e PRUNTIME_ENDPOINT="http://IP-ADDRESS:8000" -e PHALA_NODE_WS_ENDPOINT="ws://IP-ADDRESS:9944" -e MNEMONIC="YOUR-CONTROLLER-MNEMONIC" -e EXTRA_OPTS="-r" phalanetwork/phala-poc3-phost`

![](/images/docs/poc3/2.1-4.png)

> The docker contanier will start to run after the input of these two commands. To stop or restart it, please refer to [the tutorial](https://github.com/Phala-Network/phala-docker#usage).
>
> If you reboot the pRuntime container, you will have to reboot the pHost as well.
>
> `http://IP-ADDRESS:8000` is the address listened by pRuntime. And `ws://IP-ADDRESS:9944` is the WebSocker address of Phala full-node. They have to replace `IP-ADDRESS` by the IP address of your host machine instead of the localhost (e.g. `127.0.0.1`) You can find your IP address with `ip addr` command.
