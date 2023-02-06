# UEFI

## Remove entries

```
sudo apt-get install efibootmgr
```

Also, make sure your computer has been booted in UEFI mode (for that you need to set it up on the BIOS setup menu and boot from an UEFI compatible media) and that the kernel provides you with access to the EFI variables. Either compile your kernel with CONFIG_EFIVARS or load the module:

```
sudo modprobe efivars
```

If you need, take a look at the following link to know how to boot in UEFI mode and access your partition: Booting the Linux Kernel without a bootloader.

You can now run efibootmgr, assuming you have administrative privileges on the machine:

```
$ sudo efibootmgr
BootCurrent: 0000
BootOrder: 0000,0000,0000,0000,0000,0000
Boot0000* Linux-3.8.2
Boot0001* Grub
```

This menu, as you can see, lists two boot options: 0000 which is Linux-3.8.2 and 0001 which is Grub. In this example, the first entry directly loads the kernel (recent Linux kernels have the hability to be loaded directly by the UEFI without requiring a bootloader, see Booting the Linux Kernel without a bootloader) and the second entry loads Grub (to later load a kernel).

Imagine that, because you are able to load the kernel directly, you don't need Grub anymore. Therefore, you also don't need an extra entry in the menu. So, remove it:

```
sudo efibootmgr -b 1 -B
```

This means that efibootmgr will select for alteration the entry 1 (option -b 1) and apply the removal operation (-B).

