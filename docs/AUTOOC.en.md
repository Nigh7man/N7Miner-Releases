# AutoOC — automatic core clock offset tuning

**[Русский](AUTOOC.md) · [English](AUTOOC.en.md)**

AutoOC finds a stable **Core Clock Offset** for each NVIDIA GPU while it mines. It gradually raises the offset, checks stability and saves the result. With a locked core clock, a higher offset can reduce voltage and power use while keeping the target core frequency.

**AutoOC does not tune AMD GPUs or memory clocks.** Each supported NVIDIA card is tuned independently.

**For PearlHash (PRL), we recommend locking GPU memory to 810 MHz** for better power efficiency, based on tested configurations. Use **Lock Memory Clock** in HiveOS or `--mclock 810` on Windows. This is a separate setting: AutoOC only tunes the core clock offset.

## Requirements

- An N7Miner build with the `--autooc` option.
- A **locked core clock**: set `Lock Core Clock` in HiveOS or pass `--cclock` on Windows. Cards without a core clock lock are skipped.
- A **starting core offset**: HiveOS `Core Clock` or `--coffset`. Without `--coffset`, N7Miner uses the GPU's current offset.

When active, `AutoOC` appears after `PearlHash` in the TUI header. The GPU table shows `AUTO` and `CORE OC` columns.

## How to enable it

### HiveOS

1. In the NVIDIA GPU's OC settings, set **Lock Core Clock**, for example `1800` MHz.
2. Set **Core Clock** (starting offset), for example `150` MHz. For PRL, we also recommend **Lock Memory Clock = 810 MHz** if the GPU supports locking memory to that value.
3. Add this flag to N7Miner's **Extra config arguments** in the Flight Sheet:

~~~text
--autooc
~~~

4. Apply the Flight Sheet and restart the miner. Confirm that the TUI shows `AutoOC` and the `AUTO` / `CORE OC` columns.

### Windows (BAT)

Append AutoOC and the clock parameters to your launch command (replace the wallet):

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --cclock 1800 --coffset 150 --mclock 810 --autooc
~~~

`--cclock 1800` locks the core clock, and `--coffset 150` sets the starting offset. For multiple GPUs, values can be given as comma-separated lists.

## How tuning works

| Stage | What happens |
| --- | --- |
| **Warm-up: 3 minutes** | At startup, the GPU temporarily runs **20 MHz below** its selected offset to reduce the risk of cold-start boost instability. |
| **Search** | After every **20 minutes of stable mining**, AutoOC increases the offset by **10 MHz**. |
| **Testing the new step** | The higher setting is tested for 20 minutes. A single fault during this test immediately steps back by 10 MHz, and the failed value becomes the ceiling. |
| **Tuned** | Once the upper limit or a stability ceiling is reached, the current stable offset is saved and used through restarts. |
| **Faults after tuning** | If a tuned GPU has **3 faults within 24 hours** at the same offset, AutoOC decreases it by 10 MHz and saves a new ceiling. Faults older than 24 hours expire. |

**Search limits:** the lower bound is starting offset −100 MHz; the upper bound is starting offset +150 MHz, or the custom `--autooc-max` setting. A full search may take several hours.

Only time actually spent mining counts. Pool disconnects and pauses do not count toward the 20-minute intervals.

### Example TUI output

~~~text
n7miner | PearlHash | AutoOC

GPU                       AUTO  CORE OC
RTX 3080                    ▲    +210
RTX 3080                    ✓    +180
RTX 3080                    ~    +190
RTX 3080                    ▼    +160
~~~

### After tuning is complete

Once each GPU has found its stable setting, the `AUTO` column shows `✓` for every card. For example:

~~~text
GPU                       AUTO  CORE OC
RTX 3080                    ✓    +230
RTX 3080                    ✓    +180
RTX 3080                    ✓    +210
RTX 3080                    ✓    +160
~~~

After a miner restart or machine reboot, AutoOC uses the saved offsets: the GPUs warm up first, then return to their stored settings.

