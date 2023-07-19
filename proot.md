# PRoot

## Running firefox

Need:

* `-b /run/shm -b /proc -b /dev`
* Install some fonts

## The key(s) in the keyring XXX.gpg are ignored as the file is not readable by user '' executing apt-key.

```bash
sed -i "s/deb/deb [trusted=yes]/g" /etc/apt/sources.list
```

# Not needed

```bash
-b /etc/mtab -b /etc/netgroup -b /etc/networks -b /etc/passwd -b /etc/group -b /etc/nsswitch.conf -b /etc/resolv.conf -b /etc/localtime -b /dev/ -b /sys/ -b /tmp/ -b /run/ -b /var/run/dbus/system_bus_socket -b $HOME
```

All 

```bash
-b /etc/mtab -b /etc/netgroup -b /etc/networks -b /etc/passwd -b /etc/group -b /etc/nsswitch.conf -b /etc/resolv.conf -b /etc/localtime -b /dev/ -b /sys/ -b /tmp/ -b /run/ -b /var/run/dbus/system_bus_socket -b $HOME -b /proc
```

 -S works fine
* no /dev -> segfault
* no /proc -> cannot find profile
* no /run/shm -> segfault

* no /sys OK!
* no /tmp OK!
