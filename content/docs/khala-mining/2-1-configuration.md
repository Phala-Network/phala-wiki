---
title: "2.1 Configuration"
---

{{< tip >}}
If you have successfully installed the SGX driver and finished the benchmarking, you can skip the following tutorials.
{{< /tip >}}

## Install SGX Driver

You can reinstall the DCAP driver with

```bash
sudo phala install dcap
```

or reinstall the isgx driver with

```shell
sudo phala install isgx
```


## Install

You can use the following commands to install Phala tools. It will automatically set the number of CPU cores to use, node name, gas fee account mnemonic and pool owner account.

```bash
sudo phala install
```

If you want to manually config the tools, use the following commands and set the parameters.

```bash
sudo phala config set
```

> The script will ask for re-enter if the received parameter is invalid.
> To ensure the proceeding of mining, the balance of gas fee account should be greater than 0.1 PHA.

You can get the current parameters in use with

```bash
sudo phala config show
```
