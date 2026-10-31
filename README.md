# N7Miner

**[Русский](README.md) · [English](README.en.md)**

N7Miner — майнер PearlHash для видеокарт NVIDIA и AMD. Работает на Windows, Linux и HiveOS. Можно запустить одну карту, целую ферму или вместе использовать NVIDIA и AMD.

**Windows x64 · Linux x64 · HiveOS · Комиссия разработчика 1%**

## Что изменилось в 0.1.130

- **AutoOC** — автоматический подбор стабильного Core Clock Offset для видеокарт NVIDIA. [Как включить и настроить](docs/AUTOOC.md).
- Мелкие исправления ошибок.

## Скачать

| Система | Архив |
| --- | --- |
| Windows x64 | [N7Miner 0.1.130 для Windows](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.130/N7Miner-v0.1.130-universal-windows-x64.zip) |
| Linux x64 | [N7Miner 0.1.130 для Linux](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.130/N7Miner-v0.1.130-universal-linux-x64.tar.gz) |
| HiveOS | [N7Miner 0.1.130 для HiveOS](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.130/n7miner-0.1.130_universal_hive_x64.tar.gz) |

[Все релизы](https://github.com/Nigh7man/N7Miner-Releases/releases) · [Последний релиз](https://github.com/Nigh7man/N7Miner-Releases/releases/latest)

Проверяйте архивы по файлу [SHA256SUMS.txt](https://github.com/Nigh7man/N7Miner-Releases/releases/download/v0.1.130/SHA256SUMS.txt).

# Что умеет N7Miner

- Работает с NVIDIA и AMD, в том числе в одной ферме.
- Показывает хэшрейт, температуру, потребление и статистику шар по каждой карте.
- Есть удобный терминальный интерфейс (TUI) и обычный консольный режим для логов.
- Поддерживает резервные пулы и переключается на них при проблемах с основным.
- Позволяет задавать частоты ядра и памяти, включая настройки для отдельных GPU, и восстанавливать их при выходе.
- Есть команды для проверки запуска и сетевого подключения.

## AutoOC

Новый режим `--autooc` сам подбирает offset ядра NVIDIA при залоченной частоте. Карты настраиваются по отдельности: майнер повышает offset небольшими шагами, откатывает его при ошибках и сохраняет найденные значения.

Для запуска в HiveOS укажите **Lock Core Clock** и стартовый **Core Clock**, затем добавьте `--autooc` в `Extra config arguments`. В Windows используйте `--cclock`, `--coffset` и `--autooc`.

**Для PRL рекомендуем фиксировать частоту памяти на 810 МГц для лучшей энергоэффективности** (если карта поддерживает такую настройку).

**[Инструкция AutoOC на русском: настройка, этапы подбора, значки, ошибки и сброс](docs/AUTOOC.md)**

## Как выглядит

### Режим TUI

![N7Miner 0.1.129 — TUI, RTX 5080](docs/images/n7miner-rtx5080-tui-0.1.129.png)

*Скриншот предыдущей версии (0.1.129). Обозначения AutoOC приведены в отдельной инструкции.*

Для корректного значения `Eff HR` дайте майнеру поработать хотя бы 10–15 минут.

### Консольный режим

![N7Miner — консольный режим на трёх RTX 3070 Laptop](docs/images/n7miner-3x3070l-console.png)

Если нужен обычный вывод в консоль вместо TUI:

~~~bash
n7miner --notui --log-interval 180
~~~

## Как запустить

### Windows

Распакуйте архив, откройте BAT-файл своего пула и укажите адрес кошелька. Можно запустить и вручную:

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 -p x -d auto
~~~

Вместо `YOUR_WALLET` вставьте адрес кошелька Pearl. `rig01` — имя воркера, его можно поменять.

Если в ферме есть и NVIDIA, и AMD:

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --devices all --amd-devices all
~~~

### HiveOS

В HiveOS майнер добавляется как Custom Miner. Готовые настройки для нескольких пулов есть в [инструкции по HiveOS](docs/HIVEOS.md).

Все параметры запуска, настройки частот и диагностические команды собраны в [справке по командной строке](docs/CLI.md).

## Хэшрейт на проверенных картах

Протестировано на следующих видеокартах:

| Видеокарта | Хэшрейт |
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

Результат зависит от видеокарты, частот, питания, охлаждения и драйверов.

## Проверка скачанных файлов

Сверьте SHA-256 скачанного архива с `SHA256SUMS.txt` из релиза. Команды для проверки (подставьте имя скачанного файла):

**Windows:** `Get-FileHash .\N7Miner-v0.1.130-universal-windows-x64.zip -Algorithm SHA256`

**Linux:** `sha256sum N7Miner-v0.1.130-universal-linux-x64.tar.gz`

Если майнер работает неправильно, [создайте Issue](https://github.com/Nigh7man/N7Miner-Releases/issues). Напишите версию, модель GPU, драйвер, пул и приложите нужный фрагмент лога. Не публикуйте пароли и приватные ключи.

N7Miner распространяется «как есть», без каких-либо гарантий.
