# Linux hardening
## /etc/modprobe.d
```sh
options nf_conntrack nf_conntrack_helper=0

install  af_802154      /bin/true
install  appletalk      /bin/true
install  atm            /bin/true
install  ax25           /bin/true
install  bluetooth      /bin/true
install  btusb          /bin/true
install  can            /bin/true
install  cifs           /bin/true
install  cramfs         /bin/true
install  dccp           /bin/true
install  decnet         /bin/true
install  econet         /bin/true
install  firewire-core  /bin/true
install  freevxfs       /bin/true
install  gfs2           /bin/true
install  hfs            /bin/true
install  hfsplus        /bin/true
install  ipx            /bin/true
install  jffs2          /bin/true
install  ksmbd          /bin/true
install  msr            /bin/true
install  n-hdlc         /bin/true
install  netrom         /bin/true
install  nfs            /bin/true
install  nfsv3          /bin/true
install  nfsv4          /bin/true
install  p8022          /bin/true
install  p8023          /bin/true
install  psnap          /bin/true
install  rds            /bin/true
install  rose           /bin/true
install  sctp           /bin/true
install  squashfs       /bin/true
install  thunderbolt    /bin/true
install  tipc           /bin/true
install  udf            /bin/true
#need it for camera
install  uvcvideo       /bin/true
install  vivid          /bin/true
install  x25            /bin/true
```
## /etc/sysctl.d
```sh
dev.tty.ldisc_autoload = 0
fs.protected_fifos = 2
fs.protected_hardlinks = 1
fs.protected_regular = 2
fs.protected_symlinks = 1
fs.suid_dumpable = 0
kernel.core_pattern = |/bin/false
kernel.dmesg_restrict = 1
kernel.kexec_load_disabled = 1
kernel.kptr_restrict = 2
kernel.perf_event_paranoid = 3
kernel.printk = 3 3 3 3
kernel.sysrq = 4
kernel.unprivileged_bpf_disabled = 1
kernel.unprivileged_userns_clone = 0
kernel.yama.ptrace_scope = 2
net.core.bpf_jit_harden = 2
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.default.log_martians = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.icmp_echo_ignore_all = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
# only if it's not a router
net.ipv4.ip_forward = 0
net.ipv4.tcp_dsack = 0
net.ipv4.tcp_fack = 0
net.ipv4.tcp_rfc1337 = 1
net.ipv4.tcp_sack = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_timestamps = 0
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.all.accept_source_route = 0
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.all.use_tempaddr = 2
net.ipv6.conf.default.accept_ra = 0
net.ipv6.conf.default.accept_redirects = 0
net.ipv6.conf.default.accept_source_route = 0
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.default.use_tempaddr = 2
vm.max_map_count = 1048576
vm.mmap_rnd_bits = 32
vm.mmap_rnd_compat_bits = 16
vm.swappiness = 1
vm.unprivileged_userfaultfd = 0
```

## /etc/default/grub

```sh
# already default
#GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX mitigations=auto"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX apparmor=1"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX security=apparmor"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX slab_nomerge"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX init_on_alloc=1"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX init_on_free=1"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX page_alloc.shuffle=1"
# default already activates if vulnerable
#GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX pti=on"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX vsyscall=none"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX debugfs=off"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX oops=panic"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX module.sig_enforce=1"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX lockdown=confidentiality"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX mce=0"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX loglevel=0"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX randomize_kstack_offset=on"

# default is auto, which already applies if needed
#GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX spectre_v2=on"

# default is already auto
#GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX spec_store_bypass_disable=on"

GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX tsx=auto"

# full,nosmt is better but my machine is slow. default is full
#GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX tsx_async_abort=full"

# full,nosmt is better but my machine is slow. default is full
#mds=full

# full,force would be better but disables SMT which is good for old PC's
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX l1tf=flush"

# my pc is too slow do disable smt
#GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX nosmt=force"

# default is already auto
#GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX kvm.nx_huge_pages=force"

GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX efi=disable_early_pci_dma"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX intel_iommu=on"
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX random.trust_cpu=off"
```

```sh
grub-mkconfig -o /boot/efi/EFI/$distro/grub.cfg
# or
update-grub
```

## /etc/fstab

```sh
# log gets noexec,nosuid,nodev
/dev/vgp/log    /var/log   ext4  defaults,noexec,nosuid,nodev           0  2
# tmp gets noexec,nosuid,nodev
/dev/vgp/tmp    /tmp       ext4  defaults,noexec,nosuid,nodev           0  2
# home gets noexec,nosuid,nodev
/dev/vgp/home   /home      ext4  defaults,nosuid,nodev                  0  2
# efi gets noexec,nosuid,nodev
UUID=B8E4-B711  /boot/efi  vfat  utf8,fmask=0077,noexec,nosuid,nodev    0  2
# tmp mount bind to /var/tmp
/tmp            /var/tmp   none  bind                                   0  0
# dev/shm gets noexec,nodev,nosuid
tmpfs          /dev/shm  tmpfs  defaults,noexec,nodev,nosuid,size=2G    0  0
```

