---
title: Self-examination
---

To further identify the issue, there are two more items to be verified on your mainboard:

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
