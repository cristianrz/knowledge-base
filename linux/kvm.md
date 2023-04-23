# KVM



## Open VM ports to others

```
ssh -L 0.0.0.0:3000:localhost:4000 vmguest -p 5000 -l crz
```

* 3000: local port to open
* 4000: remote port to forward
* 5000: remote ssh port

## DCHP 

Release IP

```
sudo dhclient -r
```

ask DHCP

```
sudo dhclient
```

## Manual IP config

```
ifconfig eth0 192.168.1.5 netmask 255.255.255.0 up
route add default gw 192.168.1.1
echo "nameserver 1.1.1.1" > /etc/resolv.conf
```

## GPU Passthrough

1. `intel_iommu=on iommu=pt` to `/etc/sysconfig/grub`
2. Rebuild grub `grub2-mkconfig -o /etc/grub2-efi.cfg`
3. Check `dmesg | grep -i -e DMAR -e IOMMU | grep enabled`
4. Add `pci-stub.ids=10de:11fa` to `/etc/sysconfig/grub`
4. Rebuild grub `grub2-mkconfig -o /etc/grub2-efi.cfg`
5. Check `lspci -nnk -d 10de:13c2 | grep stub`

## Trim qcow2

***[This](https://pve.proxmox.com/wiki/Shrink_Qcow2_Disk_Files) is preferrable.***


## Cloud init

```bash
virt-install --name garlic \
	--memory 4096 --noreboot \
	--os-variant detect=on,name=ubuntu22.04 \
	--cloud-init user-data="/home/superman/cloudinit-user-data.yaml" \
	--disk=size=30,backing_store="/srv/images/jammy-server-cloudimg-amd64-disk-kvm.img"
```

## Port forward

On `/etc/libvirt/hooks/qemu`:

```bash
#!/bin/bash

# IMPORTANT: Change the "VM NAME" string to match your actual VM Name.
# In order to create rules to other VMs, just duplicate the below block and configure
# it accordingly.
if [ "${1}" = "VM NAME" ]; then

   # Update the following variables to fit your setup
   GUEST_IP=
   GUEST_PORT=
   HOST_PORT=

   if [ "${2}" = "stopped" ] || [ "${2}" = "reconnect" ]; then
    /sbin/iptables -D FORWARD -o virbr0 -p tcp -d $GUEST_IP --dport $GUEST_PORT -j ACCEPT
    /sbin/iptables -t nat -D PREROUTING -p tcp --dport $HOST_PORT -j DNAT --to $GUEST_IP:$GUEST_PORT
   fi
   if [ "${2}" = "start" ] || [ "${2}" = "reconnect" ]; then
    /sbin/iptables -I FORWARD -o virbr0 -p tcp -d $GUEST_IP --dport $GUEST_PORT -j ACCEPT
    /sbin/iptables -t nat -I PREROUTING -p tcp --dport $HOST_PORT -j DNAT --to $GUEST_IP:$GUEST_PORT
   fi
fi
```
