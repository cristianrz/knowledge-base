# restic

## Cheatsheet

```bash
# Initialise repo
restic -r sftp:restic-backup-host:/srv/restic-repo init

# Backup directory
restic -r /srv/restic-repo --verbose backup --tag projectX ~/work

# diff between repos
restic -r /srv/restic-repo diff 5845b002 2ab627a6

# List snapshots
restic -r /srv/restic-repo snapshots

# Before migration to a new repo
restic -r /srv/restic-repo-copy init --from-repo /srv/restic-repo --copy-chunker-params

# Migrate snapshots
restic -r /srv/restic-repo-copy copy --from-repo /srv/restic-repo

# Remove a file from all snaps
restic -r /srv/restic-repo rewrite --exclude secret-file

# Remove a file from a specific snap
restic -r /srv/restic-repo rewrite --exclude secret-file 6160ddb2

# Check for errors on index
restic -r /srv/restic-repo check

# Check for actual integrity
restic -r /srv/restic-repo check --read-data

# Check a random subset
restic -r /srv/restic-repo check --read-data-subset=2.5%

# Restore a snapshot
restic -r /srv/restic-repo restore 79766175 --target /tmp/restore-work

# Restore a specific directory
restic -r /srv/restic-repo restore latest --target /tmp/restore-art --path "/home/art" --host luigi

# Mount 
restic -r /srv/restic-repo mount /mnt/restic

#  Remove a snapshot
restic -r /srv/restic-repo forget bdbd3439
restic -r /srv/restic-repo prune

# Remove according to policy dry running first
restic forget \
	--dry-run         \
	--group-by ''     \
	--keep-hourly  1  \
	--keep-daily   3  \
	--keep-weekly  3  \
	--keep-monthly 3  \
	--keep-yearly 10  \
	
```