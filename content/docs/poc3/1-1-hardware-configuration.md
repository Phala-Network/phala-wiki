---
title: "1.1 Hardware Configuration"
---

![](/images/docs/poc3/1.1.png)

How to check whether your device supports SGX:

- [Whether your hardware CPU is SGX-supported](https://forum.phala.network/t/how-to-check-whether-your-cpu-is-sgx-supported/1252)

## BIOS Settings

1. Google how to enter your BIOS Settings.
2. Enter BIOS after reboot.
3. Go to `Security` -> `Secure Boot`, set it to `Disabled`.
4. Go to `Security` -> `SGX`, set it to `Enabled` or `Software Control` (the name of the option may vary according to the different manufacturers)
5. Go to `Boot` -> `Boot Mode`, and make sure its set to `UEFI`. (You also need to make sure the Linux is installed in UEFI mode)
6. Save and reboot.

## SGX Driver Verification

First, check if you already have some type of the driver installed:

- Type in command `ls /dev/isgx` and if the file exists: you have SGX driver installed
- Type in command `ls /dev/sgx` and if the directory exists: you have DCAP driver installed

As long as either type of driver works, your computer will be ready to mine PHA.

If neither of the types works, please try to install the DCAP driver first. Type in the commands below to install DCAP driver:

```bash
sudo apt-get install dkms
wget https://download.01.org/intel-sgx/sgx-dcap/1.9/linux/distro/ubuntu18.04-server/sgx_linux_x64_driver_1.36.2.bin
chmod +x sgx_linux_x64_driver_1.36.2.bin
sudo ./sgx_linux_x64_driver_1.36.2.bin
```

After installation, retry `ls /dev/sgx` to check whether the installation succeeds. If the DCAP driver doesn't work, please **uninstall the DCAP driver** and then switch to the SGX driver. To uninstall the DCAP driver, run:

```bash
sudo /opt/intel/sgxdriver/uninstall.sh
```

Then try following commands to install the SGX driver:

```bash
wget https://download.01.org/intel-sgx/sgx-linux/2.11/distro/ubuntu18.04-server/sgx_linux_x64_driver_2.6.0_b0a445b.bin
chmod +x sgx_linux_x64_driver_2.6.0_b0a445b.bin
sudo ./sgx_linux_x64_driver_2.6.0_b0a445b.bin
```

Run `ls /dev/isgx` again to check if the SGX driver works.

## Double check the SGX capability

After the installation of your driver, please use the following utility to double check if everything goes well. Docker is required.

- If you are using the DCAP driver:

  ```bash
  docker run -ti --rm --name phala-sgx_detect --device /dev/sgx/enclave --device /dev/sgx/provision phalanetwork/phala-sgx_detect
  ```

- If you are using the SGX driver:

  ```bash
  docker run -ti --rm --name phala-sgx_detect --device /dev/isgx phalanetwork/phala-sgx_detect
  ```

- Even if there's no `/dev/sgx` or `/dev/isgx`, you can still run the utility to collect some diagnose information:

  ```bash
  docker run -ti --rm --name phala-sgx_detect phalanetwork/phala-sgx_detect
  ```

Please pay attention to the following checks:

1. SGX system software → Able to launch enclaves → `Production Mode`
2. Flexible launch control → `Able to launch production mode enclave`

Among them, **the former one is a must to run Phala Network pRuntime**. If it's not supported (tagged as ✘ in the report example below), we are afraid you can't mine PHA with this setup. You may want to replace the motherboard and/or the CPU.

The latter is not a must though, it is suggested to be checked as it would be essential to install the DCAP driver. The DCAP driver is future-proof and thus more recommended.

The report below would be a positive result:

```txt
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

If your got a report like below, please screenshot it, and send it to [Phala Discord Server](https://discord.gg/zjdJ7d844d) or [Telegram Miner Group](https://t.me/phalaminer) for help.

```txt
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
```

If you can't run Phala pRuntime with both of them tagged as ✔, you may have to check whether your BIOS is the latest version with latest security patches. If you still can't run Phala pRuntime docker with the latest BIOS of your motherboard manufacturer, we are afraid you can't mine PHA with this motherboard.

---

#### Reference

1. [What is Trusted Execution Environment](https://www.trustonic.com/technical-articles/what-is-a-trusted-execution-environment-tee/)
2. [What is SGX](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions.html)

#### Tech Support

[![](https://img.shields.io/discord/697726436211163147?label=Phala%20Discord)](https://discord.gg/zzhfUjU) [![](https://img.shields.io/badge/Join-Telegram-blue)](https://t.me/phalaminer)
