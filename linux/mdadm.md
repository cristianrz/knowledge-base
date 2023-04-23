# mdadm

## Create RAID

```sh
mdadm --create /dev/md0 --level=10 --raid-devices=4 /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1

mkfs -t ext4 /dev/md0

mdadm --detail --scan >> /etc/mdadm.conf
```

## Check status

```sh
mdadm --detail /dev/md0
```

## Keep RAID

```sh
mdadm --detail --scan >> /etc/mdadm.conf
```

## Replace disk

```sh
# mark disk as faulty
mdadm --manage /dev/md0 --fail /dev/sdd
mdadm --detail /dev/md0

# remove disk
mdadm --manage /dev/md0 --remove /dev/sdd

# add new disk
mdadm --manage /dev/md0 --add /dev/sdf

# check if finished
mdadm --detail /dev/md0 | grep spare
```

## Scrub

```sh
echo check > /sys/block/md0/md/sync_action

# check status
cat /proc/mdstat

# check result
cat /sys/block/md0/md/mismatch_cnt
```







