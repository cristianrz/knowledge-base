# wireguard

## Connect to server

```bash
wg-quick up <server>
```

## server list

```bash
ls /etc/wireguard
```

## systemd-resolved with plain DNS

```sh
ln -s /usr/bin/resolvectl /usr/local/bin/resolvconf
```

```conf
PostUp = resolvectl dns %i 1.1.1.1; resolvectl domain %i '~'; resolvectl dnsovertls %i; resolvectl dnssec %i 0

PostDown = resolvectl revert %i
```