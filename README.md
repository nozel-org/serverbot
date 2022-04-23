# Serverbot
`serverbot` is a simplistic, lightweight and small (<50 kilobytes/<1000 LOC) server monitoring tool made for FreeBSD.

## Features
* Easy to use CLI interface with clear help text.
* Metrics like cpu, memory and disk usage.
* Server information like hostname and uptime.
* Overviews with combined metrics and server information.
* Alert function that reports when thresholds have been reached.
* Output to CLI and Telegram are supported.

Some examples:
```
# serverbot --overview
hostname       freebsd.development.nozel.org
os/kernel      FreeBSD 12.1-RELEASE-p13 (amd64)
uptime         4 hours 30 minutes 11 seconds

cpu load       1 min        5 min          15 min
               0.34 (8%)    0.37 (9%)      0.37 (9%)

memory         total        used           free
               8311 MB      175 MB (2%)    8043 MB (96%)

disk usage     total        used           free
               68 GB        4587 MB (6%)   63 GB (93%)

```
```
# serverbot --memorytree
Total             8311 MB       100%
|-- Free          8043 MB        96%
|-- Used           176 MB         2%
|   |-- Active    3538 KB         0%
|   `-- Wired      172 MB         2%
|-- Inactive        92 MB         1%
|-- Laundry           0 B         0%
`-- Cache             0 B         0%
```

All features output to the CLI. At a later stage support for Telegram will be added.

## How to use
Quite easy! Run `serverbot` with a valid (combination of) argument(s):
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

## How to install
### Requirements
Only base FreeBSD is required to run `serverbot`. To use the Telegram output method, additionaly `curl` is required.

### Install
Copy `freebsd-serverbot` to `/usr/local/bin/serverbot` (owner=`root`, group=`wheel`, permissions=`555` (read & execute)). This will look something like:
```
# wget https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/freebsd-serverbot -O /usr/local/bin/serverbot
# chown root:wheel /usr/bin/serverbot
# chmod 555 /usr/bin/serverbot
```
Optionally you can add serverbot's configuration file for additional options:
```
# wget https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/serverbot.conf -O /usr/local/etc/serverbot.conf
```

## Support
If you have questions/suggestions about `serverbot` or find bugs, please let us know via the issue tracker.

## Changelog
### 1.1.0-RELEASE (23-04-2022)
- Changed STABLE tag to RELEASE tag.
- Added serverbot.conf for additional options.
- Added feature Alert.
- Added method Telegram.

### 1.0.0-STABLE ([14-02-2021](https://github.com/nozel-org/freebsd-serverbot/commit/066fc9525af8daa444ba45648c61a5a450609002))
- First stable release.