---
title: "1.1 Hardware Configuration"
---

![](/images/docs/poc3/1.1.png)

How to check whether your device is SGX-supported:

- [Whether your hardware CPU is SGX-supported](https://forum.phala.network/t/how-to-check-whether-your-cpu-is-sgx-supported/1252)
- Refer to [the SGX Hardware List](https://github.com/Phala-Network/phala-docs/wiki/SGX-Hardware-List)
- Take [the SGX Test](https://forum.phala.network/t/30-pha-for-each-passed-device-motherboard-sgx-testing-hardware/1024/3)

## BIOS Settings

1. Google how to enter your BIOS Settings.
2. Enter BIOS after reboot.
3. Go to `Security` -> `Secure Boot`, set it to `Disabled`.
4. Go to `Security` -> `SGX`, set it to `Enabled` (or any other option that is similar as the description varies according to different manufacturers.)
5. Go to `Boot` -> `Boot Mode`, set it to `UEFI`.
6. Press `F10` and wait for rebooting.

## Reference

1. [What is Trusted Execution Environment](https://www.trustonic.com/technical-articles/what-is-a-trusted-execution-environment-tee/)
2. [What is SGX](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions.html)