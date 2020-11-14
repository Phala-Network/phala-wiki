---
title: "4 Frequently Asked Questions"
---

### Q: It says "no such command" after I typed `docker pull docker.pkg.github.com/phala-network/phala-docker/phala-node:pre-test-4` 

Please check the installation of Docker-CE.

### Q: It says "permission denied"

Add `sudo` in front of your commands. 

### Q: How to check if my installed driver is SGX or DCAP

- Run `ls /dev/isgx` and if the file exists: SGX-driver
- Run `ls /dev/sgx` and if the directory exists: DCAP-driver
