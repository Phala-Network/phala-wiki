---
title: "1.2 Software Configuration"
---

### Ubuntu 18.04

- Versions other than 18.04 are not considered in this tutorial, though other Linux distribution may be supported as well.
- [How to intsall](https://phoenixnap.com/kb/how-to-install-ubuntu-18-04)
- Make all your packages up-to-date: type `sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y` in the terminal.

> How to open your Terminal: right click on your desktop → `Open in Terminal`

### Install the SGX driver

If you have already installed the SGX driver, you can verify by:

1. Type  `ls /dev | grep sgx` in the terminal
2. If there's any output, your SGX driver is ready.

If there's no output, you have to [**download and install it**](https://01.org/intel-software-guard-extensions/downloads). And verify it again. For FLC ready CPUs and motherboards, the DCAP driver is preferred. Othrewise insall the SGX driver.

### Install Docker-CE

1. Use commands as below in the terminal:
    - `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
    - `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`
    - `sudo apt-get install -y docker-ce docker-ce-cli containerd.io`
    - `sudo usermod -aG docker [YOUR_UBUNTU_USERNAME]` (Replace "[YOUR_UBUNTU_USERNAME]" to the user name you are using, without the brackets)

This is how it looks like after it's successfully deployed👇.

![](/images/docs/poc3/1.2.png)

> Reference: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
