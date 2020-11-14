---
title: "3.5 Upgrade Miner"
---

## Stop Mining

> If your miner is not mining, please go to step 2.

1. Refer to [this link](https://hackmd.io/@phala/B1jP8rhtP) to stop mining.
2. Run the following commands:

    ```bash
    docker kill phala-phost
    docker kill phala-pruntime
    docker kill phala-node
    sudo docker image prune -a  # type `y` to confirm the deletion
    ```


## Reset Docker Containers

1. `sudo rm -rf /PATH/TO/YOUR/phala-node-data`
    - By default, the Phala node data is stored under `$HOME/phala-node-data`
2. Type commands as follows

    ```bash
    sudo docker pull jasl123/phala-poc3-node
    sudo docker pull jasl123/phala-poc3-pruntime
    sudo docker pull jasl123/phala-poc3-phost
    ```

3. Type commands as follows:

    ```bash
    # start the full node
    sudo docker run -ti --rm --name phala-node -d -e NODE_NAME=YOUR_NODE_NAME -p 9933:9933 -p 9944:9944 -p 30333:30333 -v $HOME/phala-node-data:/root/data jasl123/phala-poc3-node
    # start pRuntime
    sudo docker run -d -ti --rm --name phala-pruntime -p 8000:8000 -v $HOME/phala-pruntime-data:/root/data --device /dev/isgx jasl123/phala-poc3-pruntime
    # start the bridge (wait for 30s after started the full node and pRuntime)
    sudo docker run -d -ti --rm --name phala-phost -e PRUNTIME_ENDPOINT="http://IP-ADDRESS:8000" -e PHALA_NODE_WS_ENDPOINT="ws://IP-ADDRESS:9944" -e MNEMONIC="THE-MNEMONIC-OF-YOUR-CONTROLLER" -e EXTRA_OPTS="-r" jasl123/phala-poc3-phost
    ```
    
## Restart Mining

Refer to [3.2 Start Mining]({{< relref "docs/poc3/3-2-start-mining" >}}).
