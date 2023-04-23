% nmap

# Ping sweep

```sh
nmap -sn
```

# Scripts

Scripts are in `/usr/share/nmap/scripts` and can be used with `nmap --script xxx`.

# Scanning

```bash
nmap -T4 -p- -A <ip>

nmap -T4 -p -sU <ip>
```