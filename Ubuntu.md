# Ubuntu

## Upgrade only security updates

```bash
apt-get -s dist-upgrade |
	grep "^Inst" | 
    grep -i securi |
    awk -F " " {'print $2'} | 
    xargs apt-get install
```
