# Firewalld

## Get zones

```
firewall-cmd --list-all-zones
```

## Allow service

```
firewall-cmd --add-service=ssh --zone=public
```

## New zone

```
firewall-cmd --permanent --new-zone=public
```

## Assign interface to zone

```
firewall-cmd --zone=zone-name --change-interface=<interface-name>
```

## Set log level

```bash
sudo firewall-cmd --set-log-denied=[all/off/denied]
```
## Forward port

```bash
sudo firewall-cmd --permanent --zone=users \
	--add-forward-port=port=80:proto=tcp:toport=5080
```

## Wrong zone is applied

Priorities are alphabetical.

## Add service

```bash
firewall-cmd --permanent --new-service=dlna
firewall-cmd --permanent --service=dlna --add-port=8096/tcp
firewall-cmd --permanent --service=dlna --add-port=1900/udp
firewall-cmd --permanent --service=dlna --add-port=7359/udp
```

## Direct rules

```bash
firewall-cmd \
	--direct \
	--add-rule ipv4 \
	filter IN_public_allow 0 -m tcp -p tcp --dport 666 -j ACCEPT
```

```bash
firewall-cmd --direct --get-rules ipv4 filter IN_public_allow

firewall-cmd --permanent  --direct --get-all-rules
```

## Rich rules

```bash
firewall-cmd --permanent --policy=lan-to-external --add-rich-rule='
  rule family="ipv4"
  source address=192.168.0.0/24
  port protocol="tcp" port="443" reject
'

firewall-cmd --add-rich-rule='
  rule family=ipv4
  source address=192.168.0.0/24
  service name=dns
  reject
'

firewall-cmd --add-rich-rule='
  rule family=ipv4
  source address=192.168.0.0/24
  forward-port port=number_or_range protocol=protocol /
            to-port=number_or_range to-addr=address
'
```