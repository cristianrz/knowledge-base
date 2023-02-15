# Iptables

## Port forward

`iptables` is the fastest way because it's in the kernel

```sh
PROTOCOL=tcp
SRC_PORT=8022
DST_IP=192.168.122.145
DST_PORT=22

iptables -t nat \
	-I PREROUTING \
	-p "$PROTOCOL" \
	--dport "$SRC_PORT" \
	-j DNAT \
	--to-destination "${DST_IP}:${DST_PORT}" 

iptables \
	-I FORWARD \
	-p "$PROTOCOL" \
	-i "$INTERFACE" \
	--dport "$SRC_PORT" \
	-d "$DST_IP" \
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
