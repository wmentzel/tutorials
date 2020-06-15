### How to secure an existing Ubuntu (20.04) installation using full disc encryption (LUKS) alongside Windows installation (UEFI/BIOS)

1. Backup complete drive using CloneZilla (for worst case scenario)
1. Backup linux partition using rsync

1. Create unencrypted boot partition using GParted. ESP should already exist (ignore this for BIOS mode).

1. Find its name using ```lsblk``` (let's call it ***/dev/sdaZ*** from now on)

1. Create directories for mounting
```
sudo mkdir /mnt/boot
sudo mkdir /mnt/linux
sudo mkdir /mnt/backup
```

1. Mount drives
```
sudo mount /dev/sdZ /mnt/boot
sudo mount /dev/sdX /mnt/linux -o ro, noload
sudo mount /dev/sdY /mnt/backup
```

1. Copy contents of /boot to new partition (Note the trailing "/")
```
sudo rsync -axHAWXS --numeric-ids --info=progress2 /boot/ /mnt/boot/
```

1. Remove /mnt/boot directory content, we have it on its own partition now
```
sudo rm -rf /mnt/boot/*
```

1. Make Linux mount the boot partition at startup, by adding the following line to /etc/fstab
```
UUID=XXX-XXX-XXX /boot ext4 defaults 0 2
```

1. Copy linux system to backup drive
```
sudo rsync -axHAWXS --numeric-ids --info=progress2 /mnt/linux/ /mnt/backup/
```

1. (optional sanity check, should only return warning for device files etc.)
```
sudo diff -r --no-dereference /mnt/linux/ /mnt/backup/
```

1. Unmount linux partition
```
sudo umount /dev/sdaX
```

1. Encrypt linux partition
```
sudo cryptsetup -v luksFormat /dev/sdX
```

1. Open encrypted partition. After that there will be a device /dev/mapper/systempartition
```
sudo cryptsetup luksOpen /dev/sdaX systempartition
```

1. Create filesystem
```
sudo make.ext4 /dev/mapper/systempartition
```

1. Mount the new drive
```
sudo mount /dev/mapper/systempartition /mnt/linux
```

1. Copy linux system back
```
sudo rsync -axHAWXS --numeric-ids --info=progress2 /mnt/backup/ /dev/linux/
```

1. Find out UUID of encrypted device using blkid (block device ids)

1. Add this line to /etc/crypttab
```
systempartition UUID=jslfdjj-jksldjf-jljsdf-jljsdf	none	luks
```

1. chroot

1. sudo initramfs -u

1. update-grub

1. restart

# Sources:

- How to mount a hard disk as read-only from terminal
- Recursively cmpare two directories with diff -r
