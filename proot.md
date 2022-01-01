# PRoot

## Running firefox

Need:

* `-b /run/shm -b /proc -b /dev`
* Install some fonts


# Don't know

# Needed


# Not needed

-b /etc/mtab -b /etc/netgroup -b /etc/networks -b /etc/passwd -b /etc/group -b /etc/nsswitch.conf -b /etc/resolv.conf -b /etc/localtime -b /dev/ -b /sys/ -b /tmp/ -b /run/ -b /var/run/dbus/system_bus_socket -b $HOME



All 

-b /etc/mtab -b /etc/netgroup -b /etc/networks -b /etc/passwd -b /etc/group -b /etc/nsswitch.conf -b /etc/resolv.conf -b /etc/localtime -b /dev/ -b /sys/ -b /tmp/ -b /run/ -b /var/run/dbus/system_bus_socket -b $HOME -b /proc



 -S works fine
* no /dev -> segfault
* no /proc -> cannot find profile
* no /run/shm -> segfault

* no /sys OK!
* no /tmp OK!



