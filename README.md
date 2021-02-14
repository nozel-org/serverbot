# Serverbot
`serverbot` is a simple and small (<50 kilobytes/<1000 LOC) server monitoring tool that is both easy to use and easy to extend. We found most monitoring software to be overkill for our needs and made a simplistic and lightweight alternative. It's well commented and can be easily extended or hacked to provide more features.

This started as a full rewrite of [`serverbot` for Linux](https://github.com/nozel-org/serverbot), but then specifically tailored to FreeBSD and POSIX shell. It will have more functionality than its Linux counterpart. It's still a work in progress and isn't ready for use on production systems yet.

## Features
* Easy to use CLI interface with clear help text.
* Show basic server information.
* Show server uptime information.
* Show CPU load metrics.
* Show basic or extended memory metrics.
* Show basic disk usage metrics.
* Show summary with most important metrics.
* Show overview with all metrics and information.

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
 --uptime               Server uptime metrics
 --cpu                  CPU load metrics
 --memory               Basic memory usage metrics
 --memorytree           Extended memory usage metrics
 --memorytree-wide      Wide mode extended memory usage metrics
 --disk                 Disk usage metrics
 --network              Interfaces and IP addresses
 --summary              Basic server overview
 --overview             Extended server overview

Methods:
 --cli (default)        Output [feature] to command line

Options:
 --help                 Display this help and exit
 --version              Display version information and exit
```

## How to install
### Requirements
Only FreeBSD is required to run `serverbot`. In the future when `telegram` has been added as a method, `curl` will be required for that functionality as well.

### Install
Copy `freebsd-serverbot` to `/usr/bin/serverbot` (owner=`root`, group=`wheel`, permissions=`555` (read & execute)). This will look something like:
```
# wget https://raw.githubusercontent.com/nozel-org/freebsd-serverbot/master/freebsd-serverbot -O /usr/bin/serverbot
# chown root:wheel /usr/bin/serverbot
# chmod 555 /usr/bin/serverbot
```

## Support
If you have questions/suggestions about `serverbot` or find bugs, please let us know via the issue tracker.

## Changelog
### 1.0.0-STABLE (14-02-2021)