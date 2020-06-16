### How to migrate Ubuntu (18.04) from BIOS (legacy mode) to UEFI

1. Change your system firmware from (legacy) BIOS to EFI mode

1. Boot Ubuntu Live CD

1. Create ESP (EFI system partition) using GParted
  - filesystem: fat32
  - size: 175MB
  - flags: boot, esp

1. Find out UUID of newly created esp
```
blkid
```

1. Mount linux system partitions
```
sudo mount /dev/sda2 /mnt
```

1. Mount it on startup. Add the following line to ```/mnt/etc/fstab```
```
UUID=688A-E900  /boot/efi       vfat    umask=0077      0       1
```

1. Assign mount points for ```chroot```
```
sudo mkdir /mnt/boot/efi
sudo mount /dev/sda2 /mnt/boot/efi (vfat)
cd /mnt
sudo mount --bind /sys sys
sudo mount --bind /dev dev
sudo mount --bind /run run
sudo mount --bind /proc proc
sudo chroot /mnt
```

1. Install GRUB for EFI
```
apt install grub-efi
```

1. Reinstall grub
```grub-install --target=x86_64-efi /dev/sdb```

1. Reboot
