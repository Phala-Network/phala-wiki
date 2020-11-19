---
title: "1.2 Install Docker-CE"
---

### Ubuntu 18.04

- Versions other than 18.04 are not considered in this tutorial, though other Linux distributions may work as well.
- [How to install Ubuntu 18.04](https://phoenixnap.com/kb/how-to-install-ubuntu-18-04)
- Make all your packages up-to-date: type `sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y` in the terminal.

> How to open your Terminal: right click on your desktop → `Open in Terminal`

### Install Docker-CE

1. Use commands as below in the terminal:
    - `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
    - `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`
    - `sudo apt-get install -y docker-ce docker-ce-cli containerd.io`
    - `sudo usermod -aG docker YOUR_UBUNTU_USERNAME` (Replace `YOUR_UBUNTU_USERNAME` with the username you are using)

This is how it looks like after successfully deployed👇.

![](/images/docs/poc3/1.2.png)

> Reference: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
