# Linux 

## Fork/exec

* When you run a command on the shell if first forks the process and then
  execs the command
* `exec` does not fork
* `ps f` shows processes as a tree
* a child inherits stdin, stdout and stderr

## bg/fg

* Ctrl + z suspends the process
* `jobs` shows jobs running
* `fg` brings a process to the foreground (if there is only one)
* `fg %3` brings process no. 3 to the foreground
* T means stopped, S means sleep
* + is last job and - is second to last

## kill

* `ps ax` show all processes
* `kill -l` list of signals
* `kill -15` sigterm
* `kill` sigterm, kills gracefully
* `kill -2` sigint
* `kill -1` sighup
* `kill -9` sigkill, brute force kill

## Check open ports

```terminal
# ss -lpntu
# ss -plunt
```

or 

```
# lsof -i -P -n
```

## Run process as user without login

```bash
su -s /bin/bash -c '/path/to/your/script' testuser
```

## Check if something is already mounted

```
mountpoint /dev
```

## Octal/numeric permissions

```shell
$ stat -c "%a %n" *
```

## Convert from timestamp to date

```term
$ date -d @1363292159.532
Thu Mar 14 21:15:59 CET 2013
```

## Find file corresponding to an inode

```term
$ find /sys -xdev -inum 30
/sys/devices/system/cpu/online
```

## Find with multiple conditions and execs

```bash
find . \( $COND1 -exec $CMD1 \) , \( $COND2 -exec $CMD2 \)
```

## Quotas

- For XFS systems, on `/etc/default/grub`

```bash
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX rootflags=uquota,pquota"
```

## Send to another process stdout

```bash
echo "test log1" >> /proc/$PID/fd/1
```

## nsswitch.conf

- files -> /etc/hosts
- myhostname, mymachines -> systemd-container
- resolve -> systemd-resolved
- dns -> /etc/resolv.conf

https://news.ycombinator.com/item?id=19439722

## Performance

1. Disable cores:

```bash
echo 0 > /sys/devices/system/cpu/cpu<n>/online
```

2. Check frequency range

```bash
cpupower frequency-info
```

2. Adjust frequencies and governor

```bash
# set governor
cpupower frequency-set -g <governor>

# max freq
cpupower frequency-set -u <freq>

# min freq
cpupower frequency-set -d <freq>
```

## Laptop not going to sleep

```ini
[Unit]
Description=Powertop tunings

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/sbin/powertop --auto-tune

[Install]
WantedBy=multi-user.target
```

## Run init daemon as user

```bash
runuser -s /bin/bash <user> -c "$*"
```

`setpriv` is better for dropping privileges since it doesn't use PAM and it doesn't spawn an extra process

```bash
setpriv --no-new-privs --reuid=1000 --regid=1000 --init-groups --reset-env "$@"
```

## ACLs

```bash
# default for new files
setfacl -d -R -m u:theUsersName:rwx

# actually set permission
setfacl -R -m u:theUsersName:rwx
```

# Tar pipe

```bash
( cd "$src" && tar -cf - .) | (cd "$dst" && tar xvf - )
```

## Routing tables

```bash
# show all rules
ip rule list

# create table
echo 200 custom >> /etc/iproute2/rt_tables

# from ip 
ip rule add from <source address> lookup <table name>

# from interface
ip rule add iif <interface> table isp2 priority 1000

# add default gw
ip route add default via 192.168.30.1 dev eth1 table custom

# show table 1
ip route show table 1
```