---
title: "1.2 Software Configuration"
---

### System Requirement

- This tutorial is based on Ubuntu 18.04. Ubuntu with versions other than 18.04 are not covered in this tutorial, though they and other Linux distributions may also work as well.
- [How to install Ubuntu 18.04](https://phoenixnap.com/kb/how-to-install-ubuntu-18-04)
- Make sure wget is installed all your packages are up-to-date:

```bash
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
sudo apt install wget
```

### Install Phala-solo-Scripts

Download the Phala-solo-Scripts form Github

```bash
wget https://codeload.github.com/Phala-Network/solo-mining-scripts/zip/main.zip
```

Unzip and run the install.sh to install the scripts
A Phala miner helps maintain the backbone of the network. We suggest you to run a Phala miner on a device with 50+ GB storage.
Your node name ，IP address and controllor mnemonic will be set by the scripts.

```bash
unzip main.zip
cd solo-mining-scripts-main
sudo chmod +x *.sh
sudo ./install.sh en
sudo phala install init
```
> The scripts will install the docker,sgx driver and pull all the Phala docker images

> ## Double check the SGX capability

After the installation of your driver, please use the following utility to double check if everything goes well. Docker is required for this step, and you can peek at how to install it in the [next secton]({{< relref "docs/poc3/1-2-software-configuration" >}}).

- You can run the SGX test by the Phala scripts

  ```bash
  sudo phala sgx-test
  ```

Please pay attention to the following checks:

1. SGX system software → Able to launch enclaves → `Production Mode`
2. Flexible launch control → `Able to launch production mode enclave`

Among them, **the former one is a must to run Phala Network pRuntime**. If it's not supported (tagged as ✘ in the report example below), we are afraid you can't mine PHA with this setup. You may want to replace the motherboard and/or the CPU.

The latter is not a must though, it is suggested to be checked as it would be essential to install the DCAP driver.

The report below would be a positive result:

```txt
Detecting SGX, this may take a minute...
aesm_service[15]: Malformed request received (May be forged for attack)
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
  ？ CPU configuration
  ✘  Able to launch production mode enclave
✔  SGX system software
  ✔  SGX kernel device (/dev/isgx)
  ✔  libsgx_enclave_common
  ✔  AESM service
  ✔  Able to launch enclaves
    ✔  Debug mode
    ✘  Production mode
    ✔  Production mode (Intel whitelisted)

🕮  Flexible launch control > CPU configuration
Your hardware supports Flexible Launch Control, but whether it's enabled could not be determined. More information might be available by re-running this program with sudo. Would you like to do that?
(not supported yet)

debug: Error reading MSR 3Ah: Failed to load MSR kernel module
debug: cause: Failed to load MSR kernel module
debug: cause: Failed executing modprobe
debug: cause: No such file or directory (os error 2)

More information: https://edp.fortanix.com/docs/installation/help/#flc-cpu-configuration

🕮  SGX system software > Able to launch enclaves > Production mode
The enclave could not be launched. This might indicate a problem with FLC.

debug: failed to load report enclave
debug: cause: failed to load report enclave
debug: cause: The EINITTOKEN provider didn't provide a token
debug: cause: aesm error code GetLicensetokenError_6

More information: https://edp.fortanix.com/docs/installation/help/#run-enclave-prod

You're all set to start running SGX programs!
Generated machine id:
[162, 154, 220, 15, 163, 137, 184, 233, 251, 203, 145, 36, 214, 55, 32, 54]

Testing RA...
aesm_service[15]: [ADMIN]EPID Provisioning initiated
aesm_service[15]: The Request ID is 09a2bed647d24f909d4a3990f8e28b4a
aesm_service[15]: The Request ID is 8d1aa4104b304e12b7312fce06881260
aesm_service[15]: [ADMIN]EPID Provisioning successful
isvEnclaveQuoteStatus = GROUP_OUT_OF_DATE
platform_info_blob { sgx_epid_group_flags: 4, sgx_tcb_evaluation_flags: 2304, pse_evaluation_flags: 0, latest_equivalent_tcb_psvn: [15, 15, 2, 4, 1, 128, 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 11, 0], latest_pse_isvsvn: [0, 11], latest_psda_svn: [0, 0, 0, 2], xeid: 0, gid: 2919956480, signature: sgx_ec256_signature_t { gx: [99, 239, 225, 171, 96, 219, 216, 210, 246, 211, 20, 101, 254, 193, 246, 66, 170, 40, 255, 197, 80, 203, 17, 34, 164, 2, 127, 95, 41, 79, 233, 58], gy: [141, 126, 227, 92, 128, 3, 10, 32, 239, 92, 240, 58, 94, 167, 203, 150, 166, 168, 180, 191, 126, 196, 107, 132, 19, 84, 217, 14, 124, 14, 245, 179] } }
advisoryURL = https://security-center.intel.com
advisoryIDs = "INTEL-SA-00219", "INTEL-SA-00289", "INTEL-SA-00320", "INTEL-SA-00329"
```

If your got a report like below, please screenshot it, and send it to [Phala Discord Server](https://discord.gg/zjdJ7d844d) or [Telegram Miner Group](https://t.me/phalaminer) for help.

```txt
Detecting SGX, this may take a minute...
✔  SGX instruction set
  ✔  CPU support // if tagged with ❌: it does not suppoort SGX function, you would need to use other types of CPU.
  ✔  CPU configuration // if tagged with ❌: you would need to check BIOS updates.
  ✔  Enclave attributes // if tagged with ❌: probably caused by [CPU support issue] and [CPU configuration]
  ✔  Enclave Page Cache // if tagged with ❌: probably caused by [CPU support issue] and [CPU configuration]
  SGX features
    ✘  SGX2  ✘  EXINFO  ✘  ENCLV  ✘  OVERSUB  ✘  KSS // It's OK if SGX2 was tagged with ❌. Phala has not integrated with SGX2 technology in the current stage.
    Total EPC size: 94.0MiB
✘  Flexible launch control
  ✔  CPU support
  ✘  CPU configuration // if tagged with ❌: you can give it a try but your miner might be affected when the SGX driver upgrades in the future.
✔  SGX system software
  ✔  SGX kernel device (/dev/isgx)
  ✔  libsgx_enclave_common
  ✔  AESM service
  ✔  Able to launch enclaves
    ✔  Debug mode
    ✘  Production mode // if tagged with ❌: you would need to check BIOS updates.
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

If you can't run Phala pRuntime with both of them tagged as ✔, you may have to check whether your BIOS is the latest version with latest security patches. If you still can't run Phala pRuntime docker with the latest BIOS of your motherboard manufacturer, we are afraid you can't mine PHA for now with this motherboard.

---