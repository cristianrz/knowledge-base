# Proxmox

## Use xterm.js

1. Add a virtual serial port to the VM using PVE Web GUI and restart the VM

2. Enable and start the virtual serial port on VM, change tty number as needed (Reference: https://askubuntu.com/a/621209/838946)
```terminal
$ sudo systemctl enable serial-getty@ttyS0.service
$ sudo systemctl start serial-getty@ttyS0.service
```

3. Done! You can now select xterm.js in the PVE Web GUI