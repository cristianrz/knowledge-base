# zfs

## enable services

```sh
systemctl enable zfs-import-cache.service
systemctl enable zfs-mount.service

# enable auto for pool
zpool set cachefile=/etc/zfs/zpool.cache <pool>
```

## create pool

```
# create a single big partition on each drive that is the same size (leave a buffer)

ls -l /dev/disk/by-partlabel
ls -l /dev/disk/by-partuuid

zpool create -o ashift=12 -m <mount point> <poolname> raidz2 <disk1 ID> <disk2 ID> <disk3 ID> <disk4 ID>

# create dataset
zfs create <nameofzpool>/<nameofdataset>
# put quotas
zfs set quota=20G <nameofzpool>/<nameofdataset>/<directory>
```

## Check status

```sh
zpool status -v
```

## If it fails to automount

```sh
# not sure who is by-id
zpool import -d /dev/disk/by-id bigdata
```

## Move pool to another device

```sh
# if we don't do this we won't be able to import
zpool export <pool>
```

##  Change mount point

```sh
zfs set mountpoint=/foo/bar poolname
```

## Create snapshot

```sh
zfs snapshot zpool1/filestore@initial
```

## Scrub

```sh
zpool scrub <pool>
```

## Replace

```sh
zpool offline <pool> <old>
zpool replace <pool> <old> <new>
```






