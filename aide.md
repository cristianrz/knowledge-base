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