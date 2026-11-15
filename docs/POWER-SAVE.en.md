# Power Save — NVIDIA power-efficiency profiles

**[Русский](POWER-SAVE.md) · [English](POWER-SAVE.en.md)**

Power Save selects an additional NVIDIA power-efficiency profile while mining PearlHash. The `--ps` flag is an alias for `--power-save`. This is **separate** from Power Limit (`--pl`), memory clocks (`--mclock`) and AutoOC.

## Availability

- **Windows x64:** N7Miner 0.1.138 with a compatible NVIDIA GPU and driver.
- **Linux x64 / HiveOS:** N7Miner 0.1.138 with a compatible NVIDIA GPU, supported driver and **`root`** privileges for GPU controls.
- **AMD:** Power Save is not supported.

Actual compatibility depends on the GPU and driver. Seeing `--ps` in `--help` alone does not establish that a particular GPU supports the mode.

## Tested drivers

**NVIDIA drivers tested with Power Save:** HiveOS — `610.43.03`, `615.71.10`; Windows — `596.49`, `621.24`. Testing on these versions does not guarantee compatibility with every GPU model.

## Levels

| Level | Meaning |
| --- | --- |
| `0` | Do not enable Power Save for the selected GPU. |
| `1` | First power-efficiency profile; a good starting point for measurements. |
| `2` | Second profile, with a stronger setting on supported GPUs. |
| `3` | Third profile; differs from level `2` on some generations but is equivalent on others. |

**The largest Power Save benefit in our testing has been on RTX 50-series GPUs (such as RTX 5080).** This does not guarantee the same improvement on every card. **Power Save levels are profile numbers, not power-saving percentages.** In the current implementation levels `2` and `3` use the same main control setting for RTX 20/30/40, but differ on RTX 50. Actual hashrate, power draw and efficiency depend on the GPU and driver. Higher numbers are not guaranteed to improve TH/W.

## Windows usage

Add Power Save to your launch command:

```bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --devices 0 --ps 1
```

Replace `YOUR_WALLET` with your wallet address. This selects NVIDIA GPU 0 and level 1.

To apply one level to all selected NVIDIA cards:

```text
--ps 1
```

To set different levels in `--devices` order:

```text
--devices 0,1,2 --ps 1,2,0
```

GPU 0 uses level 1, GPU 1 uses level 2, and GPU 2 has Power Save off. When supplying a list, its length must match the selected NVIDIA GPU count. The full option name is `--power-save 1`. Level `4` or higher is invalid.

## HiveOS / Linux

Under **Flight Sheet → Extra config arguments** on HiveOS, add:

```text
--ps 1
```

This requests level 1 on all selected NVIDIA GPUs. For per-GPU profiles use a list, for example `--devices 0,1,2 --ps 1,2,0`.

On Linux use the same option with `./n7miner`. Applying and restoring the driver-level Power Save settings requires **`root`** privileges. On unsupported drivers or GPUs the miner may warn that the mode cannot be applied.

## Related GPU controls

| Option | Purpose |
| --- | --- |
| `--ps 1` | Select a Power Save profile. |
| `--pl 250` | Set a 250 W GPU power limit. |
| `--pl 80%` | Request 80% of the GPU's default power limit. |
| `--mclock 405` | Request a 405 MHz memory lock where the GPU and driver permit it. |
| `--autooc` | Tune NVIDIA core offset automatically with a locked core clock. |

The options can be combined in supported configurations, but comparing one change at a time makes it easier to understand the results. Compare hashrate, power draw and accepted shares after the run stabilizes.

## Understanding the PS column

The TUI and scrolling console use **`A`**, **`OFFSET`** and **`PS`** columns:

- **A** — AutoOC tuning state: `▲`, `~`, `▼`, `✓` or `!`.
- **OFFSET** — current NVIDIA core clock offset in MHz.
- **PS** — Power Save status: a green `1`/`2`/`3` means active; a yellow number means pending; red `off` means disabled after a failed attempt; `—` means no Power Save was requested.

The **AutoOC** label appears in the TUI banner only when `--autooc` is enabled. Power Save can run independently.

## Troubleshooting

Check build support, GPU compatibility and driver version first. Some drivers do not support the required controls. After a confirmed rollback, mining can continue without Power Save and `PS` shows `off`.

The miner attempts to restore modified settings on normal exit. After an unexpected exit, verify GPU settings because forced termination may prevent restoration.

[Back to N7Miner](../README.en.md) · [CLI reference](CLI.md) · [AutoOC](AUTOOC.en.md)
