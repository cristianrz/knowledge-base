# OpenBSD

## Websites

* undeadly.org: news aggregator
* daemonforums

## Documentation

* apropos: word
* whatis: exact word

## Good practices

* Separate directories in partitions
* Read and write partitions separate
* Avoid slower drives
* Least privilege approach (create new users?)
* Run daemons under a user with:
	* no home directory,
	* no login shell and
	* hashed password of \*
* Unprivileged users start with _
* Don't give sudo access to everything
* Disable hyperthreading

## Updating

* install security patches with `syspatch`
* install ports updates with `pkg_add -u`

## Update to next release

```sh
sysupgrade

reboot

syspatch

pkg_add -Uu

sysmerge -d
```

## Tricks

* `sudo -k` to make sudo forget your password

## Ports

```sh
# find a package
pkg_info -Q unzip

# install a package
pkg_add -v rsync

# remove a package
pkg_delete -v screen

# autoremove
pkg_delete -v -a
```

## Partitioning

```sh
disklabel -E wd0

# equivalent to mkfs
newfs sd2a

# equivalent to blkid
sysctl hw.disknames

# view partition table
fdisk sd0

# initialise blank disk
fdisk -iy sd0
```

## Disklabel

```sh
> a a                  # add parition labeled as 'a'
offset: [64]           # just click Enter key
size: [62910476] 1.0G  # set partition size
FS type: [4.2BSD]      # just click Enter key
mount point: [none] /  # set mount point

> a b                  # add parition labeled as 'b'
offset: [2104512]      # just click Enter key
size: [60806028] 1.1G
FS type: [swap]        # just click Enter key

# skip 'c'

> a d                  # add parition labeled as 'd'

...

> p                    # check partitions in the end
> q                    # save changes and exit
```