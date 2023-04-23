# ClamAV

## Install

```sh
sudo dnf install clamav clamav-update
```

## Scan

```term
# clamscan -r --log=/tmp/log --move=/tmp/quarantine /mnt/pool
```
