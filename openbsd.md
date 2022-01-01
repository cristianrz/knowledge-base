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

## Tricks

* `sudo -k` to make sudo forget your password
