# Tested GPUs & PearlHash Performance

N7Miner supports NVIDIA and AMD GPUs, including mixed-vendor and multi-GPU systems.

## Best Observed Hashrates

These are **best observed miner-reported per-GPU hashrates** among running test machines. Values were sampled on **24 October 2026 (UTC)** from the miners' live status data. A model with multiple GPUs is listed using the highest individual reading rather than combining cards.

| GPU | Best observed TH/s | Approx. GPU power | Test platform |
| --- | ---: | ---: | --- |
| NVIDIA GeForce RTX 5080 | ~251.6 | ~286 W | Windows |
| NVIDIA GeForce RTX 3080 Ti | ~151.3 | ~274 W | HiveOS |
| NVIDIA GeForce RTX 3080 | ~129.1 | ~239 W | HiveOS |
| NVIDIA CMP 90HX | ~95.2 | ~249 W | Linux |
| NVIDIA GeForce RTX 3070 | ~83.1 | ~151 W | HiveOS |
| NVIDIA GeForce RTX 3070 Ti Laptop GPU | ~79.0 | ~85 W | HiveOS |
| NVIDIA GeForce RTX 3070 Laptop GPU | ~70.7 | ~115 W | HiveOS |
| AMD Radeon RX 6600 XT | ~10.9 | ~61 W | HiveOS |
| AMD Radeon RX 6600M | ~7.9 | ~52 W | HiveOS |
| AMD Radeon RX 5700 XT | ~3.9 | ~129 W | HiveOS |

**Measurement notes:**

- These are readings from **development/test v0.1.127-prototype builds**, with GPU power telemetry recorded alongside the readings. They are **not** benchmarks of the public **v0.1.128** package.
- The readings are local miner-reported GPU hashrates (`gpus[].ths`), **not pool-side effective hashrates**, and should not be presented as 15-minute pool averages.
- The Linux CMP 90HX and Windows RTX 5080 / RTX 3080 and some HiveOS results were observed during long-running sessions. Other HiveOS readings came from recently started miners, so those results are provisional pending longer tests.
- The development/user test builds have different distribution and fee settings from the **public 2% developer-fee build**. Public release performance may differ.
- The GPU power figures are telemetry readings rather than wall-socket measurements. AMD power reporting may be driver-dependent.
- Different cards were measured with their current clocks, power settings, driver versions and cooling. These are not matched-condition comparisons.

### AMD Radeon RX 5700 XT — legacy observation

A separate Windows machine running the older **N7Miner v0.1.113** reported approximately **4.8 TH/s** on an RX 5700 XT. However, the session counters showed **23 accepted and 10 rejected shares**. Because this does not establish reliable pool-side performance, the main compatibility table uses the newer v0.1.127-prototype reading of approximately **3.9 TH/s** instead. The 4.8 TH/s value is documented here for reference, not advertised as a validated result.

## Compatibility Notes

Compatibility and performance depend on supported GPU architectures, installed drivers and runtimes, hardware and clock configuration.

Not every GPU detected on the N7 fleet was used to mine PearlHash during this survey: models without an active N7Miner test are omitted.

## Reliable Comparisons

For a production performance comparison, run each configuration for at least 10–15 minutes, preferably longer, and check submitted/accepted/rejected shares, a stable miner-reported rate, pool-side effective hashrate and measured electrical power. Do not equate one miner-reported high reading with guaranteed pool income.

---

[← Back to N7Miner](../README.md)
