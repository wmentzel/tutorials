### Fix GRUB

#### (after Windows destroyed it for example)

    sudo cryptsetup luksOpen /dev/nvme0n1p3 systempartition
    
    sudo mkdir /mnt/linux
    sudo mount /dev/mapper/systempartition /mnt/linux

    sudo mount --rbind /sys /mnt/linux/sys
    sudo mount --rbind /dev /mnt/linux/dev
    sudo mount --rbind /run /mnt/linux/run
    sudo mount --bind /proc /mnt/linux/proc

    sudo mount /dev/nvme0n1p1 /mnt/linux/boot     # partiton should contain initrd.img* files
    sudo mount /dev/nvme0n1p2 /mnt/linux/boot/efi

    sudo chroot /mnt/linux

    update-initramfs -u -k all
    grub-install
    grub-update