## Boot permissions
```sh
sed -ri 's/chmod\s+[0-7][0-7][0-7]\s+\$\{grub_cfg\}\.new/chmod 400 ${grub_cfg}.new/' /usr/sbin/grub-mkconfig
sed -ri 's/ && ! grep "\^password" \$\{grub_cfg\}.new >\/dev\/null//' /usr/sbin/grub-mkconfig

chmod u-wx,go-rwx /boot/grub/grub.cfg
```

## Root password

```sh
# Single user mode is used for recovery when the system detects an issue during boot or by
# manual selection from the bootloader.
# 
# Requiring authentication in single user mode prevents an unauthorized user from
# rebooting the system into single user to gain root privileges without credentials.
passwd root
```

## /etc/security/limits.d
```sh
* hard core 0
```

## /etc/motd, /etc/issue, /etc/issue.net
```
rm /etc/motd
echo "Authorized users only. All activity may be monitored and reported." > /etc/issue
echo "Authorized users only. All activity may be monitored and reported." > /etc/issue.net
```

## /etc/gdm3/greeter.dconf-defaults

```conf
[org/gnome/login-screen]
banner-message-enable=true
banner-message-text='Authorized users only. All activity may be monitored and reported.'
disable-user-list=true
```

```sh
dpkg-reconfigure gdm3
```

## /etc/systemd/timesyncd.conf

```sh
NTP=0.debian.pool.ntp.org 1.debian.pool.ntp.org #Servers listed should be In Accordence With Local Policy
FallbackNTP=2.debian.pool.ntp.org 3.debian.pool.ntp.org #Servers listed should be In Accordence With Local Policy
RootDistanceMax=1 #should be In Accordence With Local Policy
```

```
systemctl start systemd-timesyncd.service
timedatectl set-ntp true
```

## Packages

```sh
systemctl stop avahi-daaemon.service
systemctl stop avahi-daemon.socket
apt --autoremove purge avahi-daemon

# Need for printing
#apt --autoremove purge cups

systemctl mask rsync

apt --autoremove purge telnet
```

## Firewalld

TODO


## /etc/systemd/journald.conf

```sh
Storage=persistent
```

## cron permissions
```sh
chmod og-rwx /etc/crontab
chmod og-rwx /etc/cron.hourly/
chmod og-rwx /etc/cron.daily/
chmod og-rwx /etc/cron.weekly/
chmod og-rwx /etc/cron.monthly/
chmod og-rwx /etc/cron.d/

rm /etc/cron.deny
touch /etc/cron.allow
chmod g-wx,o-rwx /etc/cron.allow

rm /etc/at.deny
touch /etc/at.allow
chmod g-wx,o-rwx /etc/at.allow
```

## doas is installed

```sh
type doas
```

## SSH config

TODO

## PAM

```sh
apt install libpam-pwquality
```

## /etc/security/pwquality.conf
```sh
minlen = 14
minclass = 4
```
## /etc/pam.d/common-password
```sh
password [success=1 default=ignore] pam_unix.so      sha512
password requisite                  pam_pwquality.so retry=3
password required                   pam_pwhistory.so remember=5
#password required                   pam_pwquality.so retry=2 minlen=16 difok=6 dcredit=-3 ucredit=-2 lcredit=-2 ocredit=-3 enforce_for_root
#password required pam_unix.so use_authtok sha512 shadow
```

## /etc/pam.d/common-auth
```sh
# pam_tally is deprecated
#auth required pam_tally2.so onerr=fail audit silent deny=5 unlock_time=900
```

## /etc/pam.d/common-account
```sh
account requisite pam_deny.so
# pam_tally is deprecated
#account required pam_tally2.so
```

## /etc/pam.d/common-session
```
session optional
```

## /etc/login.defs
```sh
PASS_MIN_DAYS 1
PASS_MAX_DAYS 365
PASS_WARN_AGE 7


UMASK 027
USERGROUPS_ENAB no
```

## Password expiry
```sh
useradd -D -f 30
```

## /etc/profile.d

```sh
TMOUT=900
readonly TMOUT
export TMOUT
```

## /etc/securetty

```
# empty
```

## restrict su

```
groupadd sugroup
```

### /etc/pam.d/su

```
auth required pam_wheel.so use_uid group=sugroup
```

