% BTRFS

## Most common commands

```bash
# List snapshots
btrfs subvolume list -s <main vol path>

# List subvolumes
btrfs subvolume list -p <main vol path>

# Create subvolume
btrfs subvolume create </path/to/subvolume>

# Create snapshot
btrfs subvolume snapshot -r \
	<source path> \
	<snapshot dest_path>

# Delete snapshot
btrfs subvolume delete /mnt/data/@some_dir-snapshot-test

# Usage
btrfs filesystem usage <path>

# Balance (if disks are added/removed or FS is full)
btrfs balance start --bg /
btrfs balance status /
```

## Snapshots to an external disk

```bash
(
	set -eu

	btrfs_root=
	src=
	btrfs_dst="${btrfs_root}/root"
	date="$(date '+%Y-%m-%d')"
	
	# mirror to btrfs disk
	rsync -Pv -aHAXS --delete "${src}/" "$btrfs_dst"
	
	# create new snapshot of mirror
	btrfs subvolume snapshot -r \
		"$btrfs_dst" \
		"${btrfs_root}/.snapshots/root_ro_${date}"
	
	# check all snapshots
	btrfs subvolume list -s "$btrfs_root"
	
	# remove as necessary
	btrfs subvolume delete <path>
```

