# Serverbot

Serverbot is a simplistic, lightweight and small server monitoring tool made for FreeBSD.

![alt text](https://codeberg.org/nozel/serverbot/raw/branch/master/resources/banner.jpg)

## Features

* Easy to use CLI interface with clear help text.
* Shows and alerts about metrics like CPU, memory, disk usage and temperature.
* Shows system information such as kernel, userland, hostname and uptime.
* Reports about available base system and package updates.
* Reports about known package vulnerabilities and checksum mismatches.
* Supports CLI, logger and/or Telegram bots as output methods.
* Features can be automated and cronjobs can be created automatically.

Serverbot is completely written in basic shell and should run on any FreeBSD device 'out of the box'.

## How to use

Serverbot has **features**, **methods** and **options**. Options must be used standalone, but a feature can be used with a method. Some examples:

```
# features/methods
$ serverbot --overview ---cli                # Shows a extended system overview on the CLI
$ serverbot --summary --telegram             # Shows basic server metrics on Telegram
$ serverbot --pkgupdates --cli               # Reports about available package updates on the CLI
$ serverbot --alert --telegram               # Shows whether system metrics exceed thresholds on Telegram

# options
$ serverbot --help                           # Displays help text
$ serverbot --cron                           # Effectuates automated tasks
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
wget https://codeberg.org/nozel/serverbot/raw/branch/master/serverbot -O /usr/local/bin/serverbot
chown root:wheel /usr/local/bin/serverbot
chmod 555 /usr/local/bin/serverbot
```
Optionally you can add serverbot's [configuration file](https://codeberg.org/nozel/serverbot/src/branch/master/serverbot.conf) for additional features and more fine-grained control:
```
wget https://codeberg.org/nozel/serverbot/raw/branch/master/serverbot.conf -O /usr/local/etc/serverbot.conf
chown root:wheel /usr/local/etc/serverbot.conf
chmod 555 /usr/local/etc/serverbot.conf
```

Do note that Serverbot requires the following dependencies:

* `curl` for using Telegram as a method.
* `smartctl` for temperature checks and alerts.

You can install these by running `pkg install curl smartmontools`.

## Configuration and automated tasks

General settings, automated tasks and the Telegram bot can be configured in `/usr/local/etc/serverbot.conf`. Automated tasks can be effectuated with `serverbot --cron`.

## Support

If you have questions, suggestions or find bugs, please let us know via the issue tracker.
 
## License

This software (i.e. code and written documentation in this case) is licensed under the Apache 2.0 license. Do note that this license does not grant permission to use trade names, trademarks, logos, images, service marks or product names of the licensor, except as required for reasonable and customary use in describing the origin of the Work and reproducing the content of the NOTICE file.
