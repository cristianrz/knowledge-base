# Samba

## Fixing delete config files

Find out what package installed the config file:

```shell
$ dpkg -S unity-greeter.conf
unity-greeter: /etc/lightdm/unity-greeter.conf
```

As you can see, the name of the package is unity-greeter.

If you deleted a directory, like /etc/pam.d, you can list every package that added to it by using the directory path:

```shell
$ dpkg -S /etc/pam.d
 login, sudo, libpam-runtime, cups-daemon, openssh-server, cron, policykit-1, at, samba-common, ppp, accountsservice, dovecot-core, passwd: /etc/pam.d
```

Run the following command, replacing <package-name> with the name of the package:

```shell
$ sudo apt install --reinstall -o Dpkg::Options::="--force-confask,confnew,confmiss" <package-name>
```

And for restoring the directory:

```shell
sudo apt install --reinstall -o Dpkg::Options::="--force-confask,confnew,confmiss" $(dpkg -S /etc/some/directory | sed 's/,//g; s/:.*//')
```

If everything worked as expected, you should get a message:

```shell
Configuration file `/etc/lightdm/unity-greeter.conf', does not exist on system. 
Installing new config file as you requested.
```

A Practical example when needing to reinstall all of the PulseAudio configuration files:

```shell
apt-cache pkgnames pulse |xargs -n 1 apt-get -o Dpkg::Options::="--force-confmiss" install --reinstall
```

## Ports not openning

Need  to disable the firewall. If you uninstalled a firewall that was active you need to reinstall it and isable it before uninstalling again.

## Security policies

"You can't access this shared folder because your organization's security policies block unauthenticated guest access. These policies help protect your PC from unsafe or malicious devices on the network."

Add these lines

```
[global]
   client min protocol = SMB3
   client max protocol = SMB3
   restrict anonymous = 2
   encrypt passwords = true
```

Remove

the `map to guest` option (as bad user)
any `guest ok` line in your smbd.conf