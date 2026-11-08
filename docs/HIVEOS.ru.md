# N7Miner — настройка HiveOS

**[Русский](HIVEOS.ru.md) · [English](HIVEOS.md)**

Используйте пакет **Custom Miner** из официального раздела [GitHub Releases](https://github.com/Nigh7man/N7Miner-Releases/releases).

## AutoOC

Для автоматического подбора частоты ядра NVIDIA задайте в HiveOS **Lock Core Clock** и начальный **Core Clock** (смещение частоты). Для PearlHash (PRL) на поддерживаемых видеокартах рекомендуем **Lock Memory Clock = 810 МГц** для лучшей энергоэффективности. В **Extra config arguments** добавьте `--autooc`, примените Flight Sheet и перезапустите майнер. AutoOC регулирует только смещение частоты ядра.

[Подробная инструкция AutoOC на русском](AUTOOC.md) · [AutoOC guide in English](AUTOOC.en.md)

## GeForce RTX — экспериментальный лок памяти 405 МГц

Для совместимых **GeForce RTX 20/30/40/50** режим 405 МГц проверен с драйвером NVIDIA **`610.43.03`**. В HiveOS его можно установить или обновить командой:

```bash
nvidia-driver-update 610.43.03
```

После обновления драйвера добавьте в **Flight Sheet → Extra config arguments**:

```text
--mclock 405
```

Режим требует **root** и доступен на Linux и HiveOS. Если используете `--autooc`, предварительно задайте **Lock Core Clock** (фиксированную частоту ядра). Совместимость зависит от видеокарты и драйвера.

## CMP — экспериментальная разблокировка вычислений

В обычном пакете HiveOS доступен экспериментальный CMP compute unlock для совместимых **CMP 40HX, 50HX, 70HX, 90HX** и некоторых **170HX**. Для **CMP 30HX** анлок не предусмотрен.

В **Flight Sheet → Extra config arguments** используйте `--cmp check` для проверки совместимости. Для включения — `--cmp unlock`: майнер загрузит подходящий [CMP Driver Pack v5](https://github.com/Nigh7man/N7Miner-Releases/releases/tag/cmp-driver-pack-v5), соберёт модуль и попытается перезагрузить риг. Если автоматическая перезагрузка не удалась, проверьте **Event log**. Для возврата штатного модуля используйте `--cmp restore`.

**Анлок выключен по умолчанию.** Перед включением проверьте совместимость командой `--cmp check`.

[Инструкция CMP на русском](CMP-HIVEOS.md) · [English CMP guide](CMP-HIVEOS.en.md)

## Настройка Flight Sheet

1. Добавьте **PRL-кошелёк** в HiveOS.
2. Скопируйте JSON для своего пула ниже.
3. Откройте **Flight Sheets → Import from Clipboard**, вставьте JSON и импортируйте.
4. Выберите или подтвердите PRL-кошелёк и примените Flight Sheet.

Майнер скачивается напрямую из официального релиза N7Miner на GitHub.

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
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.134/n7miner-0.1.134_universal_hive_x64_405fix.tar.gz",
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
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.134/n7miner-0.1.134_universal_hive_x64_405fix.tar.gz",
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
        "install_url": "https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.134/n7miner-0.1.134_universal_hive_x64_405fix.tar.gz",
        "user_config": ""
      },
      "pool_geo": []
    }
  ]
}
```

---

[← К N7Miner](../README.md)
