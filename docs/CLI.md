# N7Miner — Command Line Reference

N7Miner accepts command-line configuration for GPU selection, pool connections, logging and GPU clock controls.

Confirm options against your installed version with `n7miner --help`.

## Command Line Options

Common options:

| Option | Short | Description |
| --- | --- | --- |
| `--pool` | `-o` | Pool endpoint or comma-separated failover list |
| `--wallet` | `-u` | Pool wallet/account |
| `--worker` | `-w` | Worker name |
| `--pass` | `-p` | Pool password |
| `--devices` | `-d` | NVIDIA devices: index list, `auto` or `all` |
| `--amd-devices` |  | AMD devices: index list, `auto` or `all` |
| `--algo` | `-a` | Mining algorithm |
| `--notui` |  | Scrolling console mode |
| `--log-interval` |  | Console statistics interval |
| `--no-color` |  | Disable console colors |

GPU clock control:

| Option | Description |
| --- | --- |
| `--cclock` | Lock core clock |
| `--mclock` | Lock memory clock |
| `--coffset` | Core clock offset |
| `--moffset` | Memory clock offset |
| `--restore-cclock` | Core clock baseline to restore on exit |
| `--restore-mclock` | Memory clock baseline to restore on exit |
| `--reset-oc` | Restore clock settings when the miner exits |
| `--no-reset-oc` | Keep miner-applied clocks after exit |

Diagnostic / maintenance options:

| Option | Description |
| --- | --- |
| `--self-check` | Verify production startup integrity without starting GPU workers |
| `--self-check-network` | Check production service connectivity without mining |
| `--no-submit` | Search normally but do not submit shares |
| `--help` | Show command-line help |
| `--version` | Show N7Miner version |

## AutoOC (NVIDIA)

AutoOC automatically tunes a stable NVIDIA GPU core offset while mining. A locked core clock is required; AMD GPUs and memory offsets are not tuned.

| Option | What it does |
| --- | --- |
| `--autooc` | Enable automatic GPU core offset tuning. |
| `--autooc-max 300` | Set the maximum offset in MHz (default: initial offset +150 MHz). |
| `--autooc-warmup 3` | Set startup warm-up duration in minutes (default 3; range 0–60). |
| `--autooc-reset` | Reset saved tuning once for all GPUs. |
| `--autooc-reset 0,2` | Reset only the selected GPUs. |

Use `--cclock` to lock the core clock and `--coffset` to set the starting offset. **For PearlHash (PRL), locking memory to 810 MHz with `--mclock 810` is recommended for better power efficiency** on supported GPUs. AutoOC itself only tunes the core offset:

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --cclock 1800 --coffset 150 --mclock 810 --autooc
~~~

**[Full AutoOC guide (English)](AUTOOC.en.md)** · **[Инструкция AutoOC на русском](AUTOOC.md)**

## CMP compute unlock (HiveOS, experimental in v0.1.133)

For compatible CMP 40HX, 50HX, 70HX, 90HX and certain 170HX variants (not CMP 30HX). The installed NVIDIA Open version must match the driver pack and headers for the running kernel must be available.

| Option | Description |
| --- | --- |
| `--cmp check` | Check GPU, exact driver version and kernel compatibility without installing modules. |
| `--cmp unlock` | Download a matching driver pack, build the module and attempt a reboot. Check the Event log if automatic reboot fails. |
| `--cmp restore` | Restore the stock module; reboot if needed. |

Unlock is off by default. On Ubuntu 24.04/26.04, use the separate `n7miner-cmp.py` launcher.

[Full CMP guide (English)](CMP-HIVEOS.en.md) · [Инструкция CMP на русском](CMP-HIVEOS.md)

## GeForce RTX 405 MHz memory clock (experimental, Linux/HiveOS)

For compatible **GeForce RTX 20, 30, 40 and 50 series** cards with NVIDIA driver **`595.91.07`** or **`610.43.03`**, N7Miner 0.1.133 offers an experimental low-memory-clock mode. Run as **root** and add `--mclock 405` to the miner arguments. When using `--autooc`, configure a **locked core clock** first. If the GPU or driver is unsupported, do not use this mode.

## Separate status relay

If you use a standalone status relay, replace `--token` with `--token-file` or set `N7_PRL_MONITOR_TOKEN` in its environment. This change applies to the separate relay, not ordinary mining.

## Examples

### Single pool (Windows)

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 -p x -d auto
~~~

### Mixed NVIDIA and AMD rig (Windows)

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --devices all --amd-devices all
~~~

### Pool failover

~~~bash
--pool primary.pool:7048,backup1.pool:7048,backup2.pool:7048
~~~

### Console mode

~~~bash
--notui --log-interval 180
~~~

---

[← Back to N7Miner](../README.md)
