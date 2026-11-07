# CMP compute unlock — HiveOS

**[Русский](CMP-HIVEOS.md) · [English](CMP-HIVEOS.en.md)**

In **N7Miner 0.1.133**, experimental CMP compute unlock is available in the standard HiveOS package. N7Miner checks compatibility, downloads the pack for the exact NVIDIA Open driver version and builds a module for the running kernel.

**Installing or restoring the driver module may require a rig reboot.**

## Supported GPUs and requirements

- `--cmp check` supports compatibility checks for **CMP 40HX, 50HX, 70HX, 90HX**, and certain **CMP 170HX** variants.
- **CMP 50HX and 90HX** also require a compatible **PCI subsystem**.
- For **CMP 170HX**, only NVIDIA Open `610.43.02`, `610.57.04` and `615.71.09` are supported.
- **CMP 30HX** has **no compute unlock**.

For **Ubuntu 24.04 and 26.04**, a separate launch path uses `n7miner-cmp.py`.

## How to use it on HiveOS

Add commands under **Flight Sheet → Extra config arguments** in the standard N7Miner package:

1. **Check without installing:** `--cmp check`.
2. **Enable unlock:** replace that option with `--cmp unlock`. N7Miner downloads the package matching the exact NVIDIA Open version, builds the module for the running kernel and attempts to **reboot the rig**. If automatic reboot fails, instructions appear in the **Event log**.
3. After reboot, verify GPU visibility using `nvidia-smi -L`.
4. **Restore the stock module:** `--cmp restore`; a reboot may be needed.

---

[← N7Miner](../README.en.md) · [Русский](CMP-HIVEOS.md)
