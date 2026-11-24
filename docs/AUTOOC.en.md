# AutoOC — automatic core clock offset tuning

**[Русский](AUTOOC.md) · [English](AUTOOC.en.md)**

AutoOC finds a stable **Core Clock Offset** for each NVIDIA GPU while it mines. It gradually raises the offset, checks stability and saves the result. With a locked core clock, a higher offset can reduce voltage and power use while keeping the target core frequency.

**AutoOC does not tune AMD GPUs or memory clocks.** Each supported NVIDIA card is tuned independently.

**For PearlHash (PRL), a 405 MHz memory lock is preferred for power efficiency on compatible GPUs and drivers; use 810 MHz if 405 MHz is unavailable.** Use `--mclock 405` for 405 MHz (under **Extra config arguments** on HiveOS). For 810 MHz, use `--mclock 810` or **Lock Memory Clock** in HiveOS. This is a separate setting: AutoOC only tunes the core clock offset.

## Requirements

- An N7Miner build with the `--autooc` option.
- A **locked core clock**: set `Lock Core Clock` in HiveOS or pass `--cclock` on Windows. Cards without a core clock lock are skipped.
- A **starting core offset**: HiveOS `Core Clock` or `--coffset`. Without `--coffset`, N7Miner uses the GPU's current offset.

With `--autooc` enabled, **AutoOC** appears after `PearlHash` in the TUI header (the NVIDIA driver version may appear between them). The **`A`** column shows AutoOC state, **`OFFSET`** shows the current core offset in MHz, and **`PS`** is a separate Power Save indicator. [Power Save guide](POWER-SAVE.en.md).

## How to enable it

### HiveOS

1. In the NVIDIA GPU's OC settings, set **Lock Core Clock**, for example `1800` MHz.
2. Set **Core Clock** (starting offset), for example `150` MHz. For PRL, prefer `--mclock 405` under **Extra config arguments** on supported GPUs and drivers; otherwise use **Lock Memory Clock = 810 MHz**.
3. Add this flag to N7Miner's **Extra config arguments** in the Flight Sheet:

~~~text
--autooc
~~~

4. Apply the Flight Sheet and restart the miner. Confirm that the TUI shows `AutoOC` and `A` / `OFFSET` columns.

### Windows (BAT)

Append AutoOC and the clock parameters to your launch command (replace the wallet):

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --cclock 1800 --coffset 150 --mclock 405 --autooc
~~~

If your GPU or driver does not support `--mclock 405`, replace it with `--mclock 810` in the example.

`--cclock 1800` locks the core clock, and `--coffset 150` sets the starting offset. For multiple GPUs, values can be given as comma-separated lists.

## How tuning works

| Stage | What happens |
| --- | --- |
| **Warm-up: 1 minute** | At startup, the GPU temporarily runs **20 MHz below** its selected offset to reduce the risk of cold-start boost instability. Adjust with `--autooc-warmup`. |
| **Search** | After every **20 minutes of stable mining**, AutoOC increases the offset by **10 MHz**. |
| **Testing the new step** | The higher offset is verified over 20 minutes of stable mining. A first confirmed fault retries the same setting; a second fault at the same offset within three hours lowers it by 10 MHz and excludes the failed value as the ceiling. |
| **Tuned** | Once the upper limit or a stability ceiling is reached, the current stable offset is saved and used through restarts. |
| **Repeated GPU faults** | Two confirmed hardware faults at the same applied offset within three hours lower it by 10 MHz. This applies during warm-up, search, and after tuning. |
| **Wrong calculation results** | Three CPU-verified wrong results within one hour on a GPU count as one worker fault toward the ordinary retry policy; they do not automatically lower the offset on their own. |
| **Hard GPU hang** | When the watchdog identifies a hard hang on a specific GPU at a verified offset, AutoOC excludes that offset and higher values on the first incident. Recovery uses a safer saved offset. |

**Fixes in v0.1.141:** AutoOC enforces the configured --autooc-max on startup and recovery, preserves offset rollbacks after driver resets, and resumes a selective multi-GPU --autooc-reset after restarts. Once the lower limit is reached, no further reduction is possible.

**Search limits:** normally the lower bound is starting offset −100 MHz, and the upper bound is starting offset +150 MHz or custom `--autooc-max`. If a saved cap is below the default floor, the allowed range may extend lower for safe recovery. A full search may take several hours.

Only time actually spent mining counts. Pool disconnects and pauses do not count toward the 20-minute intervals.

### Example TUI output

~~~text
n7miner | PearlHash | AutoOC

GPU                        A  OFFSET  PS
RTX 3080                  ▲    +210    —
RTX 3080                  ✓    +180    —
RTX 3080                  ~    +190    —
RTX 3080                  ▼    +160    —
~~~

### After tuning is complete

Once each GPU has found its stable setting, the `A` column shows `✓` for every card. For example:

~~~text
GPU                        A  OFFSET  PS
RTX 3080                  ✓    +230    —
RTX 3080                  ✓    +180    —
RTX 3080                  ✓    +210    —
RTX 3080                  ✓    +160    —
~~~

After a miner restart or machine reboot, AutoOC uses the saved offsets: the GPUs warm up first, then return to their stored settings.

### The A column

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
| `--autooc-warmup 1` | Startup warm-up in minutes (0–60). Default: 1; use `0` to disable warm-up. |
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

AutoOC handles GPU kernel faults, including CUDA 700 and 714–719 (often reported as Xid 13 or Xid 31), GPU hangs and verified wrong calculation results. In v0.1.139 the miner can recover the affected GPU worker separately. **Network failures, pool errors and rejected shares do not count as AutoOC faults.**

Example log lines (illustrative):

~~~text
GPU0 autooc core offset +200 -> +210 MHz (20 min stable)
GPU3 autooc kernel fault 700 at core +170 MHz (1/2 in 3h)
GPU3 autooc core offset +170 -> +160 MHz (2 faults in 3h; ceiling +170)
GPU1 autooc counted a GPU hang (no completed work) at core +200 MHz
GPU0 autooc disabled: needs a locked core clock (--cclock or Hive OC core lock)
autooc reset: every card starts a new search
~~~

**Things to know:**

- In v0.1.139, GPU workers are isolated: a failure can trigger a local restart of the affected GPU process without normally restarting other cards.
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
