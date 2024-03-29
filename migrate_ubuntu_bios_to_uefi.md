### How to migrate Ubuntu (18.04) from BIOS (legacy mode) to UEFI

1. Change your system firmware from (legacy) BIOS to EFI mode

2. Boot Ubuntu Live CD

3. To make sure that you really booted in EFI mode the following directory should exist: */sys/firmware/efi*. If not
   don't continue.

4. Create ESP (EFI system partition) using GParted
   - filesystem: fat32
   - size: 175 MB
   - flags: boot, esp

5. Find out UUID of newly created ESP.
    ```
    blkid
    ```

6. Find out what is what.
    ```
    lsblk
    ```
   **From here on I will refer to the linux partion as sda1 and to the ESP as sda2**

7. Mount linux system partition.
    ```
    sudo mount /dev/sda1 /mnt
    ```

8. Mount ESP during boot. Add the following line to ```/mnt/etc/fstab```
    ```
    UUID=XXXX-XXXX  /boot/efi       vfat    umask=0077      0       1
    ```

9. Assign mount points for ```chroot```.
    ```
    sudo mkdir /mnt/boot/efi
    sudo mount /dev/sda2 /mnt/boot/efi
    cd /mnt
    sudo mount --bind /sys sys
    sudo mount --bind /dev dev
    sudo mount --bind /run run
    sudo mount --bind /proc proc
    sudo chroot /mnt
    ```

10. Install GRUB for EFI package.
     ```
     apt install grub-efi
     ```

11. Reinstall GRUB.
     ```
     grub-install --target=x86_64-efi /dev/sda
     ```

    **Important: Use the whole disc *sda* and not a specific partition like *sda1***.

12. Reboot
