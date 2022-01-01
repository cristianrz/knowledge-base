# Borg

```sh
$ borg init --encryption=repokey /path/to/repo
$ borg create --progress -v /path/to/repo::Tuesday ~/src ~/Documents
$ borg list /path/to/repo
$ borg info /path/to/repo::12345
$ borg prune --list --show-rc --keep-daily 7 --keep-weekly 4 --keep-monthly 6
$ borg mount ssh://borg@backup.example.org:2222/path/to/repo /mnt/borg
$ borg extract ssh://borg@backup.example.org:2222/path/to/repo
```
 