### The AUTO column

| Symbol | Meaning |
| --- | --- |
| `▲` | Searching or testing a new offset step. |
| `~` | Warm-up after startup; temporarily lower offset. |
| `▼` | Offset was recently lowered due to faults. |
| `✓` | Tuning completed; the saved offset is in use. |
| `!` | The floor was reached but faults continue. Check risers, power and memory OC. |
| *Empty* | AutoOC disabled or the GPU has no locked core clock. |

## Command-line options

| Option | Description |
| --- | --- |
| `--autooc` | Enables AutoOC for eligible NVIDIA GPUs; off by default. |
| `--autooc-max 300` | Maximum core offset in MHz. Accepts per-GPU comma-separated values such as `300,250`. Default: starting offset +150 MHz. |
| `--autooc-warmup 3` | Startup warm-up in minutes (0–60). Default: 3; use `0` to disable warm-up. |
| `--autooc-reset` | Reset saved tuning for all GPUs. |
| `--autooc-reset 0,2` | Reset only GPU 0 and GPU 2. |
| `--cclock 1800` | Lock the GPU core clock; HiveOS can supply the lock through OC settings. |
| `--coffset 150` | Starting core offset. Without this flag, the current offset is used. |

## Saving and resetting tuning

N7Miner stores per-GPU settings by UUID in `n7miner-autooc.state`.

- **HiveOS:** `/hive-config/n7miner-autooc.state` — survives miner restarts, OS reboots and miner updates.
- **Windows:** next to `n7miner.exe`. Copy this file when moving the miner to a different folder.
- Adding a new GPU starts tuning only that card; existing settings remain.
- Changing a card's starting core offset or locked frequency in HiveOS resets tuning for that GPU **after the miner restarts**.

### Starting over

Add:

~~~text
--autooc --autooc-reset
~~~

The reset is **one-shot for that flag value**: repeated automatic miner restarts will not repeatedly wipe the saved result. To reset again, remove the flag, run the miner once, then add the flag back. Alternatively, change the selection (for example, from `all` to `0,2`).

For a full manual reset, stop the miner and delete `n7miner-autooc.state`.

## Faults and logs

AutoOC counts GPU kernel faults, including CUDA 700 and 714–719 (often reported as Xid 13 or Xid 31), and GPU hangs with no completed work that trigger the miner's watchdog restart. **Network failures, pool errors and rejected shares do not count as AutoOC faults.**

Example log lines (illustrative):

~~~text
GPU0 autooc core offset +200 -> +210 MHz (20 min stable)
GPU3 autooc kernel fault 700 at core +170 MHz (2/3)
GPU3 autooc core offset +170 -> +160 MHz (3 kernel faults; ceiling +170)
GPU1 autooc counted a GPU hang (no completed work) at core +200 MHz
GPU0 autooc disabled: needs a locked core clock (--cclock or Hive OC core lock)
autooc reset: every card starts a new search
~~~

**Things to know:**

- While tuning, a fault on one GPU can restart the entire miner. Some restarts in the first hours are possible.
- An unstable memory overclock can look like a core fault. Keep some memory OC headroom while AutoOC is searching.
- A ceiling stays saved; it does not rise automatically. After fixing a bad riser or power issue, restart tuning for the affected card using `--autooc-reset`.
- After changing HiveOS overclock settings, restart the miner.

## Monitoring

In `status.json` and `http://127.0.0.1:21550/api/v1/status`, each GPU has an `autooc` object (or `null` when disabled).

Example:

~~~json
"autooc": {
  "state": "capped",
  "base_mhz": 150,
  "target_mhz": 230,
  "ceiling_mhz": 240,
  "floor_mhz": 50,
  "max_mhz": 300,
  "faults": 0
}
~~~

Possible states: `searching`, `warmup`, `probation`, `lowered`, `capped` (tuned) and `floor` (lower limit).

---

[← N7Miner](../README.en.md) · [Русский](AUTOOC.md)
