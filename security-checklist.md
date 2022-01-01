## Checklist

### Router

* [ ] Change router username and password
* [ ] Change the router site to https in random port
* [ ] Disable NATPMP and UPNP
* [ ] Enable router MAC filtering
* [ ] Only allow router admin access via cable
* [ ] Port-scan router from outside
* [ ] Set number of DHCP leases to number of usual devices on router
* [ ] Use a dedicated LAN
* [ ] Use a long router password

### Permissions

* [ ] chmod 700 home dirs
* [ ] Dedicated account for sudo (admin)
* [ ] Disable su
* [ ] Enable either SELinux or AppArmor
* [ ] Mount data file systems with nodev, nosuid and noexec (home, tmp, dev/shm, var/tmp)
* [ ] Separate users
* [ ] Use specific account for virtualbox
* [ ] Use wayland instead of xorg or run xorg rootless

### Kernel

* [ ] Disable unprivileged user namespaces

## Others

* [ ] Disable bluetooth
* [ ] Disable hyperthreading
* [ ] Disable javascript
* [ ] Disable previews in file manager
* [ ] Don't dual boot, one system can infect the boot partition

## Processes

* Access router on incognito mode
* Don't access router from smartphone
* Don't browse with admin user
* Don't use ~/tmp ~/Download ~/Downloads ~/download ~/downloads ~/Desktop
* Open untrusted files in a disposable VM
* Port-scan router from outside from time to time
* Spoof MAC if travelling

## Notes

* VirtualBox hardening: https://www.whonix.org/wiki/Virtualization_Platform_Security#VirtualBox_Hardening
