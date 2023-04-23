# cockpit

## Disable SSL and unsafe TLS

Create file /etc/systemd/system/cockpit.service.d/ssl.conf containing:

```bash
[Service]
Environment=G_TLS_GNUTLS_PRIORITY=NORMAL:-VERS-SSL3.0:-VERS-TLS1.0:-VERS-TLS1.1
```

