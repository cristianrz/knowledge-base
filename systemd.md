# Systemd

## rc.local compatibility

```conf
[Unit]
Description=/etc/rc.local compatibility
After=network-online.target
Requires=network-online.target

[Service]
Type=oneshot
ExecStart=/bin/sh /etc/rc.local
ExecStop=/bin/sh  /etc/rc.local.shutdown
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```


## List failed services

```sh
sudo systemctl list-units --failed
```

## Run a process with limited memory

```sh
systemd-run --user --pty --property MemoryHigh=2G firefox
```

## Systemd security

```ini
# This is a systemd service file for the example service

[Service]
# This service is allowed to bind to network ports with privileged port numbers
CapabilityBoundingSet=CAP_NET_BIND_SERVICE

# Protect the system and kernel resources from being modified by this service
ProtectSystem=strict
ProtectHome=true
ProtectKernelTunables=true
ProtectKernelModules=true
ProtectControlGroups=true
ProtectKernelLogs=true
ProtectHostname=true
ProtectClock=true

# Hide the /proc directory from this service
ProtectProc=invisible ProcSubset=pid

# Isolate this service's file system, users, devices, and IPC resources
PrivateTmp=true
PrivateUsers=true
PrivateDevices=true
PrivateIPC=true

# Deny write and execute access to memory for this service
MemoryDenyWriteExecute=true

# Disallow the service from gaining new privileges
NoNewPrivileges=true

# Lock the personality of this service to the current personality
LockPersonality=true

# Restrict the ability of this service to use real-time scheduling
RestrictRealtime=true

# Restrict the use of SUID and SGID bits by this service
RestrictSUIDSGID=true

# Restrict the address families that this service can use
RestrictAddressFamilies=AF_INET

# Restrict the namespace types that this service can create
RestrictNamespaces=true

# Allow only the specified system calls for this service
SystemCallFilter=write read openat close brk fstat lseek mmap mprotect munmap rt_sigaction rt_sigprocmask ioctl nanosleep select access execve getuid arch_prctl set_tid_address set_robust_list prlimit64 pread64 getrandom

# Specify that only native system calls are allowed for this service
SystemCallArchitectures=native

# Set the default file creation mask for this service to 0077
UMask=0077

# Deny access to any IP addresses for this service
IPAddressDeny=any

# Use the specified AppArmor profile for this service
AppArmorProfile=/etc/apparmor.d/usr.bin.exampl
```

## Fix systemd-boot

Mount with this, otherwise it won't pick it up properly

```sh
sudo cryptsetup luksOpen /dev/nvme0n1p3 cryptdata
```

After chroot'ing fix with

```sh
apt install --reinstall linux-image-generic linux-headers-generic
update-initramfs -c -k all
exit
sudo bootctl --path=/mnt/boot/efi install
```

Be sure that there is no `cryptsetup: WARNING: target 'cryptdata' not found in /etc/crypttab` entry when running the `update-initramfs -c -k all` . If there is:

1. check to be sure that `/etc/crypttab` does not have a string of characters after `cryptdata`.
2. If it does, remove the characters after `cryptdata` (`_U0qNZ`, in this example) so that the entry starts only with `cryptdata`.
3. Then, re-run the `update-initramfs -c -k all` command and continue with recovery.

## Seccomp filters

Can seccomp systemd services:

1. Run the command with `strace -e trace=all COMMAND | tee strace.log`
2. Find syscalls with `awk -F'(' '{print $1}' strace.log  | sort | uniq | xargs echo SystemCallFilter`
3. Add output from previous step to service file
4. Run service. If it runs, we're done. If it doesn't, continue. If it's getting too complicated use `@system-service`. Other types available in the [systemd code](https://github.com/systemd/systemd/blob/12e2b70f9b849e54018f147b8a11154cd5e2dcf6/src/shared/seccomp-util.c).
5. Run `journalctl -x  -f  _AUDIT_TYPE_NAME=SECCOMP` to see SECCOMP denies
6. Check `syscall=` field for the syscall number
7. Lookup on the [table](https://github.com/torvalds/linux/blob/v4.17/arch/x86/entry/syscalls/syscall_64.tbl#L11) in the 3rd column.
8. Add to `SystemCallFilter`
9. Go to step 4.

## User services

```bash
# enable
loginctl enable-linger $USER

# list lingering users
ls /var/lib/systemd/linger
```

## nspawn

```bash
(
	set -eux
	image="jellyfin/jellyfin"
	name="jellyfin"
	
	docker pull "$image"
	docker create --name "$name" "$image"
	trap 'docker rm "$name"' 2
	trap 'docker rm "$name"' 15

	mkdir -p "/var/lib/machines/${name}"

	docker export "$name" | ( cd "/var/lib/machines/${name}" && tar xvf - )
)

tmux new systemd-nspawn --as-pid2 -M jellyfin /jellyfin/jellyfin
```


```bash
MACHINE_NAME=vaultcloud
MACHINE_DIR="/var/lib/machines/${MACHINE_NAME}"

mkdir -p "${MACHINE_DIR}/usr/bin"

cd "$MACHINE_DIR"

wget 'https://cdimage.ubuntu.com/ubuntu-base/releases/22.04.1/release/ubuntu-base-22.04.1-base-amd64.tar.gz'

cp $(command -v busybox) "${MACHINE_DIR}/usr/bin"

cd "${MACHINE_DIR}/usr/bin"

ln -s busybox ls
ln -s busybox tar
ln -s busybox ash
ln -s busybox sh

systemd-nspawn -U -D "$MACHINE_DIR"

passwd root

apt install -y systemd-sysv

exit

systemd-nspawn -b -U -D "$MACHINE_DIR"
```

## Unencrypted journald remote logging

```bash
systemctl edit systemd-journal-remote.service
```

```ini
[Service]
ExecStart=
ExecStart=/usr/lib/systemd/systemd-journal-remote --listen-http=-3 --output=/var/log/journal/remote/
```

```bash
systemctl enable --now systemd-journal-remote.service
```

On `/etc/systemd/journal-upload.conf`

```ini
[Upload]
URL=http://192.168.0.100:19532
```

```bash
systemctl enable --now systemd-journal-upload.service
```

To check logs

```bash
journalctl --follow --directory=/var/log/journal/remote _HOSTNAME=raspberry-pi-3
```

https://web.archive.org/web/20221028110703/https://harryhodge.co.uk/posts/2021/07/centralised-logging-with-systemd-and-journald/

## Run job in the background

```bash
systemd-run \
	--remain-after-exit \
	-u "$unit_name" \
	bash -c "$command$"
```