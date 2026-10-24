# Supported GPUs & Historical Performance

N7Miner includes NVIDIA and AMD GPU support, including multi-GPU and mixed-vendor mining rigs.

## Hardware tested on earlier builds

The values below are **historical observations copied from the previous README**. They must **not** be interpreted as official N7Miner v0.1.128 performance results. They are not a controlled head-to-head comparison.

### Previously reported effective hashrates

N7Miner supports multiple NVIDIA and AMD GPU architectures.

Live-tested hardware and observed effective hashrate on N7Miner test rigs:

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

> Results are observed values from tested hardware and depend on clocks, power limits, cooling, drivers and silicon quality.

Additional NVIDIA architectures, CMP cards and datacenter GPUs are supported through architecture-specific paths.

Compatibility and performance depend on the installed driver, runtime, hardware and clock configuration.

## Notes on benchmarking

Compare miners using sustained pool-side effective hashrate and measured power at matched clock settings, thermal conditions and test durations. Short kernel hashrate snapshots are not sufficient to establish real-world performance differences.

---

[← Back to N7Miner](../README.md)
