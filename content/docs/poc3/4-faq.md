---
title: "4 Frequently Asked Questions"
---

### Get connected

Please don't hesitate to ask us if you have any questions. You can find us at:

- Discord #miner channel: <https://discord.gg/zjdJ7d844d>
- Telegram miner group: <https://t.me/phalaminer>

### Q: It says "no such command" after I typed `docker pull docker.pkg.github.com/phala-network/phala-docker/phala-node:pre-test-4` 

Please check the installation of Docker-CE.

### Q: It says "permission denied"

Add `sudo` in front of your commands. 

### Q: How to check if my installed driver is SGX or DCAP

- Run `ls /dev/isgx` and if the file exists: SGX-driver
- Run `ls /dev/sgx` and if the directory exists: DCAP-driver

### Q: Where are the code releases / native build? How can I run it natively instead of using a Docker?

We didn’t prepare a native binary release, though theoretically the Docker image contains the native binary for Ubuntu 18.04. There are three core components in Phala Network: the full node, the bridge (phost), and pRuntime. You can build the first two from the source directly (master branch, maybe we will create a dedicated release branch like "poc3" soon).

pRuntime can also be build from the source. However, your own pRuntime build cannot join the network because only the official builds are whitelisted on the blockchain. The reason behind is that the official builds are signed with Intel’s SGX certificates. We understand it's important that everyone should be able to reproduce the binary from the source. So in the future, we would like to publish the detached signature on Github so that everyone can reproduce the exact same build as ours, and therefore make sure the binary actually comes from the open source code. There are existing reproducible build systems like Bitcoin Core's Gitian builder that can do the exact same thing as we just described.

In the future, we should also release the binary build (with PGP signatures) directly on the Github release page.
