# Bluetooth

## First time

```term
# apt install pulseaudio pulseaudio-module-bluetooth pavucontrol bluez-firmware
# service bluetooth restart
$ killall pulseaudio
```

## Connect from terminal

Start `bluetooth.service`.

Run

```term
$ bluetoothctl
```

to be greeted by its internal command prompt. Then enter:

```term
[bluetooth]# power on
[bluetooth]# agent on
[bluetooth]# default-agent
[bluetooth]# scan on
[bluetooth]# pair 00:1D:43:6D:03:26
[bluetooth]# connect 00:1D:43:6D:03:26
```

If you are getting a connection error `org.bluez.Error.Failed` retry by killing existing PulseAudio daemon first: 

```term
$ pulseaudio -k
[bluetooth]# connect 00:1D:43:6D:03:26
```

Finally, if you want to automatically connect to this device in the future:

```term
[bluetooth]# trust 00:1D:43:6D:03:26
```

## Headset connected but doesn't show in sound settings

```bash
systemctl --user restart pipewire{,-pulse}
```

