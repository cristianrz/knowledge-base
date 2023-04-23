# rclone

## Sync from local to remote

```sh
rclone config

rclone sync -P "$LOCAL_PATH" "$REMOTE_DRIVE":"$REMOTE_PATH"
```

## Serve HTTP/WebDAV/SFTP/DLNA/FTP

```bash
rclone serve http -L  --addr 127.0.0.1:8888 home:
```
