# N7Miner — Command Line Reference

N7Miner accepts command-line configuration for GPU selection, pool connections, logging and GPU clock controls.

Confirm options against your installed version with `n7miner --help`.

## Command Line Options

Common options:

| Option | Short | Description |
| --- | --- | --- |
| `--pool` | `-o` | Pool endpoint or comma-separated failover list |
| `--wallet` | `-u` | Pool wallet/account |
| `--worker` | `-w` | Worker name |
| `--pass` | `-p` | Pool password |
| `--devices` | `-d` | NVIDIA devices: index list, `auto` or `all` |
| `--amd-devices` |  | AMD devices: index list, `auto` or `all` |
| `--algo` | `-a` | Mining algorithm |
| `--notui` |  | Scrolling console mode |
| `--log-interval` |  | Console statistics interval |
| `--no-color` |  | Disable console colors |

GPU clock control:

| Option | Description |
| --- | --- |
| `--cclock` | Lock core clock |
| `--mclock` | Lock memory clock |
| `--coffset` | Core clock offset |
| `--moffset` | Memory clock offset |
| `--restore-cclock` | Core clock baseline to restore on exit |
| `--restore-mclock` | Memory clock baseline to restore on exit |
| `--reset-oc` | Restore clock settings when the miner exits |
| `--no-reset-oc` | Keep miner-applied clocks after exit |

Diagnostic / maintenance options:

| Option | Description |
| --- | --- |
| `--self-check` | Verify production startup integrity without starting GPU workers |
| `--self-check-network` | Check production service connectivity without mining |
| `--no-submit` | Search normally but do not submit shares |
| `--help` | Show command-line help |
| `--version` | Show N7Miner version |

## Examples

### Single pool (Windows)

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 -p x -d auto
~~~

### Mixed NVIDIA and AMD rig (Windows)

~~~bat
n7miner.exe -a pearlhash -o prl-ru.kryptex.network:7048 -u YOUR_WALLET -w rig01 --devices all --amd-devices all
~~~

### Pool failover

~~~bash
--pool primary.pool:7048,backup1.pool:7048,backup2.pool:7048
~~~

### Console mode

~~~bash
--notui --log-interval 180
~~~

---

[← Back to N7Miner](../README.md)
