# Serverbot
Serverbot is a simple and small (<50 kilobytes/<1000 LOC) server monitoring tool that is both easy to use and easy to extend. We found most monitoring software to be overkill for our needs and made a simplistic and lightweight alternative. It's well commented and can be easily extended or hacked to provide more features.

`freebsd-serverbot` is a full rewrite of [`serverbot`](https://github.com/nozel-org/serverbot) tailored to FreeBSD and POSIX shell. `freebsd-serverbot` will have more functionality than its Linux counterpart. It's still a work in progress and isn't ready for use on production systems yet.

## Planned features
| Feature | Methods | Status |
| ------- | ------- | ------ |
| Basic memory overview (like `free` on Linux) | CLI | WIP |
| Extensive memory overview | CLI | WIP |
| CPU and load overview | CLI | WIP |
| Storage overview | CLI | WIP |
| Network overview | CLI | WIP |
| Basic server overview | CLI | WIP |
| Complete server overview | CLI | WIP |
| Alerts with configurable thresholds | CLI<br>Telegram<br>Email | WIP |
| Update notices for system and packages | CLI<br>Telegram<br>Email | WIP |
| EOL operating system notice | CLI<br>Telegram<br>Email | WIP |

## Requirements
We try to limit the number of required dependencies to a minimum. Outside FreeBSD base, `curl` and `wget` are required as of now.
