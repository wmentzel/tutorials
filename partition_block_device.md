## Partition block device

    sudo fdisk /dev/sda
    m # show options
    n # new partition
    p # primary
    <enter> <enter> <enter> # accept defaults
    p # show partitons
    w # write changes
    sudo mkfs.ext4 /dev/sda1