Download Raspbian Jessie Lite.

    unzip 2016-05-27-raspbian-jessie-lite.zip

Use `diskutil list` to figure out where your SD card is. The N below will need to be replaced with the number that corresponds to your SD card. Get this wrong and you probably overwrite the boot sector of your hard drive.

Unmount the disk, if needed.

    sudo diskutil unmountDisk /dev/diskN

Copy over the disk image.

    sudo dd if=2016-05-27-raspbian-jessie-lite.img of=/dev/rdiskN bs=1m
