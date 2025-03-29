# Serverbot Manual

Serverbot is a simplistic, lightweight and small server monitoring tool made for FreeBSD.

| # | Chapter | Description |
| --- | ------- | ----------- |
| 0 | General information | How to use and install Serverbot |
| 1 | Features | A overview of features and their specifics |
| 2 | Methods | A overview of methods and their specifics |
| 3 | Options | A overview of options and their specifics |
| 4 | Configuration | A overview of configuration parameters |
| 5 | File locations | A overview of default file locations |

## 0 General information

### 0.1 How to use

Serverbot has **features**, **methods** and **options**. Options must be used standalone, but a feature can be used with a method:

```
serverbot [feature]                    # No explicit method defaults to method cli
serverbot [feature] [method]           # Only one feature can be used at a time
serverbot [feature] [method] [method]  # Multiple methods can be used at once
serverbot [option]
```
Some examples:
```
serverbot --overview                   # Outputs feature Overview to CLI
serverbot --overview --telegram        # Outputs feature Overview to Telegram
serverbot --overview --telegram --cli  # Outputs feature Overview to Telegram and CLI
serverbot --help                       # Prints the help text to CLI
```

### 0.2 How to install

Serverbot can be run without a configuration file, but the configuration file is required to 'unlock' all of Serverbot's features. The files can be downloaded from the [project's repository](https://codeberg.org/nozel/serverbot) on Codeberg.

That being said, never trust some random stranger on the internet and always be cautious when installing new software from a random git repository. In this case Serverbot is open source, so you can verify for yourself whather this software should be trusted and meets your quality standards.

#### 0.2.1 Install `serverbot`

The [program](https://codeberg.org/nozel/serverbot/src/branch/master/serverbot) can be installed by placing `serverbot` in `/usr/local/bin/serverbot` with owner `root`, group `wheel` and permissions `555` (read & execute).
This will look something like:

```
wget https://codeberg.org/nozel/serverbot/raw/branch/master/serverbot -O /usr/local/bin/serverbot
chown root:wheel /usr/local/bin/serverbot
chmod 555 /usr/local/bin/serverbot
```

#### 0.2.2 Install `serverbot.conf`

The [configuration file](https://codeberg.org/nozel/serverbot/src/branch/master/serverbot.conf) can be installed by placing `serverbot.conf` in `/usr/local/etc/serverbot.conf` with owner `root`, group `wheel` and permissions `555` (read & execute). This will look something like:

```
wget https://codeberg.org/nozel/serverbot/raw/branch/master/serverbot.conf -O /usr/local/etc/serverbot.conf
chown root:wheel /usr/local/etc/serverbot.conf
chmod 555 /usr/local/etc/serverbot.conf
```

## 1 Features

Serverbot has the following features:

| # | Feature | Description |
| --- | ------- | ----------- |
| 1.1 | Basic | Provides basic server information (part of Overview) |
| 1.2 | Summary | Provides a overview of basic server metrics |
| 1.3 | Overview | Provides a extended overview of server information and server metrics |
| 1.4 | Uptime | Shows the operating system's uptime (part of Overview) |
| 1.5 | CPU | Shows CPU load metrics (part of Summary and Overview) |
| 1.6 | Memory | Shows memory usage metrics (part of Summary and Overview) |
| 1.7 | Memorytree | Shows a extended memory usage overview |
| 1.8 | Disk | Shows disk usage metrics (part of Summary and Overview) |
| 1.9 | Alert | Reports whether metric and temperature thresholds are reached |
| 1.10 | Baseupdates | Reports about available FreeBSD base system updates |
| 1.11 | Pkgupdates | Reports about available package updates |
| 1.12 | Pkgaudit | Reports about known package vulnerabilities |
| 1.13 | Pkgchecksum | Reports about mismatching package checksums |

### 1.1 Feature Basic

Feature Basic provides basic server information, limited to the server's hostname and FreeBSD's version. Ideal to quickly check the server's hostname and/or FreeBSD version.

This feature can be used with the arguments `serverbot -b` and `serverbot --basic`. 

#### 1.1.1 Automation

Automation cannot be configured for this feature.

### 1.1.2 Methods

The default CLI output looks like:
```
root@server:~ # serverbot --basic
server.hostname.tld
FreeBSD 13.2-RELEASE-p8 (amd64)
```
Other methods like Logger or Telegram report the same information.

### 1.2 Feature Summary

Feature Summary provides a overview of some primary server metrics such as CPU load, memory usage and disk usage. Ideal to quickly check on the server's status in terms of available resources.

This feature can be used with the arguments `serverbot -s` and `serverbot --summary`.

#### 1.2.1 Automation

Automation can be enabled in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `SUMMARY_LOGGER=''` | Enables automation with method Logger, either 'YES' or 'NO' | `NO` |
| `SUMMARY_TELEGRAM=''` | Enables automation with method Telegram, either 'NO' or 'NO' | `90` |
| `SUMMARY_CRON=''` | Cron schedule for automated tasks | `0 8 * * 1` |

#### 1.2.2 Output

The default CLI output looks like:
```
root@server:~ # serverbot --summary
load           0.16              (1%)
memory      7952 MB /   33 GB    (23%)
disk         813 GB / 1906 GB    (42%)
```
Other methods like Logger or Telegram report the same information.

### 1.3 Feature Overview

Feature Overview provides a extended overview of server information and server metrics such as hostname, os, installed kernel, running kernel, userland version, uptime and primary server metrics.

This feature can be used with the arguments `serverbot -o` and `serverbot --overview`. 

#### 1.3.1 Automation

Automation can be enabled in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `OVERVIEW_LOGGER=''` | Enables automation with method Logger, either 'YES' or 'NO' | `NO` |
| `OVERVIEW_TELEGRAM=''` | Enables automation with method Telegram, either 'NO' or 'NO' | `90` |
| `OVERVIEW_CRON=''` | Cron schedule for automated tasks | `0 8 * * 1` |

#### 1.3.2 Output

The default CLI output looks like:
```
root@server:~ # serverbot --overview
hostname            server.hostname.tld
operating system    FreeBSD (amd64)
kernel (installed)  13.2-RELEASE-p8
kernel (running)    13.2-RELEASE-p8
userland version    13.2-RELEASE-p9
uptime              41 days 2 hours 6 minutes 23 seconds

cpu load       1 min        5 min          15 min
               0.32 (2%)    0.25 (1%)      0.19 (1%)

memory         total        used           unused
               33 GB        7947 MB (23%)  25 GB (76%)

disk usage     total        used           free
               1906 GB      813 GB (42%)   1093 GB (57%)
```
Other methods like Logger or Telegram report the same information.

### 1.4 Feature Uptime

Feature Uptime only shows the operating system's uptime. This is also part of Feature Overview.

This feature can be used with the argument `serverbot --uptime`.

#### 1.4.1 Automation

Automation cannot be configured for this feature.

### 1.4.2 Methods

The default CLI output looks like:
```
root@server:~ # serverbot --uptime
41 days 2 hours 10 minutes 14 seconds
```
Other methods like Logger or Telegram report the same information.

### 1.5 Feature CPU

Feature CPU only shows the server's CPU load. This is also part of Feature Summary and Feature Overview.

This feature can be used with the argument `serverbot --cpu`.

#### 1.5.1 Automation

Automation cannot be configured for this feature.

### 1.5.2 Methods

The default CLI output looks like:
```
root@server:~ # serverbot --cpu
cpu load       1 min        5 min          15 min
               0.28 (1%)    0.26 (1%)      0.20 (1%)
```
Other methods like Logger or Telegram report the same information.

### 1.6 Feature Memory

Feature Memory only shows the server's basic memory usage. This is also part of Feature Summary and Feature Overview.

This feature can be used with the argument `serverbot --memory`. 

#### 1.6.1 Automation

Automation cannot be configured for this feature.

### 1.6.2 Methods

The default CLI output looks like:
```
root@server:~ # serverbot --memory
memory         total        used           unused
               33 GB        7941 MB (23%)  25 GB (76%)
```
Other methods like Logger or Telegram report the same information.


### 1.7 Feature Memorytree

Feature Memorytree provides a extended memory usage overview for all available memory categories.

This feature can be used with the arguments `serverbot -M` and `serverbot --memorytree`. 

#### 1.7.1 Automation

Automation cannot be configured for this feature.

#### 1.7.2 Methods

The default CLI output looks like:
```
root@server:~ # serverbot --memorytree
Total                  33 GB       100%
|- Free              4073 MB        12%
|- Used                23 GB        71%
   |- Active         1415 MB         4%
   `- Wired            22 GB        67%
      |- Kernel      6525 MB        19%
      `- Arc           15 GB        47%
|- Inactive          5466 MB        16%
|- Laundry               0 B         0%
`- Cache                 0 B         0%
```
Other methods are not available for this feature.

### 1.8 Feature Disk

Feature Disk only shows the server's basic disk usage. This is also part of Feature Summary and Feature Overview.

This feature can be used with the argument `serverbot --disk`. 

#### 1.8.1 Automation

Automation cannot be configured for this feature.

### 1.8.2 Methods

The default CLI output looks like:
```
root@server:~ # serverbot --disk
disk usage     total        used           free
               1906 GB      813 GB (42%)   1093 GB (57%)
```
Other methods like Logger or Telegram report the same information.

### 1.9 Feature Alert

Feature Alert reports whether metric and temperature thresholds are reached. Perfect for keeping an eye on the server's health. CPU load, memory usage, disk usage and CPU temperature are supported by this feature.

This feature can be used with the arguments `serverbot -a` and `serverbot --alert`.

#### 1.9.1 Automation

Automation can be enabled in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `ALERT_LOGGER=''` | Enables automation with method Logger, either 'YES' or 'NO' | `NO` |
| `ALERT_TELEGRAM=''` | Enables automation with method Telegram, either 'NO' or 'NO' | `90` |
| `ALERT_CRON=''` | Cron schedule for automated tasks | `*/15 * * * *` |

#### 1.9.2 Thresholds

Thresholds can be set in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `THRESHOLD_LOAD=''` | Rounded percentage between 1-infinite, no % character | `95` |
| `THRESHOLD_MEMORY=''` | Rounded percentage between 1-100, no % character | `90` |
| `THRESHOLD_DISK=''` | Rounded percentage between 1-100, no % character | `80` |
| `THRESHOLD_CPUTEMP=''` | Rounded temperature in degrees Celsius between 1-infinite | `95` |
| `THRESHOLD_SSDTEMP=''` | Rounded temperature in degrees Celsius between 1-infinite | `70` |
| `THRESHOLD_HHDTEMP=''` | Rounded temperature in degrees Celsius between 1-infinite | `50` |

#### 1.9.3 Methods

The default CLI output looks like:
```
root@server:~ # serverbot --alert
[i] current server load of 1% does not exceed the threshold of 95%
[i] current memory usage of 23% does not exceed the threshold of 90%
[i] current disk usage of 42% does not exceed the threshold of 80%
[i] current CPU temperature of 50C does not exceed the threshold of 95C
[i] current SSD temperature of 37C does not exceed the threshold of 70C
[i] current HDD temperature of 42C does not exceed the threshold of 50C
```
```
root@server:~ # serverbot --alert
[!] current server load of 110% exceeds the threshold of 95%
[!] current memory usage of 93% exceeds the threshold of 90%
[!] current disk usage of 87% exceeds the threshold of 80%
[!] current CPU temperature of 108C exceeds the threshold of 95C
[!] current SSD temperature of 73C exceeds the threshold of 70C
[!] current HDD temperature of 56C exceeds the threshold of 50C
```
Other methods like Telegram or Logger only report when metrics or temperatures exceed the thresholds.

### 1.10 Feature Baseupdates

Feature Baseupdates reports about available FreeBSD base system updates.

This feature can be used with the argument `serverbot --baseupdates`.

#### 1.10.1 Automation

Automation can be enabled in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `BASE_UPDATES_LOGGER=''` | Enables automation with method Logger, either 'YES' or 'NO' | `NO` |
| `BASE_UPDATES_TELEGRAM=''` | Enables automation with method Telegram, either 'NO' or 'NO' | `90` |
| `BASE_UPDATES_CRON=''` | Cron schedule for automated tasks | `0 4 * * *` |

#### 1.10.2 Methods

All methods only report when base updates are available. The default CLI output looks like:
```
```
Other methods like Logger or Telegram report the same information.

### 1.11 Feature Pkgupdates

Feature Pkgupdates reports about available package updates.

This feature can be used with the argument `serverbot --pkgupdates`.

#### 1.11.1 Automation

Automation can be enabled in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `PKG_UPDATES_LOGGER=''` | Enables automation with method Logger, either 'YES' or 'NO' | `NO` |
| `PKG_UPDATES_TELEGRAM=''` | Enables automation with method Telegram, either 'NO' or 'NO' | `90` |
| `PKG_UPDATES_CRON=''` | Cron schedule for automated tasks | `0 5 * * *` |

#### 1.11.2 Methods

All methods only report when pkg updates are available. The default CLI output looks like:
```

```
Other methods like Logger or Telegram report the same information.

### 1.12 Feature Pkgaudit

Feature Pkgaudit reports about known package vulnerabilities.

This feature can be used with the argument `serverbot --pkgaudit`.

#### 1.12.1 Automation

Automation can be enabled in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `PKG_AUDIT_LOGGER=''` | Enables automation with method Logger, either 'YES' or 'NO' | `NO` |
| `PKG_AUDIT_TELEGRAM=''` | Enables automation with method Telegram, either 'NO' or 'NO' | `90` |
| `PKG_AUDIT_CRON=''` | Cron schedule for automated tasks | `0 6 * * *` |

#### 1.12.2 Methods

All methods only report when vulnerabilities are found. The default CLI output looks like:
```

```
Other methods like Logger or Telegram report the same information.

### 1.13 Feature Pkgchecksum

Feature Pkgchecksum reports about mismatching package checksums.

This feature can be used with the argument `serverbot --pkgchecksum`.

#### 1.13.1 Automation

Automation can be enabled in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `PKG_CHECKSUM_LOGGER=''` | Enables automation with method Logger, either 'YES' or 'NO' | `NO` |
| `PKG_CHECKSUM_TELEGRAM=''` | Enables automation with method Telegram, either 'NO' or 'NO' | `90` |
| `PKG_CHECKSUM_CRON=''` | Cron schedule for automated tasks | `0 7 * * *` |

#### 1.13.2 Methods

All methods only report when mismatching checksums are found. The default CLI output looks like:
```

```
Other methods like Logger or Telegram report the same information.

## 2 Methods

Unless another method argument is used explicitly, CLI is the default method for all features. Serverbot has the following methods:

| # | Method | Description |
| --- | ------- | ----------- |
| 2.1 | CLI | Prints the feature's output to the CLI |
| 2.2 | Logger | Writes the feature's output to Logger |
| 2.3 | Telegram | Sends the feature's output to Telegram |

### 2.1 Method CLI

Method CLI prints the feature's output to the CLI. All features support method CLI.

Method CLi can be used with the argument `serverbot --feature --cli`. Method CLI is also the default method used when no method argument is given.

### 2.2 Method Logger

Method Logger writes the Feature's output to `logger`, which can often be found in `/var/log/messages`. All features support method Logger, except for feature Memorytree.

Method Logger can be used with the argument `serverbot --feature --logger`.

More information about Logger can be found in [FreeBSD's Manual Pages](https://man.freebsd.org/cgi/man.cgi?query=logger).

### 2.3 Method Telegram

Method Telegram sends the feature's output to a Telegram bot. All features support method Telegram, except for feature Memorytree.

Method Telegram can be used with the argument `serverbot --feature --telegram`.

#### 2.3.1 Configure Telegram bot

The Telegram bot's access token and the chat ID need to be configured before method Telegram can be used. This can be configured in `serverbot.conf` with the following configuration parameters:

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `TELEGRAM_TOKEN=''` | Telegram token | n/a |
| `TELEGRAM_CHAT=''` | Telegram chat ID | n/a |

For information on how to aquire a Telegram bot, look at [Telegram's documentation](https://core.telegram.org/bots).

## 3 Options

Serverbot has the following options:

| # | Option | Description |
| --- | ------- | ----------- |
| 3.1 | Cron | Effectuates the configured automation parameters in `serverbot.conf` to the cron job scheduler |
| 3.2 | Help | Prints Serverbot's help text to the CLI |
| 3.3 | Version | Prints Serverbot's version, version date and copyright/license information to the CLI |

### 3.1 Option Cron

Option Cron effectuates the configured automation parameters in `serverbot.conf` to the cron job scheduler. It checks for cron validity and adds, removes or changes cron entries automatically.

Option Cron can be used with the argument `serverbot --cron`.

#### 3.1.1 Output

The CLI output looks like:
```
root@server:~ # serverbot --cron
[0] Check whether serverbot.conf exists
 >  found /usr/local/etc/serverbot.conf
[1] Checking cron configuration validity
 >  Skipping SUMMARY_CRON (disabled)
 >  Validating OVERVIEW_CRON (enabled)
 >   [i] supported characters
 >   [i] supported format
 >  Validating ALERT_CRON (enabled)
 >   [i] supported characters
 >   [i] supported format
 >  Validating BASE_UPDATES_CRON (enabled)
 >   [i] supported characters
 >   [i] supported format
 >  Validating PKG_UPDATES_CRON (enabled)
 >   [i] supported characters
 >   [i] supported format
 >  Validating PKG_AUDIT_CRON (enabled)
 >   [i] supported characters
 >   [i] supported format
 >  Validating PKG_CHECKSUM_CRON (enabled)
 >   [i] supported characters
 >   [i] supported format
 >  All CRON entries in /usr/local/etc/serverbot.conf are valid
[2] Removing old serverbot cronjob
 >  Done
[3] Adding configured automated tasks for features:
 >  OVERVIEW    with cron schedule 0 8 * * 1
 >  ALERT       with cron schedule */15 * * * *
 >  BASEUPDATES with cron schedule 0 4 * * *
 >  PKGUPDATES  with cron schedule 0 6 * * *
 >  PKGAUDIT    with cron schedule 0 4 * * *
 >  PKGCHECKSUM with cron schedule 0 4 * * *
[4] Changing cronjob owner and group to root:wheel
 >  Done

All done! The new schedule can be found in /etc/cron.d/serverbot.
```

### 3.2 Option Help

Option Help prints Serverbot's help text to the CLI.

Option Help can be used with the argument `serverbot --help`.

#### 3.2.1 Output

The CLI output looks like:
```
root@server:~ # serverbot --help
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
      --alert                Reports whether metric and temperature thresholds are reached
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

### 3.3 Option Version

Option Version prints Serverbot's version, version date and copyright/license information to the CLI.

Option Version can be used with the argument `serverbot --version`.

#### 3.3.1 Output

The CLI output looks like:
```
root@server:~ # serverbot --version
Serverbot 1.9.0 (02-02-2024)
Copyright (C) 2016 S. Veeke. All rights reserved.
SPDX-License-Identifier: Apache-2.0.
```
## 4 Configuration

Serverbot will work without a configuration file just fine, but if you need automated tasks or more fine-grained control over Serverbot then you need a configuration file in `/usr/local/etc/serverbot.conf`.

This [example configuration file](https://codeberg.org/nozel/serverbot/raw/branch/master/serverbot.conf) with all configuration parameters present can be used as a template to create your own configuration file.

Automated tasks can be effectuated by running `serverbot --cron`.

### 4.1 General settings

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `SMT_ENABLE=''` | Halfs the amount of threads in CPU load calculation, if you don't want to inflate threads due of multithreading, either `'YES'` or `'NO'` | `NO` |
| `ZFS_ENABLE=''` | Set to 'NO' when running UFS, either `'YES'` or `'NO'` | `YES` |

### 4.2 Alert configuration

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `LOAD_INTERVAL=''` | What load interval is used (1, 5 or 15), either `1`, `5` or `15` | `15` |
| `THRESHOLD_LOAD=''` | CPU load threshold, rounded percentage between 1-infinite, no % character | `95` |
| `THRESHOLD_MEMORY=''` | Memory usage threshold, rounded percentage between 1-100, no % character | `90` |
| `THRESHOLD_DISK=''` | Disk usage threshold, rounded percentage between 1-100, no % character | `80` |
| `THRESHOLD_CPUTEMP=''` | CPU temperature threshold, rounded temperature in degrees Celsius between 1-infinite | `95` |
| `THRESHOLD_SSDTEMP=''` | Rounded temperature in degrees Celsius between 1-infinite | `70` |
| `THRESHOLD_HHDTEMP=''` | Rounded temperature in degrees Celsius between 1-infinite | `50` |

### 4.3 Automated tasks

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `FEATURE_LOGGER=''` | Configure a automated task for this feature with method Logger, either `'YES'` or `'NO'` | `NO` |
| `FEATURE_TELEGRAM=''` | Configure a automated task for this feature with method Telegram, either `'YES'` or `'NO'` | `NO` |
| `FEATURE_CRON=''` | Configure a cron job schedule for this feature, only valid cron job schedules, e.g. `0 4 * * *`, `*/15 * * * *`, `15 6 * * 1` | depends |

### 4.4 Telegram configuration

| Parameter | Description | Default value |
| --------- | ----------- | ------------- |
| `TELEGRAM_TOKEN=''` | Telegram token | `telegram_token_here` |
| `TELEGRAM_CHAT=''` | Telegram chat ID | `telegram_id_here` |

For information on how to aquire a Telegram bot, look at [Telegram's documentation](https://core.telegram.org/bots).

## 5 File locations

Serverbot's default files, file locations, owners and permissions are:

| File | Location | Owner | Permission |
| ---- | -------- | ----- | ---------- |
| Application | `/usr/local/bin/serverbot` | `root`:`wheel` | `555` |
| Configuration file | `/usr/local/etc/serverbot.conf` | `root`:`wheel` | `555` |
| Cronjob file | `/etc/cron.d/serverbot` | `root`:`wheel` | `644` |
