# VMWare player

## Port forwarding

`C:\ProgramData\VMware\vmnetnat.conf`

```conf
[incomingtcp]
80 = 192.168.80.128:80
```

and then

```cmd
net stop "VMWare NAT Service"
net start "VMWare NAT Service"
```

## Disable side-channel mitigations

on `*.vmx` file

```
ulm.disableMitigations="TRUE"
```

## Modify number of cores per socket

```
cpuid.coresPerSocket = "4"
```

## Shared folders not automounting
```fstab
vmhgfs-fuse  /mnt/hgfs  fuse  defaults,allow_other  0  0
```

