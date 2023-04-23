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
