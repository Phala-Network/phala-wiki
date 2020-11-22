---
title: "1.2 Software Configuration"
---

### System Requirement

- This tutorial is based on Ubuntu 18.04. Ubuntu with versions other than 18.04 are not covered in this tutorial, though they and other Linux distributions may also work as well.
- [How to install Ubuntu 18.04](https://phoenixnap.com/kb/how-to-install-ubuntu-18-04)
- Make sure all your packages are up-to-date:

```bash
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
```

### Install Docker-CE

Uninstall the older versions of Docker if they exist

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Run the commands in the terminal to install the latest Docker

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

If you would like to run docker without `sudo` in the future, add yourself to the `docker` group

```bash
# Replace YOUR_UBUNTU_USERNAME with the username you are using
sudo usermod -aG docker YOUR_UBUNTU_USERNAME
```

This is how it looks like after successfully deployed👇.
![](/images/docs/poc3/1.1.png)

> Reference: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
