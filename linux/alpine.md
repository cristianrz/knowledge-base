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

## Connect to internet (wired)

```sh
ifconfig eth0 up
udhcpc eth0
```

## Encrypted install

```sh
# install needed packages
setup-apkrepos
apk update
apk add gptfdisk cryptsetup lvm2 lvm2-dmeventd e2fsprogs util-linux dosfstools mkinitfs
/etc/init.d/dmeventd start

# partition drive
cfdisk /dev/nvme0n1
# trigger kernel re-read
hdparm -z /dev/hdc

# create EFI
mkfs.vfat /dev/nvme0n1p1

# create boot partition
mkfs.ext4 /dev/nvme0n1p2

# create LVM
cryptsetup luksFormat --type luks1 /dev/nvme0n1p3
cryptsetup luksOpen /dev/nvme0n1p3 cryptpart
pvcreate /dev/mapper/cryptpart
vgcreate vgp /dev/mapper/cryptpart

# setup thin pools <- we dont use thin pools anymore
#lvcreate -l 100%FREE -T vgp/thinpool
lvcreate -L XG -n name vgp
#lvcreate -V200G -T vgp/thinpool -n home
#lvcreate -V200G -T vgp/thinpool -n root
#lvcreate -V8G   -T vgp/thinpool -n tmp
#lvcreate -V8G   -T vgp/thinpool -n log
#lvcreate -V8G   -T vgp/thinpool -n swap

# format
mkfs.ext4 /dev/vgp/root
mkfs.ext4 /dev/vgp/home
mkfs.ext4 /dev/vgp/tmp
mkfs.ext4 /dev/vgp/log
mkswap /dev/vgp/swap

# setup chroot
mount /dev/vgp/root /mnt

mkdir -p /mnt/boot /mnt/var/log /mnt/tmp /mnt/home
mount /dev/nvme0n1p2 /mnt/boot

mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi
mount /dev/vgp/home  /mnt/home
mount /dev/vgp/log   /mnt/var/log
mount /dev/vgp/tmp   /mnt/tmp

# deploy alpine
setup-disk -m sys /mnt

vi /mnt/etc/mkinitfs/mkinitfs.conf
# add features="... cryptsetup"

mkinitfs -c /mnt/etc/mkinitfs/mkinitfs.conf -b /mnt/ $(ls /mnt/lib/modules/)

# chroot into it
mount -t proc /proc /mnt/proc
mount --rbind /dev /mnt/dev
mount --make-rslave /mnt/dev
mount --rbind /sys /mnt/sys
chroot /mnt
swapon /dev/vg0/swap

# install grub
apk add grub-efi efibootmgr
apk del syslinux

vi /etc/default/grub
# cryptdm=lvmcrypt
# GRUB_PRELOAD_MODULES="luks cryptodisk part_gpt lvm"
# GRUB_ENABLE_CRYPTODISK=y


```
