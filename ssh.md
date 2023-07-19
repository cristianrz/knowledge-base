# OpenSSH

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

## Use existing agent

```bash
# find ssh sockets
ssh_sock="$(find /tmp/ssh*/agent* -print -quit)" 

if [ -n "$ssh_sock" ] && [ -f "$HOME/.agent" ]; then
	# an agent is running and we saved the config
	. "$HOME/.agent"  
else
	# agent is not running or we didn't save config
	eval "$(ssh-agent | tee "$HOME/.agent")"  
	ssh-add "$HOME/.ssh/id_ed25519"  
fi

# cleanup
unset ssh_sock
```

## Kill frozen session

Press:
- Enter
- ~ (tilde)
- . (period)

## Install pubkeys from github

```bash
curl https://github.com/cristianrz.keys >> "$HOME/.ssh/authorized_keys"
sort -u "$HOME/.ssh/authorized_keys" > "$HOME/.ssh/authorized_keys.bak"
mv "$HOME/.ssh/authorized_keys.bak" "$HOME/.ssh/authorized_keys"
chmod 0700 "$HOME/.ssh/authorized_keys"
```

## Encrypt/decrypt with SSH keys

- Convert the public key into `pem` format

```bash
ssh-keygen -f path/to/id_rsa.pub -e -m pem > ~/id_rsa.pub.pem
```

- Using the public `pem` file to encrypt a string

```bash
cat ~/some_file.txt |
	openssl rsautl -encrypt -pubin -inkey ~/id_rsa.pub.pem > ~/encrypted.txt
```

- To decrypt, you'll need the private key

```bash
cat ~/encrypted.txt |
	openssl rsautl -decrypt -inkey path/to/id_rsa > ~/decrypted.txt
```
