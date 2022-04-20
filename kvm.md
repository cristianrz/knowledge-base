# KVM

## Iptables to isolate vm

```
iptables -D INPUT -i virbr0 -p udp -m udp --dport 53 -j ACCEPT
iptables -D INPUT -i virbr0 -p tcp -m tcp --dport 53 -j ACCEPT
iptables -D INPUT -i virbr0 -p udp -m udp --dport 67 -j ACCEPT
iptables -D INPUT -i virbr0 -p tcp -m tcp --dport 67 -j ACCEPT
iptables -I FORWARD -s 192.168.122.0/24 ! -d 192.168.1.0/24 -i virbr0 -j ACCEPT
iptables -D FORWARD -s 192.168.122.0/24 -i virbr0 -j ACCEPT
iptables -I INPUT -s 192.168.122.0/24 -i virbr0 -j DROP
```

## Allow inbound connections to VM but not outbound

```
-A INPUT -s 192.168.122.0/24 -i virbr0 -m conntrack --ctstate NEW -j DROP
-A FORWARD -s 192.168.122.0/24 ! -d 192.168.1.0/24 -i virbr0 -j ACCEPT
-A FORWARD -d 192.168.122.0/24 -o virbr0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -i virbr0 -o virbr0 -j ACCEPT
-A FORWARD -o virbr0 -j REJECT --reject-with icmp-port-unreachable
-A FORWARD -i virbr0 -j REJECT --reject-with icmp-port-unreachable
-A OUTPUT -o virbr0 -p udp -m udp --dport 68 -j ACCEPT
```

## Simplified ip tables

```
-P INPUT ACCEPT
-P FORWARD ACCEPT
-P OUTPUT ACCEPT
-A INPUT -s 192.168.122.0/24 -i virbr0 -m conntrack --ctstate NEW -j DROP
-A FORWARD -s 192.168.122.0/24 -d 192.168.1.0/24 -i virbr0 -m conntrack --ctstate NEW -j DROP
```

## Open VM ports to others

```
ssh -L 0.0.0.0:3000:localhost:4000 vmguest -p 5000 -l crz
```

* 3000: local port to open
* 4000: remote port to forward
* 5000: remote ssh port

## Iptables rules

* INPUT: destination IP is on the host, even it has multiple port with multiple subnet
* OUTPUT: source IP is from the host, either port
* FORWARD: Neither destination IP on the host nor source IP from the host

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

# GPU Passthrough

1. `intel_iommu=on iommu=pt` to `/etc/sysconfig/grub`
1. Rebuild grub `grub2-mkconfig -o /etc/grub2-efi.cfg`
1. Check `dmesg | grep -i -e DMAR -e IOMMU | grep enabled`
1. Add `pci-stub.ids=10de:11fa` to `/etc/sysconfig/grub`
1. Rebuild grub `grub2-mkconfig -o /etc/grub2-efi.cfg`
1. Check `lspci -nnk -d 10de:13c2 | grep stub`