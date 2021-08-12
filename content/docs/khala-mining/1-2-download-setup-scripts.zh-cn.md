---
title: "1.2 Install Phala Tools"
---

## Prerequisites

Before you go further, please ensure that your have correct setup your hardware, BIOS and operating system according to the [previous section]({{< relref "docs/khala-mining/1-1-hardware-requirements">}}).

## Download Phala Scripts

The Phala tools are availbale at https://github.com/Phala-Network/solo-mining-scripts/archive/poc5.zip, it can be downloaded with `wget` by executing the following commands in the terminal:

```bash
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
sudo apt install wget unzip
cd ~
wget https://github.com/Phala-Network/solo-mining-scripts/archive/poc5.zip
unzip poc5.zip
```

## Install Phala Tools

Execute the following commands in your terminal:

```bash
cd ~/solo-mining-scripts-poc5
chmod +x install.sh
sudo ./install.sh en
```
> This script will install the docker, sgx driver and pull all the Phala docker images.

Congratulation! You have successfully installed needed Phala tools.
