# nmap

## Ping sweep

```sh
nmap -sn 192.168.1.0/24 2>&1 | grep -B 1 up | grep report
```

## Scripts

Scripts are in `/usr/share/nmap/scripts` and can be used with `nmap --script xxx`.

## Scanning

```bash
nmap -T4 -p- -A <ip>

nmap -T4 -p -sU <ip>
```