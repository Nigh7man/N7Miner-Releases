# N7Miner

**[Русский](README.md) · [English](README.en.md)**

N7Miner mines PearlHash on NVIDIA and AMD GPUs. It runs on Windows, Linux and HiveOS, whether you have a single card, a multi-GPU rig or a mix of NVIDIA and AMD.

**Windows x64 · Linux x64 · HiveOS · Developer fee in 0.1.129: 1%**


## What's changed in 0.1.129

- **Developer fee reduced from 2% to 1%.**
- Small hashrate improvements.
- Bug fixes.

## Download

| Platform | Package |
| --- | --- |
| Windows x64 | [N7Miner 0.1.129 for Windows](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.129/N7Miner-v0.1.129-universal-windows-x64.zip) |
| Linux x64 | [N7Miner 0.1.129 for Linux](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.129/N7Miner-v0.1.129-universal-linux-x64.tar.gz) |
| HiveOS | [N7Miner 0.1.129 for HiveOS](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.129/n7miner-0.1.129_universal_hive_x64.tar.gz) |

[All releases](https://github.com/Nigh7man/N7Miner-Releases/releases) · [Latest public release](https://github.com/Nigh7man/N7Miner-Releases/releases/latest)

Download only from this repository. Each release includes a `SHA256SUMS.txt` file so you can check the archives.

## Features

- Runs NVIDIA and AMD GPUs together in one rig.
- Shows hashrate, temperature, power draw and share counts for each GPU.
- Includes an interactive TUI and a regular scrolling console mode.
- Can switch to backup pools when the primary pool isn't available.
- Offers GPU core and memory clock settings, including per-GPU settings and optional reset on exit.
- Includes startup and network checks.

## Screenshots

### TUI

![N7Miner 0.1.129 — TUI, RTX 5080](docs/images/n7miner-rtx5080-tui-0.1.129.png)

Give `Eff HR` around 10–15 minutes of mining before relying on the number.

### Console mode

![N7Miner console mode with three RTX 3070 Laptop GPUs](docs/images/n7miner-3x3070l-console.png)

For plain console output:

~~~bash
n7miner --notui --log-interval 180
~~~

## Quick start

### Windows

Extract the ZIP, open the BAT file for your pool and enter your wallet address. Or run the miner yourself:

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 -p x -d auto
~~~

Replace `YOUR_WALLET` with your Pearl wallet address. You can change the worker name `rig01`.

For a mixed NVIDIA + AMD rig:

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --devices all --amd-devices all
~~~

### HiveOS

Use the HiveOS Custom Miner package. Ready-to-import pool settings are in the [HiveOS setup guide](docs/HIVEOS.md).

See the [command-line reference](docs/CLI.md) for the full set of options, clock controls and diagnostic commands.

## Tested GPUs and hashrates

Tested on the following GPUs:

| GPU | Hashrate |
| --- | ---: |
| NVIDIA GeForce RTX 5080 | ~263 TH/s |
| NVIDIA GeForce RTX 3080 Ti | ~152 TH/s |
| NVIDIA GeForce RTX 3080 | ~130 TH/s |
| NVIDIA CMP 90HX | ~99 TH/s |
| NVIDIA GeForce RTX 3070 | ~90 TH/s |
| NVIDIA GeForce RTX 3070 Ti Laptop GPU | ~81 TH/s |
| NVIDIA GeForce RTX 3070 Laptop GPU | ~71 TH/s |
| AMD Radeon RX 6600 XT | ~11 TH/s |
| AMD Radeon RX 6600M | ~8 TH/s |
| AMD Radeon RX 5700 XT | ~5 TH/s |

Results depend on the card, clock settings, power, cooling and drivers.

## Verify your download

Compare the archive's SHA-256 hash with `SHA256SUMS.txt` from the release.

**Windows:**

~~~powershell
Get-FileHash .\N7Miner-v0.1.129-universal-windows-x64.zip -Algorithm SHA256
~~~

**Linux:**

~~~bash
sha256sum N7Miner-v0.1.129-universal-linux-x64.tar.gz
~~~

Having trouble? [Open an issue](https://github.com/Nigh7man/N7Miner-Releases/issues) with the version, GPU, driver, pool and relevant log output. Don't post passwords or private keys.

N7Miner is provided "as is", without any warranties.
