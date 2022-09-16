### Fix GRUB 
#### (after Windows destroyed it for example)

    sudo cryptsetup luksOpen /dev/nvme0n1p3 systempartition
    
    sudo mkdir /mnt/linux
    sudo mount /dev/mapper/systempartition /mnt/linux

    cd /mnt/linux
    sudo mount --bind /sys sys
    sudo mount --bind /dev dev
    sudo mount --bind /run run
    sudo mount --bind /proc proc
    sudo chroot /mnt/linux

    sudo mount /dev/nvme0n1p1 /boot     # partiton should contain initrd.img* files
    sudo mount /dev/nvme0n1p2 /boot/efi
    
    sudo update-initramfs -u
    
    sudo grub-install
