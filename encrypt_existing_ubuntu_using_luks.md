### How to secure an existing Ubuntu (20.04) installation using full disc encryption (LUKS)

1. Backup complete drive using CloneZilla (for worst case scenario)

1. Using GParted
- Shrink linux partition by 512MB.
- Create unencrypted boot partition using GParted (ext4, 512MB).

1. There should already be an ESP (EFI system partition)
  - filesystem: fat32
  - size: ~175MB
  - flags: boot, esp

1. Find out what is what using ```lsblk```

  **This will be the mapping for further steps. The names are most likely different, depending on your system: ESP = sda1, linux partition = sda2, boot partition = sda3, backup partition sdb1**


1. Create directories for mounting
```
sudo mkdir /mnt/boot
sudo mkdir /mnt/linux
sudo mkdir /mnt/backup
```

1. Mount drives
```
sudo mount /dev/sda3 /mnt/boot
sudo mount /dev/sda2 /mnt/linux -o ro, noload
sudo mount /dev/sdb1 /mnt/backup
```

1. Copy contents of /boot to new partition (Note the trailing "/")
```
sudo rsync -axHAWXS --numeric-ids --info=progress2 /boot/ /mnt/boot/
```

1. Remove /mnt/boot directory content, because it resides on its own partition now.
```
sudo rm -rf /mnt/boot/*
```

1. Make linux mount the boot partition at startup, by adding the following line to /etc/fstab
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
sudo umount /dev/sda2
```

1. Encrypt linux partition
```
sudo cryptsetup -v luksFormat /dev/sda2
```

1. Open encrypted partition. After that there will be a device /dev/mapper/systempartition
```
sudo cryptsetup luksOpen /dev/sda2 systempartition
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
systempartition UUID=XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX	none	luks
```

1. chroot

1. sudo initramfs -u

1. update-grub

1. restart

# Sources:

- [Copy entire file system hierarchy from one drive to another](https://superuser.com/questions/307541/copy-entire-file-system-hierarchy-from-one-drive-to-another)
- [How do I run update-grub from a LiveCD?](https://askubuntu.com/questions/145241/how-do-i-run-update-grub-from-a-livecd)
- [How to get grub to boot from a newly encrypted partition](https://askubuntu.com/questions/1082131/how-to-get-grub-to-boot-from-a-newly-encrypted-partition)
- [Dual Boot Ubuntu with LUKS alongside Windows](https://www.youtube.com/watch?v=IfJ6HkMxgEY&feature=youtu.be)
- [How to mount a hard disk as read-only from the terminal](https://askubuntu.com/questions/296331/how-to-mount-a-hard-disk-as-read-only-from-the-terminal)
- [Recursively compare two directories with diff -r without output on broken links](https://unix.stackexchange.com/questions/49496/recursively-compare-two-directories-with-diff-r-without-output-on-broken-links)
