# RSync

## Parameters

* `--delete`  delete no longer necessary files from dest dirs
* `-P`	      shows progress bar and allows partial uploads (Resume later)
* `-a`        syncs recursively and preserves symbolic links, special and device files, modification times, group, owner, and permissions. If you add this you do not need to add -r
* `-n`	      simulation mode
* `-v`	      verbose
* `-z`	      compresses the transfer
* `-u`        copies only if dest is older 

usually: `--delete -avzP`

A => B: `rsync -avPu --delete`
