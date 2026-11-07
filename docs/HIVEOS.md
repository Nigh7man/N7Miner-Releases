# N7Miner — HiveOS Setup

**[Русский](HIVEOS.ru.md) · [English](HIVEOS.md)**

Use the Custom Miner package from the official [GitHub Releases](https://github.com/Nigh7man/N7Miner-Releases/releases).

## AutoOC

To use NVIDIA AutoOC, set the GPU's **Lock Core Clock** and starting **Core Clock** offset in HiveOS. **For PearlHash (PRL), we recommend Lock Memory Clock = 810 MHz for better power efficiency** on supported GPUs. Add `--autooc` to the miner's **Extra config arguments**, apply the Flight Sheet and restart the miner. AutoOC only tunes the core offset.

**[AutoOC guide (English)](AUTOOC.en.md)** · **[Инструкция AutoOC (русский)](AUTOOC.md)**

## GeForce RTX 405 MHz memory clock (experimental)

On supported **RTX 20/30/40/50-series** GPUs and NVIDIA drivers **`595.91.07`** or **`610.43.03`**, set `--mclock 405` in **Flight Sheet → Extra config arguments** for experimental low-memory-clock operation. This requires **root**. To use it with `--autooc`, first configure a **locked core clock**. Not all GPU/driver combinations are compatible.

[Command line reference and requirements](CLI.md)

## CMP compute unlock (experimental in v0.1.133)

The standard HiveOS package supports an experimental compute unlock workflow for compatible CMP 40HX, 50HX, 70HX, 90HX and some 170HX models. CMP 30HX is not supported. Compatibility depends on the exact NVIDIA Open driver version and current kernel headers.

Under **Flight Sheet → Extra config arguments**, start with `--cmp check`. Use `--cmp unlock` to download the matching [CMP Driver Pack v5](https://github.com/Nigh7man/N7Miner-Releases/releases/tag/cmp-driver-pack-v5), build the kernel module and attempt a reboot. If the automatic reboot fails, check the **Event log**. Use `--cmp restore` to return to the stock module.

**Unlock is off by default.** Check GPU and driver compatibility using `--cmp check` before enabling it.

[English CMP guide](CMP-HIVEOS.en.md) · [Инструкция CMP на русском](CMP-HIVEOS.md)

## Flight Sheet

1. Add your **PRL wallet** in HiveOS.
2. Copy the JSON for your pool below.
3. Open **Flight Sheets → Import from Clipboard**, paste it and import.
4. Select/confirm your PRL wallet and apply the Flight Sheet.

The miner is downloaded directly from the official N7Miner GitHub Release.

#### Kryptex

```json
{
  "name": "N7Miner PRL - Kryptex",
  "isFavorite": false,
  "items": [
    {
      "coin": "PRL",
      "pool_ssl": false,
      "wal_id": 0,
      "dpool_ssl": false,
      "miner": "custom",
      "miner_alt": "n7miner",
      "miner_config": {
        "url": "prl.kryptex.network:7048",
        "algo": "pearlhash",
        "pass": "x",
        "miner": "n7miner",
        "template": "%WAL%.%WORKER_NAME%",
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.133/n7miner-0.1.133_universal_hive_x64.tar.gz",
        "user_config": ""
      },
      "pool_geo": []
    }
  ]
}
```

#### HeroMiners

```json
{
  "name": "N7Miner PRL - HeroMiners",
  "isFavorite": false,
  "items": [
    {
      "coin": "PRL",
      "pool_ssl": false,
      "wal_id": 0,
      "dpool_ssl": false,
      "miner": "custom",
      "miner_alt": "n7miner",
      "miner_config": {
        "url": "de.pearl.gfwroute.com:1200",
        "algo": "pearlhash",
        "pass": "x",
        "miner": "n7miner",
        "template": "%WAL%.%WORKER_NAME%",
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.133/n7miner-0.1.133_universal_hive_x64.tar.gz",
        "user_config": ""
      },
      "pool_geo": []
    }
  ]
}
```

#### LuckyPool

```json
{
  "name": "N7Miner PRL - LuckyPool",
  "isFavorite": false,
  "items": [
    {
      "coin": "PRL",
      "pool_ssl": false,
      "wal_id": 0,
      "dpool_ssl": false,
      "miner": "custom",
      "miner_alt": "n7miner",
      "miner_config": {
        "url": "pearl-eu2.luckypool.io:3360",
        "algo": "pearlhash",
        "pass": "x",
        "miner": "n7miner",
        "template": "%WAL%.%WORKER_NAME%",
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.133/n7miner-0.1.133_universal_hive_x64.tar.gz",
        "user_config": ""
      },
      "pool_geo": []
    }
  ]
}
```

---

[← Back to N7Miner](../README.md)
