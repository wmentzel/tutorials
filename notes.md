update-grub
grub-install --recheck /dev/sdb
grub-mkconfig -o /boot/grub/grub.cfg