## Find all SUID

```sh
df --local -P | awk '{if (NR!=1) print $6}' | xargs -I '{}' find '{}' -xdev -type f -perm -4000
```

## /etc/environment

```sh
PATH="/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin:$HOME/.local/bin"
EDITOR=rvim
```

## Remove permissions for sensitive folders

```sh
chmod g-rwx /boot /usr/src /usr/lib/modules /lib/modules /home/*
chmod o-rwx /boot /usr/src /usr/lib/modules /lib/modules /home/*
```

## Remove user from adm group

```sh
# certain logging daemons, such as systemd's `journalctl`, include the
# kernel logs which can be used to bypass the above `dmesg_restrict` protection.
# Removing the user from the `adm` group is often sufficient to revoke access to these
# logs:
gpasswd -d $user adm
```

## replace pulseaudio with pipewire
```sh
sudo apt install pipewire-audio-client-libraries libspa-0.2-bluetooth libspa-0.2-jack
sudo apt install wireplumber pipewire-media-session-
sudo cp /usr/share/doc/pipewire/examples/alsa.conf.d/99-pipewire-default.conf /etc/alsa/conf.d/
sudo cp /usr/share/doc/pipewire/examples/ld.so.conf.d/pipewire-jack-*.conf /etc/ld.so.conf.d/
systemctl mask pulseaudio
sudo apt remove pulseaudio-module-bluetooth pulseaudio
systemctl --user --now enable wireplumber.service
pactl info
```

## hardened malloc

```
mkdir -p ~/build
cd ~/build
git clone --depth 1 https://github.com/GrapheneOS/hardened_malloc.git
cd hardened_malloc
make
sudo cp out/libhardened_malloc.so /usr/lib
chown root:root /usr/lib/libhardened_malloc.so
chmod go-w /usr/lib/libhardened_malloc.so

echo /usr/lib/libhardened_malloc.so | sudo tee -a /etc/ld.so.preload
```

## /etc/machine-id, /var/lib/dbus/machine-id
```sh
# whonix machine id
b08dfa6083e7567a1921a715000001fb
````

## macchanger

```sh
apt install macchanger
```

```sh
#systemd unit file

```

## /etc/apt/sources.list
```sh
# changed to https except the security one which does not have https available
deb https://mirror.pulsant.com/sites/ubuntu-archive/ jammy main restricted

deb https://mirror.pulsant.com/sites/ubuntu-archive/ jammy-updates main restricted

deb https://mirror.pulsant.com/sites/ubuntu-archive/ jammy universe
deb https://mirror.pulsant.com/sites/ubuntu-archive/ jammy-updates universe

deb https://mirror.pulsant.com/sites/ubuntu-archive/ jammy multiverse
deb https://mirror.pulsant.com/sites/ubuntu-archive/ jammy-updates multiverse

deb https://mirror.pulsant.com/sites/ubuntu-archive/ jammy-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu jammy-security main restricted
deb http://security.ubuntu.com/ubuntu jammy-security universe
deb http://security.ubuntu.com/ubuntu jammy-security multiverse
```

## /etc/apt/apt.conf.d/40sandbox
```
APT::Sandbox::Seccomp "true";
```

## /etc/schroot/desktop/fstab
```sh
# fstab: static file system information for chroots.
# Note that the mount point will be prefixed by the chroot path
# (CHROOT_PATH)
#
# <file system>  <mount point>   <type>  <options>  <dump>  <pass>
/proc            /proc           none    rw,bind    0       0
/sys             /sys            none    rw,bind    0       0
/dev             /dev            none    rw,bind    0       0
/dev/pts         /dev/pts        none    rw,bind    0       0
/home            /home           none    rw,bind    0       0
/tmp             /tmp            none    rw,bind    0       0

# If you use gdm3, uncomment this line to allow Xauth to work
/var/run/gdm3    /var/run/gdm3   none    rw,bind    0       0
# For PulseAudio and other desktop-related things
/var/lib/dbus    /var/lib/dbus   none    rw,bind    0       0

# It may be desirable to have access to /run, especially if you wish
# to run additional services in the chroot.  However, note that this
# may potentially cause undesirable behaviour on upgrades, such as
# killing services on the host.
/run            /run             none    rw,bind    0       0
/run/lock       /run/lock        none    rw,bind    0       0
/dev/shm        /dev/shm         none    rw,bind    0       0
/run/shm        /run/shm         none    rw,bind    0       0 

/usr/local      /usr/local       none    rw,bind    0       0
```

## /etc/schroot/chroot.d
```sh
[arch]
description=chroot
type=directory
directory=/jail
users=user
root-groups=root
profile=desktop
```