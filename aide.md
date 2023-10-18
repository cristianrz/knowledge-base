# AIDE

## Generate initial database

```bash
aide --init
```

Database in `/etc/aide.conf`

To start using the db:

```
mv \
	/var/lib/aide/aide.db.new.gz \
	/var/lib/aide/aide.db.gz
```

## After system changes, update

```bash
aide --update
```

## Integrity check

```bash
aide --check
```

## Permissions issue

```bash
# Check if user _aide exists
grep _aide /etc/passwd

#  Check if group _aide exists
grep _aide /etc/group

# Check perms of /var/log/aide
sudo find /var/log/aide -exec ls -ld {} +

# Check perms of aide database
sudo find /var/lib/aide -exec ls -ld {} +
```