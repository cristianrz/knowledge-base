# WireGuard

## server list

```bash
ls /etc/wireguard
```

## Connect to server

```bash
# Enable and start
systemctl enable --now wg-quick@server

# Disable
systemctl stop wg-quick@server
```

## Using plain resolv.conf when WG is up

```conf
PostUp = ln -rsf /etc/resolv-wg.conf /etc/resolv.conf; systemctl stop systemd-resolved
PostDown = systemctl start systemd-resolved; ln -rsf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

Also add `systemd-resolved.service` as an `After` and `Wants` for `wg-quick@`.
