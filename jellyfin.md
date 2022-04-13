# Jellyfin

## Start with Docker-like directories

```bash
# web client path, installed by the jellyfin-web package
JELLYFIN_WEB_OPT="--webdir=/usr/share/jellyfin/web"

# Restart script for in-app server control
JELLYFIN_RESTART_OPT="--restartpath=/usr/lib/jellyfin/restart.sh"

# ffmpeg binary paths, overriding the system values
JELLYFIN_FFMPEG_OPT="--ffmpeg=/usr/lib/jellyfin-ffmpeg/ffmpeg"

JELLYFIN_USER="jellyfin"

export \
    JELLYFIN_DATA_DIR="/config/data" \
    JELLYFIN_CONFIG_DIR="/config" \
    JELLYFIN_LOG_DIR="/config/log" \
    JELLYFIN_CACHE_DIR="/config/cache"

/usr/bin/jellyfin ${JELLYFIN_WEB_OPT} ${JELLYFIN_RESTART_OPT} ${JELLYFIN_FFMPEG_OPT} ${JELLYFIN_SERVICE_OPT} ${JELLYFIN_NOWEBAPP_OPT}
```