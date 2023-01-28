# Serverbot
`serverbot` is a simplistic, lightweight and small (<1000 LOC) server monitoring tool made for FreeBSD.

![alt text](https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/resources/banner.jpg)

## Features
* Easy to use CLI interface with clear help text.
* Metrics like cpu, memory and disk usage.
* Server information like hostname and uptime.
* Overviews with combined metrics and server information.
* Alert function that reports when thresholds have been reached.
* Output to CLI and Telegram are supported.

## How to use
`serverbot` has **features**, **methods** and **options**. Options can be used standalone, but a feature always requires a method and vice versa. Some examples:

```
# options
$ serverbot --help                           # Displays help text
$ serverbot --cron                           # Effectuates automated tasks

# features/methods
$ serverbot --overview ---cli                # Shows a extended overview of server metrics on CLI
$ serverbot --summary --telegram             # Shows a summary of server metrics on Telegram
$ serverbot --alert --cli                    # Shows whether metrics exceed thresholds on CLI
$ serverbot --alert --telegram               # Shows whether metrics exceed thresholds on Telegram
```

For a full list of features, methods and options run `serverbot --help`:
```
$ serverbot --help
Usage:
  serverbot [feature]... [method]...
  serverbot [option]...

Features:
  -s, --server               Basic server information
  -S, --summary              Server overview
  -o, --overview             Extended server overview
  -u, --uptime               Server uptime metrics
  -C, --cpu                  CPU load metrics
  -m, --memory               Basic memory usage metrics
  -M, --memorytree           Extended memory usage metrics
  -d, --disk                 Disk usage metrics
  -n, --network              Interfaces and IP addresses
  -a, --alert                Alerts when metric thresholds are reached

Methods:
  -c, --cli (default)        Output [feature] to command line
  -t, --telegram             Output [feature] to Telegram

Options:
  --cron                     Effectuate cron changes from serverbot config
  --help                     Display this help and exit
  --version                  Display version information and exit
```

## Installing / Getting started
Copy `serverbot` to `/usr/local/bin/serverbot` (owner=`root`, group=`wheel`, permissions=`555` (read & execute)). This will look something like:
```
wget https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/serverbot -O /usr/local/bin/serverbot
chown root:wheel /usr/local/bin/serverbot
chmod 555 /usr/local/bin/serverbot
```
Optionally you can add serverbot's configuration file for additional features:
```
wget https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/serverbot.conf -O /usr/local/etc/serverbot.conf
chown root:wheel /usr/local/etc/serverbot.conf
chmod 555 /usr/local/etc/serverbot.conf
```

## Configuration
General settings and automated tasks can be configured in `/usr/local/etc/serverbot.conf`. Automated tasks can be effectuated with `serverbot --cron`.

## Support
If you have questions, suggestions or find bugs, please let us know via the issue tracker.

## Changelog
### 1.5.0-RELEASE ([28-01-2023](https://github.com/nozel-org/freebsd-serverbot/commit/9297b2545c296697b32938eb851bd90d3e5e12ce))
- Added shorter arguments.
- Added the direct path of command binaries.
- Added colors to CLI output for more clarity.
- Made some comments more clear.
- Fixed a big in automated cronjob creation for Feature Overview.
- Added requirement to run as root when creating cronjobs.
- serverbot now checks whether its run on FreeBSD.
- Added error message for when curl is missing.
- Fixed some issues shellcheck found.
- Alert feature now accepts load thresholds higher than 100%.

### 1.4.0-RELEASE ([27-05-2022](https://github.com/nozel-org/freebsd-serverbot/commit/e007966a2949659d0f223da4ecfb2de7ad2191cd))
- Fixed a bug where hostname was shown twice in feature overview method telegram.
- Zfs arc cache size has been subtracted from memory and alert features.
- Feature memorytree-wide has been removed.
- Wired memory has been broken down to arc and kernel in feature memorytree.

### 1.3.1-RELEASE ([26-05-2022](https://github.com/nozel-org/freebsd-serverbot/commit/6cf4d6ec3051b7912c82adc025366ff3f56207ba))
- Fixed a bug in option cron.

### 1.3.0-RELEASE ([26-05-2022](https://github.com/nozel-org/freebsd-serverbot/commit/64fbec6c31a98963ce64e04c63b6678c6f002739))
- Added ZFS file system compatibility.
- Fixed a bug where SMT_ENABLE in serverbot.conf wouldn't work properly.

### 1.2.0-RELEASE ([02-05-2022](https://github.com/nozel-org/freebsd-serverbot/commit/e839a0a4582919ea0a8547618a4097426083b911))
- Extended serverbot.conf configuration parameter validation.
- Fixed a bug where unused memory wasn't shown correctly.
- Added feature uptime to method telegram.
- Added uptime to feature overview method telegram.
- Added emoji for feature alert method telegram.
- Removed ${TELEGRAM_URL} from serverbot.conf.

### 1.1.0-RELEASE ([23-04-2022](https://github.com/nozel-org/freebsd-serverbot/commit/881c318e0aeac671a045b2701ac40d86dd807d49))
- Changed STABLE tag to RELEASE tag.
- Added serverbot.conf for additional options.
- Added feature Alert.
- Added method Telegram.

### 1.0.0-STABLE ([14-02-2021](https://github.com/nozel-org/freebsd-serverbot/commit/066fc9525af8daa444ba45648c61a5a450609002))
- First stable release.