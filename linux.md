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

