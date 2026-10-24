# N7Miner

**PearlHash GPU miner for NVIDIA and AMD**

N7Miner is a purpose-built PearlHash miner focused on sustained performance, power efficiency and reliable pool mining. It supports individual GPUs, multi-GPU systems and mixed NVIDIA + AMD mining rigs.

**Windows x64 · Linux x64 · HiveOS**  
**NVIDIA + AMD · Multi-GPU · 2% developer fee**

[**Download latest public release**](https://github.com/Nigh7man/N7Miner-Releases/releases/latest) · [All releases](https://github.com/Nigh7man/N7Miner-Releases/releases) · [Report an issue](https://github.com/Nigh7man/N7Miner-Releases/issues)

## Download v0.1.128

| Platform | Package |
| --- | --- |
| Windows x64 | [N7Miner-v0.1.128-universal-windows-x64.zip](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.128/N7Miner-v0.1.128-universal-windows-x64.zip) |
| Linux x64 | [N7Miner-v0.1.128-universal-linux-x64.tar.gz](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.128/N7Miner-v0.1.128-universal-linux-x64.tar.gz) |
| HiveOS | [n7miner-0.1.128_universal_hive_x64.tar.gz](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.128/n7miner-0.1.128_universal_hive_x64.tar.gz) |

**Always download binaries from the official N7Miner Releases repository.** SHA-256 checksums are provided with each release for verifying downloads.

## Why N7Miner?

### Performance & efficiency

- PearlHash-specific mining kernels with GPU architecture-aware execution paths.
- Support for NVIDIA and AMD GPUs in the same rig.
- Per-GPU monitoring and flexible hardware selection.
- Performance optimization focused on sustained mining workloads.

### Real-world monitoring

- Interactive terminal interface (TUI).
- Lightweight scrolling console mode for remote/headless systems.
- Per-GPU hashrate and hardware telemetry.
- Effective pool hashrate and session-effort statistics.
- Accepted, rejected and stale share counts.

### Reliability & control

- Priority-ordered pool failover.
- GPU core and memory clock configuration, including per-GPU values.
- Optional restoration of miner-applied GPU clocks on exit.
- Built-in integrity and connectivity self-check options.
- Universal packages for Windows, Linux and HiveOS.

## Interface Preview

![N7Miner interactive TUI](docs/images/n7miner-tui.png)

**Note:** Pool-side effective hashrate (`Eff HR`) needs approximately **10–15 minutes** of continuous mining to become representative.

### Console mode

![N7Miner console](docs/images/n7miner-console.png)

To run without the interactive dashboard:

~~~bash
n7miner --notui --log-interval 180
~~~

## Quick Start

### Windows

Download the Windows ZIP archive, extract it, and launch a bundled pool starter after replacing the wallet placeholder, or use the CLI:

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 -p x -d auto
~~~

Replace `YOUR_WALLET` with your Pearl wallet address. Adjust `rig01` to your preferred worker name.

### Mixed NVIDIA + AMD rig

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --devices all --amd-devices all
~~~

### HiveOS

Use the N7Miner Custom Miner package. Create a PRL wallet, configure the pool and miner in a Flight Sheet, then apply it to your rig.

**[HiveOS setup and importable Flight Sheets →](docs/HIVEOS.md)**

### Advanced configuration

**[Command-line options, clock control, diagnostics and failover →](docs/CLI.md)**

## Tested GPUs & Hashrates

N7Miner has been tested on NVIDIA GeForce, NVIDIA CMP and AMD Radeon GPUs. The table shows **observed pool-side effective hashrate (Eff HR)** on N7Miner test rigs.

| GPU | Observed Eff HR |
| --- | ---: |
| NVIDIA GeForce RTX 5080 | ~239 TH/s |
| NVIDIA GeForce RTX 3080 Ti | ~137 TH/s |
| NVIDIA GeForce RTX 3080 | ~114–116 TH/s |
| NVIDIA CMP 90HX | ~77.7 TH/s |
| NVIDIA GeForce RTX 3070 Ti Laptop GPU | ~73.7 TH/s |
| NVIDIA GeForce RTX 3070 | ~73.3 TH/s |
| NVIDIA GeForce RTX 3070 Laptop GPU | ~63–67 TH/s |
| AMD Radeon RX 6600 XT | ~10.8 TH/s |
| AMD Radeon RX 6600M | ~7.8 TH/s |
| AMD Radeon RX 5700 XT | ~3.9 TH/s |

> **Historical test results:** The numbers above were reported on earlier N7Miner builds and are **not verified benchmarks of v0.1.128**. Effective hashrate varies with drivers, clocks, power limits, cooling, silicon quality and pool conditions. They are not a controlled miner-to-miner comparison.

Additional compatible GPUs may operate through supported architecture-specific paths.

[Full GPU compatibility and benchmarking notes](docs/SUPPORTED-GPUS.md)

## Developer Fee

The public production build includes a **2% developer fee** to support ongoing development, kernel optimization, compatibility testing and maintenance.

There is no subscription fee.

## Verification & Diagnostics

- `--self-check` — verify production startup integrity without starting GPU workers.
- `--self-check-network` — check production service connectivity without mining.
- `--no-submit` — run a non-submitting test.
- `--help` / `--version` — list available options and print the installed version.

Verify release downloads against the included `SHA256SUMS.txt` before use.

### Verify on Windows

~~~powershell
Get-FileHash .\N7Miner-v0.1.128-universal-windows-x64.zip -Algorithm SHA256
~~~

### Verify on Linux

~~~bash
sha256sum N7Miner-v0.1.128-universal-linux-x64.tar.gz
~~~

## Feedback & Issues

Please [open an issue](https://github.com/Nigh7man/N7Miner-Releases/issues) with the miner version, operating system, GPU model(s), driver version, pool, relevant command-line options and a reproducible log excerpt.

**Never include private keys, passwords or other credentials in issue reports.**

## Distribution

N7Miner is proprietary software distributed as prebuilt binaries. Its source code is maintained privately.

---

**N7Miner — Built for PearlHash. Focused on performance.**
