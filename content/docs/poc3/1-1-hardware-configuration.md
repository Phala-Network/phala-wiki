---
title: "1.1 Hardware Configuration"
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

## SGX Driver Verification

First, identify your driver type:
- Type in command `ls /dev | grep isgx` and if there comes a feedback, your computer is SGX-driver-driven and it works.
- Type in command `ls /dev | grep sgx` and if there comes a feedback, your computer is DCAP-driver-driven and it works.

As long as either type of driver works, your computer will be ready to mine PHA.

If neither of the types works, please type in the commands below to install DCAP driver:

```bash
sudo apt-get install dkms

wget https://download.01.org/intel-sgx/sgx-dcap/1.9/linux/distro/ubuntu18.04-server/sgx_linux_x64_driver_1.36.2.bin

chmod +x sgx_linux_x64_driver_1.36.2.bin

sudo ./sgx_linux_x64_driver_1.36.2.bin
```

After installation, retype `ls /dev | grep sgx` to check whether the installation succeeds. If the DCAP driver is not enabled, please try following commands to install SGX driver.

```bash
wget https://download.01.org/intel-sgx/sgx-linux/2.11/distro/ubuntu18.04-server/sgx_linux_x64_driver_2.6.0_b0a445b.bin

chmod +x sgx_linux_x64_driver_2.6.0_b0a445b.bin

sudo ./sgx_linux_x64_driver_2.6.0_b0a445b.bin
```
Use `ls /dev | grep isgx` again to ensure that SGX driver works.

<br>

## Mainboard Verification

After the installation of your driver, there are two more items to be verified on your mainboard:

1. the **Production Mode**
2. the **FLC (Flexible Launch Control)**

Among them, **the former is a must to enble Phala pRuntime docker**. If it's not supported (tagged as ✘ in the report example below), we are afraid you can't mine PHA with this mainboard.

The latter is not a must though, it is suggested to be checked as it would be essential to install the DCAP driver. And DCAP driver can be a necessary requirement to mine PHA in the future.

If you can't run Phala pRuntime docker with both of them tagged as ✔, you may have to check whether your BIOS is the latest version with latest security patches.
If you still can't run Phala pRuntime docker with the latest BIOS of your mainboard manufacturer, we are afraid you can't mine PHA with this mainboard.

To testify your mainboard, please type in the following command for DCAP driver:

```bash
docker run -ti --rm --name phala-sgx_detect --device /dev/sgx/enclave --device /dev/sgx/provision phalanetwork/phala-sgx_detect
```

or the following one for SGX driver:

```bash
docker run -ti --rm --name phala-sgx_detect --device /dev/isgx phalanetwork/phala-sgx_detect
```

The report below would be a positive result:

```
Detecting SGX, this may take a minute...
✔  SGX instruction set
  ✔  CPU support
  ✔  CPU configuration
  ✔  Enclave attributes
  ✔  Enclave Page Cache
  SGX features
    ✔  SGX2  ✔  EXINFO  ✘  ENCLV  ✘  OVERSUB  ✘  KSS
    Total EPC size: 94.0MiB
✔  Flexible launch control
  ✔  CPU support
  ？ CPU configuration
  ✔  Able to launch production mode enclave
✔  SGX system software
  ✔  SGX kernel device (/dev/sgx/enclave)
  ✔  libsgx_enclave_common
  ✔  AESM service
  ✔  Able to launch enclaves
    ✔  Debug mode
    ✔  Production mode
    ✔  Production mode (Intel whitelisted)

You're all set to start running SGX programs!
```

If your got a report as below, please screenshort it, and send it to [Phala developers](https://discord.gg/zjdJ7d844d) for help:

```
Detecting SGX, this may take a minute...
✔  SGX instruction set
  ✔  CPU support
  ✔  CPU configuration
  ✔  Enclave attributes
  ✔  Enclave Page Cache
  SGX features
    ✘  SGX2  ✘  EXINFO  ✘  ENCLV  ✘  OVERSUB  ✘  KSS
    Total EPC size: 94.0MiB
✘  Flexible launch control
  ✔  CPU support
  ✘  CPU configuration
✔  SGX system software
  ✔  SGX kernel device (/dev/isgx)
  ✔  libsgx_enclave_common
  ✔  AESM service
  ✔  Able to launch enclaves
    ✔  Debug mode
    ✘  Production mode
    ✔  Production mode (Intel whitelisted)

🕮  Flexible launch control > CPU configuration
Your hardware supports Flexible Launch Control, but it's not enabled in the BIOS. Reboot your machine and try to enable FLC in your BIOS. Alternatively, try updating your BIOS to the latest version or contact your BIOS vendor.

debug: MSR 3Ah IA32_FEATURE_CONTROL.SGX_LC = 0

More information: https://edp.fortanix.com/docs/installation/help/#flc-cpu-configuration

🕮  SGX system software > Able to launch enclaves > Production mode
The enclave could not be launched. This might indicate a problem with FLC.

debug: failed to load report enclave
debug: cause: failed to load report enclave
debug: cause: The EINITTOKEN provider didn't provide a token
debug: cause: aesm error code GetLicensetokenError_6

More information: https://edp.fortanix.com/docs/installation/help/#run-enclave-prod

You're all set to start running SGX programs!
```


<br>
<br>
<br>

---

#### Reference

1. [What is Trusted Execution Environment](https://www.trustonic.com/technical-articles/what-is-a-trusted-execution-environment-tee/)
2. [What is SGX](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions.html)

#### Tech Support

[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zzhfUjU) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)
