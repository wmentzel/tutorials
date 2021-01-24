### Useful terminal commands

This is a random collection of terminal commands I need regularly. It is mostly meant for me to quickly lookup those commands. Maybe some are helpful for other people as well.

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

Get Git hash of file

    git hash-object test.txt

Diff current working tree with another commit

    git diff HEAD~2 --word-diff=color

Add origin of Git repository

    git remote set-url origin git@github.com:wmentzel/ubuntu-backup-script.git

Change origin of Git repository

    git remote add origin git@github.com:wmentzel/ubuntu-backup-script.git

Remove origin from Git repository

    git remote rm origin

Set upstream branch of current branch

    git branch --set-upstream-to=origin/master

Show branch graph

    git log --graph --oneline

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

    ssh-keygen –t rsa -b 4096 –C <email address
