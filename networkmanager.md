# NetworkManager

## Interface is fully unmanaged (Ubuntu)

Ubuntu installs a config file that sets most devices unmanaged `/usr/lib/NetworkManager/conf.d/10-globally-managed-devices.conf`

To disable this, You can create a blank file with the same name in /etc:

```sh
sudo touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf
sudo systemctl restart NetworkManager
```

