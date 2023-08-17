### How to install Ubuntu (20.04) using full disc encryption (LUKS) alongside an already existing Windows installation

1. Boot the Ubuntu Live CD.

2. Shrink Windows partition as much as you like.

3. Create boot partition out of the new unallocated space.
   - filesystem: FAT32 or ext4
   - flags: boot
   - size: 512 MB

4. Create linux system partition out of remaining unallocated space.
   - filesystem: exf4
   - size: as big as possible

5. There should already be an ESP (EFI system partition).
   - filesystem: fat32
   - size: ~175MB
   - flags: boot, esp

6. Find the name of the future linux partition:

    ```
    lsblk
    ```
   **The names are most likely different, depending on your system, but this will be the mapping for further steps:
   ESP = sda1, boot partition = sda2, linux partition = sda3**

7. Encrypt partition
    ```
    sudo cryptsetup -v luksFormat /dev/sda3
    ```

8. Type "YES".

9. Type in a good passphrase.

10. Open the encrypted partition. The unecrypted device will be */dev/mapper/systempartition*
     ```
     sudo cryptsetup luksOpen /dev/sda3 systempartition
     ```

11. Format the mounted partition
     ```
     sudo mkfs.ext4 /dev/mapper/systempartition
     ```

12. Boot Ubuntu from USB stick/CD

13. Choose "Install Ubuntu"

14. During installation choose "Something else" when asked where to install Ubuntu

15. Assign mount points
   - Choose ext4 for */dev/mapper/systempartition* and mount it at */*
   - Choose ext4 for */dev/sda2* and mount it at */boot*
   - ESP partition should already be assigned correctly

16. After the installation is done click "Continue testing".

17. Mount the linux system partition.
     ```
     sudo mount /dev/mapper/systempartition /mnt
     ```

18. Mount boot critical paritions.
     ```
     sudo mount /dev/sda2 /mnt/boot
     sudo mount /dev/sda1 /mnt/boot/efi
     ```

19. Assign mount points for ```chroot```.
     ```
     cd /mnt
     sudo mount --bind /sys sys
     sudo mount --bind /dev dev
     sudo mount --bind /run run
     sudo mount --bind /proc proc
     sudo chroot /mnt
     ```

20. Get the UUID of */dev/sda3* (Ubuntu partition)
     ```
     sudo blkid (get UUID)
     ```

21. Add this line to */etc/crypttab*:
     ```
     systempartition UUID=XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX	none	luks
     ```

22. Update the initial RAM filesystem:

     ```
     update-initramfs -u
     ```

23. Make sure cryptsetup is installed on the actual system as well.

     ```
     apt install cryptsetup
     ```

24. Reboot

25. You should be presented with the GRUB boot menu.

26. Choose "Ubuntu".

27. When asked for the passphrase, type it in.

28. Done, Ubuntu should start as before.
