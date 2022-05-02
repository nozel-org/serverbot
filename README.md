# Serverbot
`serverbot` is a simplistic, lightweight and small (<1000 LOC) server monitoring tool made for FreeBSD.

## Features
* Easy to use CLI interface with clear help text.
* Metrics like cpu, memory and disk usage.
* Server information like hostname and uptime.
* Overviews with combined metrics and server information.
* Alert function that reports when thresholds have been reached.
* Output to CLI and Telegram are supported.

## Installing / Getting started
Copy `serverbot` to `/usr/local/bin/serverbot` (owner=`root`, group=`wheel`, permissions=`555` (read & execute)). This will look something like:
```
# wget https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/serverbot -O /usr/local/bin/serverbot
# chown root:wheel /usr/local/bin/serverbot
# chmod 555 /usr/local/bin/serverbot
```
Optionally you can add serverbot's configuration file for additional options:
```
# wget https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/serverbot.conf -O /usr/local/etc/serverbot.conf
```
When installed, run `serverbot --overview`.

## Configuration
General settings and automated tasks can be configured in `/usr/local/etc/serverbot.conf`. Automated tasks can be effectuated with `serverbot --cron`.

## How to use
`serverbot` has **features**, **methods** and **options**. Options can be used standalone, but a feature always requires a method and vice versa. Some examples:"

```
# options
$ serverbot --help                           # Displays help text
$ serverbot --cron                           # Effectuates automated tasks

# features/methods
$ uptimebot --overview ---cli                # Shows a extended overview of server metrics on CLI
$ uptimebot --summary --telegram             # Shows a summary of server metrics on Telegram
$ uptimebot --alert --cli                    # Shows whether metrics exceed thresholds on CLI
$ uptimebot --alert --cli                    # Shows whether metrics exceed thresholds on Telegram
```

#### Some output examples:
```
# serverbot --overview
hostname       hostname.domain.tld
os/kernel      FreeBSD 13.0-RELEASE-p11 (amd64)
uptime         3 days 7 hours 36 minutes 9 seconds

cpu load       1 min        5 min          15 min
               3.62 (15%)   3.41 (14%)     3.43 (14%)

memory         total        used           free
               33 GB        8516 MB (25%)  24 GB (74%)

disk usage     total        used           free
               1911 GB      3667 MB (0%)   1907 GB (99%)
```
```
# serverbot --memorytree
Total               33 GB       100%
|-- Free           733 MB         2%
|-- Used          8608 MB        25%
|   |-- Active    6115 MB        18%
|   `-- Wired     2492 MB         7%
|-- Inactive        23 GB        71%
|-- Laundry       3469 KB         0%
`-- Cache             0 B         0%
```
```
# serverbot --alert
[i] the current server load of 14% does not exceeds the threshold of 95%
[i] the current memory usage of 25% does not exceeds the threshold of 90%
[i] the current disk usage of 0% does not exceeds the threshold of 80%
```
```
# serverbot --help
Usage:
 serverbot [feature]... [method]...
 serverbot [option]...

Features:
 --server               Basic server information
 --summary              Server overview
 --overview             Extended server overview
 --uptime               Server uptime metrics
 --cpu                  CPU load metrics
 --memory               Basic memory usage metrics
 --memorytree           Extended memory usage metrics
 --memorytree-wide      Wide mode extended memory usage metrics
 --disk                 Disk usage metrics
 --network              Interfaces and IP addresses
 --alert                Alerts when metric thresholds are reached

Methods:
 --cli (default)        Output [feature] to command line
 --telegram             Output [feature] to Telegram

Options:
 --cron                 Effectuate cron changes from serverbot config
 --help                 Display this help and exit
 --version              Display version information and exit
```

For a full list of features, methods and options run `serverbot --help`.

## Support
If you have questions, suggestions or find bugs, please let us know via the issue tracker.

## Changelog
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