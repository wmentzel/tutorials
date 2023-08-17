### Useful terminal commands

This is a random collection of terminal commands I need regularly. It is mostly meant for me to quickly lookup those
commands. Maybe some are helpful for other people as well.

Manage wireless networks

    nmtui

List all available wifi networks

    nmcli d wifi list

Show USB devices

    lsusb

Securely delete a harddrive

    dd if=/dev/zero of=/dev/sdX bs=4096 status=progress
    dd if=/dev/urandom of=/dev/sdX bs=4096 status=progress

Sign an APK file

    apksigner sign --ks <KEYSTORE_FILE_NAME>.keystore <APK_FILE_NAME>.apk

Get number of lines

     wc -l filename.txt

Watch specific directory contents, updating every second

    watch -n 1 -d find .

Diff two files side by side:

    diff --color=always jobs.sh jobs2.sh -y

Configure used Java version

    update-alternatives --config java

Get IP address

    hostname -I

Run single test via Gradle

    ./gradlew :modulename:test --tests "package.path.TestClass"

Show installation date of Ubuntu

    ls -lt /var/log/installer

Show all shutdown events

    last -x --time-format=full

Find PID of process listening on specific port

    sudo ss -tulpn | grep 808

Diff two directories recursively

    diff -qr directory1 directory2

Show current time and date

    sudo hwclock --show

Make file/directory executable

    chmode +x <filename>

Change owner of a file/directory

    chmod <user>:<group> <filepath>

Show size of directory

    du -shL jlocker

Show discs sizes

    df -h

Show all block devices

    lsblk -a

Copy files recursively with all attribues (resolve symbolic links)

    rsync -arL --progress <source> <dest>

Mount drive

    mount /dev/sda1 /mnt/backup

Manage system cronjobs

    crontab -e

Important files

    /etc/apache2/apache2.conf # config file for apache2
    /etc/samba/smb.conf # config file path for samba

Send mail

    mail -s “Title” <email> <<< “Text Body”

Grep number with arbitrary length

    egrep -o ‘[0-9]*’

Show members of a group

    members <groupname>

Show how long system is running since last start

    uptime

Change password of current user

    passwd

Show network load

    nload <device>

Add repository to package sources

    sudo add-apt-repository ppa:whatever/ppa

Generate SSH key pair

    ssh-keygen -t rsa -b 4096 -C <email address>

Create GRUB config

    grub-mkconfig -o /boot/grub/grub.cfg

List installed packages

    dpkg --list | grep linux-image

Install packages from directory

    sudo dpkg -i *.deb

Remove kernels

    sudo apt-get purge linux-headers-5.12.0-051200-generic

Show networks and their UUID

    nmcli c

Connect to network with UUID

    nmcli c up "8aca5106-93c4-4d67-8c9e-2b9dc96f51d3"

