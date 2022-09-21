# FreeBSD

## Package management

```sh
# search package
pkg search vim

# installed package info
pkg info vim

# list all installed packages
pkg info

# list leaves
pkg prime-list

# install package
pkg install vim

# remove package
pkg delete vim

# upgrade
pkg upgrade

# autoremove
pkg autoremove
```

## Service management

```sh
# start service if enabled
service sshd start

# start service if disabled
service sshd onestart

# stop service
service sshd stop

# /etc/rc.conf usually used for all systems /etc/rc.conf.local only
# for this system

# enable service
sysrc sshd_enable=YES

# disable service
sysrc sshd_enable=NO

# check if a service is enabled
service sshd rcvar
```

Keywords in `rc.d` files

```
PROVIDE: (services this file provides)
REQUIRE: (run after these)
BEFORE: (run before these)
```

## Networking

```sh
# configure dhcp at boot
sysrc ifconfig_dc0="DHCP"

# configure static at boot
sysrc ifconfig_dc0="inet 192.168.1.3 netmask 255.255.255.0"
```