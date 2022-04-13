# GPU Passthrough

1. `intel_iommu=on iommu=pt` to `/etc/sysconfig/grub`
1. Rebuild grub `grub2-mkconfig -o /etc/grub2-efi.cfg`
1. Check `dmesg | grep -i -e DMAR -e IOMMU | grep enabled`
1. Add `pci-stub.ids=10de:11fa` to `/etc/sysconfig/grub`
1. Rebuild grub `grub2-mkconfig -o /etc/grub2-efi.cfg`
1. Check `lspci -nnk -d 10de:13c2 | grep stub`