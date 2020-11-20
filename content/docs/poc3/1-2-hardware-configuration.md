---
title: "1.2 Hardware Configuration"
---

![](https://imgur.com/yhzVlUs.png)

How to check whether your device is SGX-supported:

- [Whether your hardware CPU is SGX-supported](https://forum.phala.network/t/how-to-check-whether-your-cpu-is-sgx-supported/1252)

<br>

## BIOS Settings

1. Google how to enter your BIOS Settings.
2. Enter BIOS after reboot.
3. Go to `Security` -> `Secure Boot`, set it to `Disabled`.
4. Go to `Security` -> `SGX`, set it to `Enabled` (or any other option that is similar as the description varies according to different manufacturers.)
5. Go to `Boot` -> `Boot Mode`, set it to `UEFI`.
6. Press `F10` and wait for rebooting.

<br>

## System: Ubuntu 18.04

- Versions other than 18.04 are not considered in this tutorial, though other Linux distributions may work as well.
- [How to install Ubuntu 18.04](https://phoenixnap.com/kb/how-to-install-ubuntu-18-04)
- Make all your packages up-to-date: type `sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y` in the terminal.

> How to open your Terminal: right click on your desktop → `Open in Terminal`

<br>
<br>
<br>

---

### Reference

1. [What is Trusted Execution Environment](https://www.trustonic.com/technical-articles/what-is-a-trusted-execution-environment-tee/)
2. [What is SGX](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions.html)

### Tech Support

[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zzhfUjU) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)
