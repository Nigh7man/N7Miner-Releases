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
