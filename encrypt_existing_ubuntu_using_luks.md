### How to secure an existing Ubuntu (20.04) installation using full disc encryption (LUKS)

1. Backup your data (for worst case scenario)

1. Shrink linux partition by 512 MB.

1. Create unencrypted boot partition
    - fileystem: ext4
    - size: 512MB

1. There should already be an ESP (EFI system partition)
   - filesystem: FAT32
   - size: ~175 MB
   - flags: boot, esp

1. Find out what is what using ```lsblk```.

   **The names are most likely different, depending on your system, but this will be the mapping for further steps:
   ESP = sda1, linux partition = sda2, boot partition = sda3, backup partition sdb1**

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

1. Remove */mnt/boot* directory contents, because it resides on its own partition now.
    ```
    sudo rm -rf /mnt/boot/*
    ```

1. Make linux mount the boot partition at startup, by adding the following line to /etc/fstab
    ```
    UUID=XXX-XXX-XXX /boot ext4 defaults 0 2
    ```

1. Copy linux system to backup drive.
    ```
    sudo rsync -axHAWXS --numeric-ids --info=progress2 /mnt/linux/ /mnt/backup/
    ```

1. Optional: sanity check, should only return warning for device files etc.
    ```
    sudo diff -r --no-dereference /mnt/linux/ /mnt/backup/
    ```

1. Unmount linux partition.
    ```
    sudo umount /dev/sda2
    ```

1. Encrypt linux partition.
    ```
    sudo cryptsetup -v luksFormat /dev/sda2
    ```

1. Open encrypted partition. After that there will be a device */dev/mapper/systempartition*.
    ```
    sudo cryptsetup luksOpen /dev/sda2 systempartition
    ```

1. Create filesystem.
    ```
    sudo mkfs.ext4 /dev/mapper/systempartition
    ```

1. Mount the new drive.
    ```
    sudo mount /dev/mapper/systempartition /mnt/linux
    ```

1. Copy the linux system back.
    ```
    sudo rsync -axHAWXS --numeric-ids --info=progress2 /mnt/backup/ /dev/linux/
    ```

1. Find out UUID of encrypted device using blkid (block device ids)

1. Add this line to */etc/crypttab*:
    ```
    systempartition UUID=XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX	none	luks
    ```

1. Assign mount points for ```chroot```.
    ```
    sudo mount /dev/sda3 /mnt/linux/boot
    sudo mount /dev/sda1 /mnt/linux/boot/efi

    cd /mnt/linux
    sudo mount --bind /sys sys
    sudo mount --bind /dev dev
    sudo mount --bind /run run
    sudo mount --bind /proc proc
    sudo chroot /mnt/linux
    ```

1. Update the initial RAM filesystem:
    ```
    update-initramfs -u
    ```

1. Update booloader:
    ```
    update-grub
    ```

1. Make sure cryptsetup is installed on the actual system as well.

    ```
    apt install cryptsetup
    ```

1. Restart
