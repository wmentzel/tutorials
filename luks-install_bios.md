1) shrink windows partition
2) create two new partitions (linux exf4, boot exf4)
2.1) lsblk
3) cryptsetup -v luksFormat /dev/sda2
4) sudo cryptsetup luksOpen /dev/sda2 sda2-dec
5) sudo mkfs.ext4 /dev/mapper/sda2-dec

6) install ubuntu (decrypted partition -> /, boot parition -> /boot)
7) "continue testing" 
8) mount a lot of stuff, then chroot...

sudo mount /dev/mapper/sda2-dec /mnt
sudo mount /dev/sda1 /mnt/boot
cd /mnt

sudo mount --bind /sys sys
sudo mount --bind /dev dev
sudo mount --bind /run run
sudo mount --bind /proc proc
sudo chroot /mnt

9) sudo blkid (get UUID)
10) sudo nano /etc/crypttab
>> sdb2-enc UUID=jslfdjj-jksldjf-jljsdf-jljsdf	none	luks

11) update-initramfs -u
----------------------------------------------------
maybe this is not needed, not tested without thought
----------------------------------------------------
12) grub-install --recheck /dev/sdb
13) grub-mkconfig -o /boot/grub/grub.cfg