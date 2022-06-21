	# SELinux

## Reset SELinux

```bash
setenforce 0
yum erase selinux-policy selinux-policy-targeted
mv /etc/selinux/targeted /etc/selinux/targeted.backup
yum install selinux-policy selinux-policy-targeted
touch /.autorelabel
reboot
```

## Check policy in use

```bash
less /etc/selinux/config
```

OR

```term
sestatus
```

OR

```
getenforce
```

## Label format

Format is `user:role:type:level`. We mostly care about type.

## See all booleans

```
getsebool -a
```

## Troubleshooting

```term
# dnf install setroubleshoot setroubleshoot-server
# reboot
```

1. Set as permissive (will log but not deny) `setenforce 0`
2. Run the application
3.  Check logs now

## See system logs

```
journalctl -b -0
```

## Check booleans already set

Check `/etc/selinux/targeted/modules/active/`

File does not do anything so don't edit manually.

## Change the context of a file manually

* `chcon` lasts until you restorecon
* `restorecon` gets it back to default

```
chcon -t httpd_content_t /path/to/file
```

OR

```
chcon --reference /something/good /something/bad
```

## Reset context of a file

```
restorecon -vR /var/www/html
```

Default contexts are in `/etc/selinux/targeted/contexts/file_contexts`

## Changing default context

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

## Enable SELinux

1. Edit `/etc/selinux/config` and set to `SELINUX=permissive`.
2. `reboot`
3. `touch /.autorelabel`
4. `setenforce 1`

## Delete a context

```
semanage fcontext -d "/web(/.*)?"
```


## Silent denials

Log everything

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

## List all booleans with defaults

```
semanage boolean -l
```
