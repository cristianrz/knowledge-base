# Firewalld

## Get zones

```
firewall-cmd --list-all-zones
```

## Allow service

```
firewall-cmd --add-service=ssh --zone=public
```

## Assign interface to zone

```
firewall-cmd --zone=zone-name --change-interface=<interface-name>
```

