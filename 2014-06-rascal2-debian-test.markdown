Download Debian file img.xz

Unzip with Unarchiver (on OS X)

`diskutil list` then insert SD card, then `diskutil list` to figure out SD card device name

Assuming card is disk1:

    diskutil unmountDisk /dev/disk1
    sudo dd if=/path/to/downloaded.img of=/dev/rdisk1 bs=1m
    diskutil eject /dev/disk1

Change passwd for debian, root accounts.

Edit /etc/hostname

apt-get install uwsgi
