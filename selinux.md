# SELinux

## Start/Stop/Status

### Check policy in use

```bash
less /etc/selinux/config
```

OR

```term
sestatus
```

OR

```term
getenforce
```

### Enable SELinux

1. Edit `/etc/selinux/config` and set to `SELINUX=permissive`.
2. `reboot`
3. `touch /.autorelabel`
4. `setenforce 1`

### List all installed policies

```bash
semodule -l
```

## Labels

### Label format

Format is `user:role:type:level`. We mostly care about type.

### Change the context of a file manually

* `chcon` lasts until you restorecon
* `restorecon` gets it back to default

```
chcon -t httpd_content_t /path/to/file
```

OR

```
chcon --reference /something/good /something/bad
```

### Reset context of a file

```
restorecon -vR /var/www/html
```

Default contexts are in `/etc/selinux/targeted/contexts/file_contexts`

### Changing default context

Tell SELinux: from now on, consider that this directory's default label should be this.

```
semanage fcontext -a -t httpd_sys_content_t "/foo(/.*)?"
restorecon -vR /foo
```

or

```
semanage fcontext -a -e /good/path /new/path
restorecon -vR /foo
```

### Delete a context

```
semanage fcontext -d "/web(/.*)?"
```

## Booleans

### See all booleans

```
getsebool -a
```

### Check booleans already set

Check `/etc/selinux/targeted/modules/active/`

File does not do anything so don't edit manually.

### List all booleans with defaults

```bash
semanage boolean -l
```

### List all booleans

```bash
getsebool -a
```

### Set a boolean

```bash
setsebool allow_ftpd_use_nfs=1
```

### Set a boolean permanently

```bash
setsebool -P allow_ftpd_use_nfs=1
```

## Troubleshooting

### Troubleshoot server

```term
# dnf install setroubleshoot setroubleshoot-server
# reboot
```

1. Set as permissive (will log but not deny) `setenforce 0`
2. Run the application
3.  Check logs now

### See system logs

```
journalctl -b -0
```

### Silent denials

Log everything (takes a while to activate!)

```
semobule -DB
```

Back to normal

```
semodule -B
```

See dontaudit rules

```
sesearch --dontaudit -s smbd_t | grep squid
```

### Reset SELinux

```bash
setenforce 0
yum erase selinux-policy selinux-policy-targeted
mv /etc/selinux/targeted /etc/selinux/targeted.backup
yum install selinux-policy selinux-policy-targeted
touch /.autorelabel
reboot
```

### Logging

One of these locations

```txt
/var/log/avc.log
/var/log/audit/audit.log
/var/log/audit.log
```

Sometimes the kernel might suppress too many messages:

```
Mar 12 17:46:42 hpl kernel: [   14.453644] audit_printk_skb: 84 callbacks suppressed
```

### Show recent (10 min) denials

```bash
ausearch -m avc --start recent
```

### Show today's denials

```bash
ausearch -m avc --start today
```

### audit2allow

```bash
audit2allow -l -a -M local
```

* `-l`  reads denials since the last policy reload
* `-a` if auditd is running, `-d` if it is not running
* `-M` don't repeat names!!

You can view the rules to be added in the `local.te` file.

## Rules

```txt
allow user_t user_home_t:file { create read write unlink };
```

This rule states that the `user_t` type is allowed to create, read, write, and delete files with the `user_home_t` type.

## Categories

- Processes can only access files on:
	- the same category
	- subcategories
	- uncategorised
- Categories defined in `/etc/selinux/NAME/setrans.conf`
- Add category to file: `chcat +CompanyNDA myfile.doc`
- Remove category: `chcat -- -Engineering myfile.doc`
- Change category: `chcat Marketing,CompanyNDA myfile.doc`
- Run process with category: `runcon -l Engineering evolution`
