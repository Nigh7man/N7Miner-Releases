# N7Miner 0.1.140 — Command Line Reference

The v0.1.140 Windows production binary passed --version, --help and CPU-only --self-check --ps 1. Other options retain the published CLI reference; Linux CLI checks have not yet been repeated for this release.

Windows: `n7miner.exe --help` / `n7miner.exe --version`.
Linux: `./n7miner --help` / `./n7miner --version`.

## Mining, pools, GPU selection

| Option | Purpose |
| --- | --- |
| `-a pearlhash` / `--algo pearlhash` | Select PearlHash (PRL). Quantus needs a compatible user build. |
| `-o HOST:PORT` / `--pool HOST:PORT` | Primary pool; comma-separated endpoints for ordered failover. |
| `-u WALLET` / `--wallet WALLET` | Pool account/wallet. |
| `-w rig01` / `--worker rig01` | Worker identifier. |
| `-p x` / `--pass x` | Pool password (defaults to `x` if not otherwise set). |
| `-d 0,1` / `--devices 0,1` | NVIDIA device selection; accepts a list, `all`, `auto`. |
| `--amd-devices all` | AMD device selection: indices, `all`, `auto`, or `off`. |
| `--amd-counter N` | Initial AMD source counter (advanced). |
| `--profile NAME` | Select an available NVIDIA kernel profile (advanced). |

Basic Windows launch:

```bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 -p x -d auto
```

Replace `YOUR_WALLET` with your own wallet address. To use both vendors, add `--devices all --amd-devices all`; to disable AMD explicitly use `--amd-devices off`.

## Secure Stratum connections

Use a pool endpoint with its actual **TLS port**, for example:

```text
--pool stratum+ssl://POOL_HOST:TLS_PORT
```

`stratum+tls://`, `ssl://` and `tls://` schemes are also supported. Certificate validation is enabled by default. `--no-strict-ssl` accepts an untrusted or self-signed pool certificate: traffic stays encrypted, but the pool identity is **not authenticated**. Plain `host:port` connections do not use TLS.

## NVIDIA clocks and power control

| Option | Purpose |
| --- | --- |
| `--pl 250` / `--power-limit 250` | Request a 250 W GPU Power Limit. |
| `--pl 80%` | Use 80% of the card's default power limit. |
| `--power-save 1` / `--ps 1` | Power Save profile, levels `0` through `3`. |
| `--cclock 1800` | Lock core clock to 1800 MHz on a compatible card. |
| `--mclock 405` / `--mclock 810` | For PearlHash, prefer 405 MHz for better power efficiency where supported; use 810 MHz as a fallback. Availability depends on the GPU/driver. |
| `--coffset 150` | Apply a core offset (MHz). |
| `--moffset -200` | Apply a memory offset (MHz). |
| `--restore-cclock BASELINE` | Previously configured core lock to restore on exit. |
| `--restore-mclock BASELINE` | Previously configured memory lock to restore on exit. |
| `--reset-oc` | Restore clock offsets / supplied lock baselines on normal exit. |
| `--no-reset-oc` | Keep miner-applied clock settings after exit. |
| `--cmp-mclock 405` | Experimental memory-control mode for compatible CMP GPUs (405 or 810 MHz). |

Examples for selected NVIDIA devices: `--devices 0,1 --pl 250,230` or `--devices 0,1 --coffset 100,150`. For Power Limit, `-` leaves an individual GPU unchanged.

### Power Save

`--ps 0` leaves Power Save off; `--ps 1`, `--ps 2` and `--ps 3` select its profiles. One value applies to all selected NVIDIA GPUs, or supply a list in `--devices` order:

```text
--ps 1
--devices 0,1,2 --ps 1,2,0
```

**Power Save (v0.1.138):** available on compatible NVIDIA GPUs under Windows, Linux and HiveOS. Linux/HiveOS requires `root` for driver controls. Verify the installed binary and driver support the mode; `--help` alone is not proof of compatibility.

**Power Save tested with NVIDIA drivers:** HiveOS `610.43.03`, `615.71.10`; Windows `596.49`, `621.24`.

[Power Save — Русский](POWER-SAVE.md) · [Power Save — English](POWER-SAVE.en.md)

### AutoOC

AutoOC adjusts NVIDIA core offsets while a locked core clock is configured:

| Option | Purpose |
| --- | --- |
| `--autooc` | Enable tuning. |
| `--autooc-max 300` | Upper offset bound; per-GPU lists accepted. |
| `--autooc-warmup 1` | Startup warm-up minutes, 0–60, default 1. |
| `--autooc-reset` | One-shot reset for all GPUs. |
| `--autooc-reset 0,2` | One-shot reset for selected GPUs. |

```bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --cclock 1800 --coffset 150 --autooc
```

The current TUI/console table uses **`A`** (AutoOC icon), **`OFFSET`** (MHz), **`PS`** (Power Save status), not the old `AUTO` or `CORE OC` columns.

[AutoOC guide — Русский](AUTOOC.md) · [AutoOC guide — English](AUTOOC.en.md)

## Console output, monitoring and diagnostics

| Option | Purpose |
| --- | --- |
| `--notui` | Scrolling console mode; also accepts legacy `-notui`. |
| `--log-interval 180` | Console stats interval in seconds, range 1–3600, default 180. |
| `--no-color` | Plain output without terminal colors. |
| `--headless` | Run without interactive TUI. |
| `--api-enable` | Enable read-only status API on localhost, port 21550. |
| `--api-port 21550` | Choose API port (also enables the API). |
| `--api-bind 127.0.0.1` | API listen address (localhost by default). |
| `--self-check` | Check integrity without starting GPU workers. |
| `--self-check-network` | Check production network services without GPU workers. |
| `--no-submit` | Search without submitting shares. |
| `--help` / `-h` | Show supported arguments. |
| `--version` / `-V` | Show miner version. |

Console example:

```bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --notui --log-interval 180
```

With API enabled, the local endpoint is `http://127.0.0.1:21550/api/v1/status`. Non-loopback API binds expose miner information without authentication.

## HiveOS CMP wrapper (separate from native miner options)

The HiveOS **Custom Miner wrapper** recognizes `--cmp check`, `--cmp unlock` and `--cmp restore`. These are **not** native flags shown by the miner's `--help`. They check GPU compatibility, install a matching driver-pack module, or restore the stock module. Ubuntu 24.04/26.04 also has the separate `n7miner-cmp.py` launcher.

[CMP guide — Русский](CMP-HIVEOS.md) · [CMP guide — English](CMP-HIVEOS.en.md)

## Separate status relay

The optional external status relay uses `--token-file` or environment variable `N7_PRL_MONITOR_TOKEN` instead of the deprecated `--token`. These are not miner CLI flags.

---

[English README](../README.en.md) · [Русский README](../README.md)
