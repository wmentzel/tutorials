### How to install Ubuntu (20.04) using full disc encryption (LUKS) alongside an already existing Windows installation

1. Boot Ubuntu Live CD
1. shrink windows partition
1. create two new partitions out of new unallocated space
  - boot (fat32/ext4, boot flag set, 256MB-768MB)
  - linux (exf4, as big as possible)


1. There should already be an ESP partition
  - esp (fat32, EFI system parition, 120MB)

1. Find name of partition for linux
```
lsblk
```
From now on, I will call the esp partition sda1, the boot partition sda2 and the linux partition sda3. This can be of course different for you, use what ``lsblk`` (list block devices) outputs.

1. Encrypt partition
```
sudo cryptsetup -v luksFormat /dev/sda3
```

1. Type "YES"

1. Type in a good passphrase

1. Open the encrypted partition. The unecrypted device will be /dev/mapper/systempartition
```
sudo cryptsetup luksOpen /dev/sda3 systempartition
```

1. Format the mounted partition
```
sudo mkfs.ext4 /dev/mapper/systempartition
```

1. Boot Ubuntu from USB stick/CD
1. During installation choose "Something else" when asked where to install Ubuntu

1. Assign mount points
  - Choose ext4 for /dev/mapper/systempartition and mount it at /
  - Choose ext4 for /dev/sda2 and mount it at /boot
  - ESP partition should already been assigned correctly

1. "continue testing"

1. Mount the linux system partition
```
sudo mount /dev/mapper/sda3 /mnt
```

1. Mount boot critical paritions
```
sudo mount /dev/sda2 /mnt/boot (has boot flag, ext4)
sudo mount /dev/sda1 /mnt/boot/efi (vfat)
```

1. Assign mount points for ```chroot```
```
cd /mnt
sudo mount --bind /sys sys
sudo mount --bind /dev dev
sudo mount --bind /run run
sudo mount --bind /proc proc
sudo chroot /mnt
```

1. Get the UUID of /dev/sda3 (Ubuntu partition)
```
sudo blkid (get UUID)
```
1. Add this line to /etc/crypttab
```
sdb2-enc UUID=jslfdjj-jksldjf-jljsdf-jljsdf	none	luks
```

1. ```update-initramfs -u```

1. Reboot

1. You should be presented with the GRUB boot menu

1. Choose ubuntu

1. Type in passphrase

1. Done
