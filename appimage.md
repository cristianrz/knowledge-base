# AppImage

## Create appimages

### Specify desktop file

```shell
./linuxdeploy-x86_64.AppImage \
	--appdir AppDir \
	--custom-apprun=[apprun] \
	--output appimage \
	-d [desktop] \
	-e [executable] \
	-i [icon]
```

### Auto-create desktop file and custom AppRun

```shell
linuxdeploy-x86_64.AppImage \
	--appdir AppDir \
	--create-desktop-file \
	--custom-apprun=[apprun] \
	--output appimage \
	-e [executable] \
	-i [icon]
```

### Just statically link a binary

```shell
linuxdeploy-x86_64.AppImage \
	--appdir AppDir \
	--create-desktop-file \
	--output appimage
	-e [executable] \
	-i [icon] 
```

## Desktop file

```desktop
[Desktop Entry]
Name=MyApp
Exec=myapp
Icon=myapp
Type=Application
Categories=Utility;
```

# AppRun

```bash
#!/bin/sh
SELF="$(readlink -f "$0")"
HERE="${SELF%/*}"
exec "$EXEC" "$@"
```
