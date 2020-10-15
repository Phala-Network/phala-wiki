---
title: Troubleshooting
weight: 4
---

## The "duplicate lang item" problem

```
error: duplicate lang item in crate `std`: `f32_runtime`.
  |
  = note: the lang item is first defined in crate `sgx_tstd` (which `enclaveapp` depends on)

error: duplicate lang item in crate `std`: `f64_runtime`.
  |
  = note: the lang item is first defined in crate `sgx_tstd` (which `enclaveapp` depends on)

error: duplicate lang item in crate `std`: `panic_impl`.
  |
  = note: the lang item is first defined in crate `sgx_tstd` (which `enclaveapp` depends on)

error: duplicate lang item in crate `std`: `begin_panic`.
  |
  = note: the lang item is first defined in crate `sgx_tstd` (which `enclaveapp` depends on)

error: duplicate lang item in crate `std`: `oom`.
  |
  = note: the lang item is first defined in crate `sgx_tstd` (which `enclaveapp` depends on)

error: aborting due to 6 previous errors
```

This is due to accidentally introduced "std" dependencies. Usually there are two common causes of this error.

Most likely the code has some syntax errors and cannot build. Sometimes these errors can confuse the rust compiler in "no_std" mode and the compiler may accidentally introduce some random dependencies, which breaks our SDK. If this is the case, scroll up and fix the other compiling errors, and then this error should disappear.

If fixing the other errors doesn't help, you should check if you accidentally introduce the "std" dependency to the runtime code. The hardware enclave sdk Phala is using doesn't allow direct or indirect "std" dependencies. You may consider to switch to a package that supports "no_std" and disable its std feature in `Cargo.toml`, or manually port the dependency package to use `teaclave-sgx-sdk`'s `tstd` instead. `tstd` is a subset of `std` but it's enough in the most cases.