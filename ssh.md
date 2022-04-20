# SSH

## Local port forwarding

* Access something they can access but I can't access
* Bypass port block

From gate:

```shell
ssh -L [gate port]:[target ip]:[target port] [user]@[gate ip]
```

Example: access slashdot:80  through localhost:8080

```shell
ssh -L localhost:8080:dest.lan:80 gateway.lan
```

* <mark>Bind `localhost:8080`</mark>
* <mark>to `dest.lan:80` (from the POV of the gateway)</mark>
* <mark>through `gateway.lan`</mark>

To run in background `-N -f`

Check that `AllowTcpForwarding` is set to `yes` on remote SSH server

## Dynamic port forwarding

Used as a web proxy (socks).

From gate:

```shell
ssh -D [gate port] [user]@[target ip]
```

## Remote port forwarding

* Give someone else a port on my machine through SSH

From target:

 ```shell
 ssh -R [gate port]:localhost:[target port] [user]@[gate port]
 ```
 
 If you have access to a remote SSH server, you can set up a remote port forwarding as follows:

```
ssh -R 8080:dest.lan:3000 -N -f gateway.lan
```

* <mark>Bind `gateway:8080`</mark>
* <mark>to `dest.lan:3000` (from my POV)</mark>

Make sure `GatewayPorts` is set to `yes` in the remote SSH server.

## SFTP chroot
```sshd_config
Match User user
	ForceCommand sftp-server.exe
	ChrootDirectory "E:\"
	AuthorizedKeysFile "E:\.ssh\authorized_keys"
```
