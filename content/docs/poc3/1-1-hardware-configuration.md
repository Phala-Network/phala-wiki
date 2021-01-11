---
title: "1.1 Hardware Requirement and Configuration"
---

Phala Network operates on trusted hardwares, in this case, Intel CPUs which support SGX feature. SGX was first introduced in 2015 with the sixth generation Intel Core microprocessors based on the Skylake microarchitecture. In practice, the support of SGX feature also relies on your motherboard and BIOS configurations.
In this section, we will first ensure that necessary hardware requirements are satisfied and all your hardwares are correcly configured.

## General Hardware Requirements
![](/images/docs/poc3/1.2.png)

### CPU Requirement

You can refer to the following tutorial to ensure that your CPU is SGX-supported.

- [Whether your CPU is SGX-supported](https://forum.phala.network/t/how-to-check-whether-your-cpu-is-sgx-supported/1252)

### BIOS Settings

First of all, we recommand to update your BIOS to the latest version.

1. **Enter BIOS**. The method varies for different motherboards, and Google is always at your service about this.
3. **Disable Secure Boot**. Go to `Security` -> `Secure Boot`, set it to `Disabled`.
4. **Enable SGX Extension**. Go to `Security` -> `SGX` (This name  may vary according to the different manufacturers), set it to `Enabled`.
>If you can only find `SGX: Software Controlled` option, you will have to run [sgx-software-enable](https://github.com/intel/sgx-software-enable) in Ubuntu. You can follow the Intel's instructions, build it from source and execute it. Also, we provide a prebuilt file for Ubuntu 18.04 / 20.04 that can be found [here](https://github.com/Phala-Network/sgx-tools/releases/tag/0.1). You can download and execute it with the following commands:
> ```bash
> wget https://github.com/Phala-Network/sgx-tools/releases/download/0.1/sgx_enable
> chmod +x sgx_enable
> sudo ./sgx_enable
> ```
5. **Use UEFI Boot**. Go to `Boot` -> `Boot Mode`, and make sure it was set to `UEFI`.
6. Save and reboot.

You also need to make sure that Ubuntu is installed in UEFI mode. SGX is not guaranteed to work properly on the OS installed in legacy mode. In such case you may want to reinstall the system.


#### Reference

1. [What is Trusted Execution Environment](https://www.trustonic.com/technical-articles/what-is-a-trusted-execution-environment-tee/)
2. [What is SGX](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions.html)

#### Miner Community

[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zzhfUjU) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)
