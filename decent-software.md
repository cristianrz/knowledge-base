# Decent Software

## Introduction

Opinionated list of sysadmin/development software that is/has:

2. Minimal configuration: software should work after a new user spending less than 5 minutes configuring/fixing it
3. Secure
4. Self hosted
5. Convenient: e.g. you can technically browse the web from your terminal, but it's not convenient
6. Already installed on Unix systems
7. Cross-platform
8. Small

in that order. 

## List
 
| Mainstream               | Alternative                     | Comments                                                |
| ------------------------ | ------------------------------- | ------------------------------------------------------- |
| Backups                  | `rsync`                         |                                                         |
| File sharing             | `sftp`                          |                                                         |
| Firewall                 | `ufw`                           | let's be honest, `iptables` is not usable               |
| General purpose language | `go`                            | cause cross-compilation                                 |
| JSON                     | `csv`                           |                                                         |
| MediaWiki                | `cmark`                         |                                                         |
| Offline reading          | `chromium` print to pdf         |                                                         |
| PDF reader               | `chromium`                      | cause sandboxing                                        |
| Package manager          | `pkgsrc`                        | wide platform support                                   |
| Password manager         | `chown`, `chmod` and `sudo cat` | cause if someone has root access you're already screwed |
| Scripting language       | `/bin/sh`                       |                                                         |
| Server dashboard         | `cockpit`                       |                                                         |
| Static site generator    | `cmark`                         |                                                         |
| Text editor              | `mg`                            |                                                         |
| VPN                      | `wireguard`                     |                                                         |
| `ansible`                | `/bin/sh` + `ssh`               |                                                         |
| `apache`                 | busybox httpd                   |                                                         |
| `bash`                   | `ash`                           |                                                         |
| `chrome`                 | `chromium`                      | big but SECURITY                                        |
| `coreutils`              | `busybox`                       |                                                         |
| `docker`                 | `chroot`                        | only for trusted software, otherwise `cockpit-machines` |
| `git`                    | `cvs`                           |                                                         |
| `glibc`                  | `musl`                          |                                                         |
| `make`                   | `bmake`                         |                                                         |
| `man-db`                 | `mandoc`                        |                                                         |
| `perl`                   | `awk`                           |                                                         |
| `qbittorrent`            | `transmission-daemon`           |                                                         |
| `selinux`                | `chmod`                         |                                                         |
| `sudo`                   | `doas`                          |                                                         |
| `systemd`                | `openrc`                        | used in alpine and gentoo                               |
| `virtualbox`             | `cockpit-machines`              |                                                         |

