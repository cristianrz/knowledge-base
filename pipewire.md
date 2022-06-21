# Pipewire

## Ubuntu install

```sh
sudo apt install \
	pipewire-audio-client-libraries \
	pipewire-pulse \
	libspa-0.2-bluetooth

systemctl --user --now disable pulseaudio.service

systemctl --user --now mask pulseaudio.service

systemctl --user --now enable \
	pipewire \
	pipewire-pulse \
	pipewire-session-manager

sudo touch /usr/share/pipewire/media-session.d/with-pulseaudio

systemctl --user restart pipewire-session-manager
```



