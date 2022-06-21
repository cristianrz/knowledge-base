# OpenVPN

## `update-resolv-conf` without systemd

```sh
#!/bin/sh

case "$script_type" in
up)
	cp /etc/resolv.conf /etc/resolv.conf.bkp
	echo "nameserver 10.5.0.1" > /etc/resolv.conf
	;;
down)
	cat /etc/resolv.conf.bkp > /etc/resolv.conf
	rm /etc/resolv.conf.bkp
	;;
esac
```

	