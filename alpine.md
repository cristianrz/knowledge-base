# Alpine Linux

## Service management

```bash
rc-update add sshd # enable

rc-status

rc-service sshd start
# or
/etc/init.d/sshd start
```

## Setup Alpine chroot

```bash
mkdir /var/alpine
cd /var/alpine

wget 'https://dl-cdn.alpinelinux.org/alpine/latest-stable/releases/aarch64/alpine-minirootfs-3.15.0-aarch64.tar.gz'

tar xzvf alpine-minirootfs-3.15.0-aarch64.tar.gz

mount -t proc /proc /var/alpine/proc/
mount -t sysfs /sys /var/alpine/sys/
mount --rbind /dev /var/apline/dev/
mount --rbind /run /var/apline/run/

cp /etc/resolv.conf etc/resolv.conf

chroot . ./bin/ash


apk add doas

echo 'permit :wheel' >> /etc/doas.d/doas.conf

adduser USER

addgroup USER wheel
```

## Alpine up

```bash
mount -t proc /proc /var/alpine/proc/
mount -t sysfs /sys /var/alpine/sys/
mount --rbind /dev /var/apline/dev/
mount --rbind /run /var/apline/run/

mount --bind /media/psf/code /var/alpine/mnt

chroot /var/alpine /usr/sbin/sshd
```

## SSH server inside chroot

```bash
ssh-keygen -A
sshd
```