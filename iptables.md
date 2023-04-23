# IPtables

## Iptables rules

* `INPUT`: destination IP is on the host, even it has multiple interfaces with multiple subnet
* `OUTPUT`: source IP is from the host, either interface
* `FORWARD`: Neither destination IP on the host nor source IP from the host

## Port forward

`iptables` is the fastest way because it's in the kernel

```sh
INTERFACE=eno1
PROTOCOL=tcp
SRC_PORT=9091
DST_IP=10.0.3.104
DST_PORT=9090

iptables --table nat \
	--insert          PREROUTING  \
	-p                "$PROTOCOL" \
	--dport           "$SRC_PORT" \
	-j                DNAT        \
	--to-destination  "${DST_IP}:${DST_PORT}" 

iptables \
	-I FORWARD \
	-p             "$PROTOCOL" \
	--in-interface "$INTERFACE" \
	--dport        "$SRC_PORT" \
	--destination  "$DST_IP" \
	-j ACCEPT
```

Dirty way

```
nc -l -k -p 8081 -c "nc 192.168.3.10 80"
```

or

```
mkfifo a
mkfifo b
nc 127.0.0.1 8000 < b > a &
nc -l 8001 < a > b &
printf "" > a
```

or

```
socat tcp-l:5050,fork,reuseaddr tcp:127.0.0.1:2020
```

or

```
ncat -k -l 8088 < svr1_to_svr2 | ncat 192.168.1.60 80 > svr1_to_svr2 &
```

## Isolate vm

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

## iptables madaidans

```bash
# Create the filter table
iptables -t filter

# Set the default policies
iptables -t filter -P INPUT DROP
iptables -t filter -P FORWARD DROP
iptables -t filter -P OUTPUT ACCEPT

# Create the TCP and UDP chains
iptables -t filter -N TCP
iptables -t filter -N UDP

# Allow established and related connections
iptables -t filter -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT

# Allow loopback connections
iptables -t filter -A INPUT -i lo -j ACCEPT

# Drop invalid connections
iptables -t filter -A INPUT -m conntrack --ctstate INVALID -j DROP

# Allow new TCP and UDP connections
iptables -t filter -A INPUT -p udp -m conntrack --ctstate NEW -j UDP
iptables -t filter -A INPUT -p tcp --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j TCP

# Reject all other incoming traffic
iptables -t filter -A INPUT -p udp -j REJECT --reject-with icmp-port-unreachable
iptables -t filter -A INPUT -p tcp -j REJECT --reject-with tcp-reset
iptables -t filter -A INPUT        -j REJECT --reject-with icmp-proto-unreachable
```

## iptables CIS

```bash
#!/bin/bash

# Flush IPtables rules
iptables -

# Ensure default deny firewall policy
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

# Ensure loopback traffic is configured
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT
iptables -A INPUT -s 127.0.0.0/8 -j DROP

# Ensure outbound and established connections are configured
iptables -A OUTPUT -p tcp -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -p udp -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -p icmp -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A INPUT  -p tcp -m state --state ESTABLISHED -j ACCEPT
iptables -A INPUT  -p udp -m state --state ESTABLISHED -j ACCEPT
iptables -A INPUT  -p icmp -m state --state ESTABLISHED -j ACCEPT

# Open inbound ssh(tcp port 22) connections
iptables -A INPUT -p tcp --dport 22 -m state --state NEW -j ACCEPT
```

## iptables Gentoo

```bash
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
 
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -p icmp --icmp-type 3 -j ACCEPT
iptables -A INPUT -p icmp --icmp-type 11 -j ACCEPT
iptables -A INPUT -p icmp --icmp-type 12 -j ACCEPT

# not sure what is this
#iptables -A INPUT -p tcp --syn --dport 113 -j REJECT --reject-with tcp-reset
```