# Fedora

## Release upgrade

```bash
sudo dnf upgrade --refresh

sudo dnf install dnf-plugin-system-upgrade

sudo dnf system-upgrade download --releasever=37

sudo dnf system-upgrade reboot
```

## Disable zram

```bash
sudo dnf remove zram-generator-defaults
```

## Hibernate not working

```bash
cat <<-EOF | tee /etc/dracut.conf.d/resume.conf
add_dracutmodules+=" resume "
EOF

dracut -f
```
