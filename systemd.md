# Systemd

## rc.local compatibility

```conf
[Unit]
Description=/etc/rc.local compatibility
After=network-online.target
Requires=network-online.target

[Service]
Type=oneshot
ExecStart=/bin/sh /etc/rc.local
ExecStop=/bin/sh  /etc/rc.local.shutdown
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

