# Serverbot
`serverbot` is a simplistic, lightweight and small (<1000 LOC) server monitoring tool made for FreeBSD.

![alt text](https://raw.githubusercontent.com/nozel-org/serverbot/master/resources/banner.jpg)

## Features
* Easy to use CLI interface with clear help text.
* Shows and alerts about metrics like CPU, memory and disk usage.
* Shows system information such as kernel, userland, hostname and uptime.
* Reports about available base system and package updates.
* Reports about known package vulnerabilities and checksum mismatches.
* Supports CLI, logger and/or Telegram bots as output methods.
* Features can be automated and cronjobs can be created automatically.

## How to use
`serverbot` has **features**, **methods** and **options**. Options must be used standalone, but a feature can be used with a method. Some examples:

```
# options
$ serverbot --help                           # Displays help text
$ serverbot --cron                           # Effectuates automated tasks

# features/methods
$ serverbot --overview ---cli                # Shows a extended system overview on the CLI
$ serverbot --summary --telegram             # Shows basic server metrics on Telegram
$ serverbot --pkgupdates --cli               # Reports about available package updates
$ serverbot --alert --telegram               # Shows whether system metrics exceed thresholds on Telegram
```

For a full list of features, methods and options run `serverbot --help`:
```
$ serverbot --help
Usage:
  serverbot [feature]                    # No explicit method defaults to method cli
  serverbot [feature] [method]           # Only one feature can be used at a time
  serverbot [feature] [method] [method]  # Multiple methods can be used at once
  serverbot [option]

Features:
  -b, --basic                Basic server information
  -s, --summary              Server metrics overview
  -o, --overview             Extended server overview
      --uptime               Server uptime metrics
      --cpu                  CPU load metrics
      --memory               Basic memory usage metrics
  -M, --memorytree           Extended memory usage metrics
      --disk                 Disk usage metrics
      --network              Interfaces and IP addresses
      --alert                Reports whether metric thresholds are reached
      --baseupdates          Reports about available base system updates
      --pkgupdates           Reports about available package updates
      --pkgaudit             Reports about known package vulnerabilities
      --pkgchecksum          Reports about mismatching package checksums

Methods:
  -c, --cli (default)        Output [feature] to command line
  -l, --logger               Output [feature] to logger
  -t, --telegram             Output [feature] to Telegram

Options:
  --cron                     Effectuate cron changes from serverbot config
  --help                     Display this help and exit
  --version                  Display version information and exit
```

## Installing / Getting started
Copy `serverbot` to `/usr/local/bin/serverbot` (owner=`root`, group=`wheel`, permissions=`555` (read & execute)). This will look something like:
```
wget https://raw.githubusercontent.com/nozel-org/serverbot/master/serverbot -O /usr/local/bin/serverbot
chown root:wheel /usr/local/bin/serverbot
chmod 555 /usr/local/bin/serverbot
```
Optionally you can add serverbot's [configuration file](https://codeberg.org/nozel/serverbot/src/branch/master/serverbot.conf) for additional features and more fine-grained control:
```
wget https://raw.githubusercontent.com/nozel-org/serverbot/master/serverbot.conf -O /usr/local/etc/serverbot.conf
chown root:wheel /usr/local/etc/serverbot.conf
chmod 555 /usr/local/etc/serverbot.conf
```

## Configuration
General settings and automated tasks can be configured in `/usr/local/etc/serverbot.conf`. Automated tasks can be effectuated with `serverbot --cron`.

## Support
If you have questions, suggestions or find bugs, please let us know via the issue tracker.

## License
This software (i.e. code and written documentation in this case) is licensed under the Apache 2.0 license. Do note that this license does not grant permission to use trade names, trademarks, logos, images, service marks or product names of the licensor, except as required for reasonable and customary use in describing the origin of the Work and reproducing the content of the NOTICE file.

## Changelog
### 1.7.0-RELEASE ([24-12-2023](https://codeberg.org/nozel/serverbot/commit/20fb9e8383916531d0fd5fc1202d014693749844))
- Added validation to cron entries in serverbot.conf #68.
- Made method cli the default method when no method is chosen #70.
- Made help text more clear and helpful #76.
- Added a check for the existence of serverbot.conf when using the automatic cronjobs generation #75.
- Removed old references to backupbot #79.
- Added and improved comments for readability #74.
- Fixed inconsistent variable validation #73.

### 1.6.0-RELEASE ([22-12-2023](https://codeberg.org/nozel/serverbot/commit/81841b4b5b2f51d911e733975da4ec1f4cd64243))
Aside from many new features, the existing code has been refactored and improved considerably as well. Despite the large amount of changes, it's completely backwards compatible (hence the minor version increase).

- Changed serverbot's license to Apache-2.0 #51.
- Added support for using multiple methods simultaneously #65.
- Added logger method #20 #64.
- Added feature baseupdates that alerts when base system updates are available #9.
- Added feature pkgupdates that alerts when package updates are available #61.
- Added feature pkgaudit that alerts when packages are vulnerable #62.
- Added feature pkgchecksum that alerts about package checksum mismatches #63.
- Added information about running kernel, installed kernel and userland version to feature overview #58 #57.
- Refactored different pars of the code #52 #54 #53 #55 #56 #59 #60 #66 #67.

### 1.5.0-RELEASE ([28-01-2023](https://github.com/nozel-org/serverbot/commit/9297b2545c296697b32938eb851bd90d3e5e12ce))
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

### 1.4.0-RELEASE ([27-05-2022](https://github.com/nozel-org/serverbot/commit/e007966a2949659d0f223da4ecfb2de7ad2191cd))
- Fixed a bug where hostname was shown twice in feature overview method telegram.
- Zfs arc cache size has been subtracted from memory and alert features.
- Feature memorytree-wide has been removed.
- Wired memory has been broken down to arc and kernel in feature memorytree.

### 1.3.1-RELEASE ([26-05-2022](https://github.com/nozel-org/serverbot/commit/6cf4d6ec3051b7912c82adc025366ff3f56207ba))
- Fixed a bug in option cron.

### 1.3.0-RELEASE ([26-05-2022](https://github.com/nozel-org/serverbot/commit/64fbec6c31a98963ce64e04c63b6678c6f002739))
- Added ZFS file system compatibility.
- Fixed a bug where SMT_ENABLE in serverbot.conf wouldn't work properly.

### 1.2.0-RELEASE ([02-05-2022](https://github.com/nozel-org/serverbot/commit/e839a0a4582919ea0a8547618a4097426083b911))
- Extended serverbot.conf configuration parameter validation.
- Fixed a bug where unused memory wasn't shown correctly.
- Added feature uptime to method telegram.
- Added uptime to feature overview method telegram.
- Added emoji for feature alert method telegram.
- Removed ${TELEGRAM_URL} from serverbot.conf.

### 1.1.0-RELEASE ([23-04-2022](https://github.com/nozel-org/serverbot/commit/881c318e0aeac671a045b2701ac40d86dd807d49))
- Changed STABLE tag to RELEASE tag.
- Added serverbot.conf for additional options.
- Added feature Alert.
- Added method Telegram.

### 1.0.0-STABLE ([14-02-2021](https://github.com/nozel-org/serverbot/commit/066fc9525af8daa444ba45648c61a5a450609002))
- First stable release.
