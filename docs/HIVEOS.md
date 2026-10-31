# N7Miner — HiveOS Setup

Use the Custom Miner package from the official [GitHub Releases](https://github.com/Nigh7man/N7Miner-Releases/releases).

## AutoOC in v0.1.130

To use NVIDIA AutoOC, set the GPU's **Lock Core Clock** and starting **Core Clock** offset in HiveOS. **For PearlHash (PRL), we recommend Lock Memory Clock = 810 MHz for better power efficiency** on supported GPUs. Add `--autooc` to the miner's **Extra config arguments**, apply the Flight Sheet and restart the miner. AutoOC only tunes the core offset.

**[AutoOC guide (English)](AUTOOC.en.md)** · **[Инструкция AutoOC (русский)](AUTOOC.md)**

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
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.130/n7miner-0.1.130_universal_hive_x64.tar.gz",
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
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.130/n7miner-0.1.130_universal_hive_x64.tar.gz",
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
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.130/n7miner-0.1.130_universal_hive_x64.tar.gz",
        "user_config": ""
      },
      "pool_geo": []
    }
  ]
}
```

---

[← Back to N7Miner](../README.md)
