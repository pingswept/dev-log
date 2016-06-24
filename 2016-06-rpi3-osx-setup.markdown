Download Raspbian Jessie Lite.

    unzip 2016-05-27-raspbian-jessie-lite.zip

Use `diskutil list` to figure out where your SD card is. The N below will need to be replaced with the number that corresponds to your SD card. Get this wrong and you probably overwrite the boot sector of your hard drive.

Unmount the disk, if needed.

    sudo diskutil unmountDisk /dev/diskN

Copy over the disk image.

    sudo dd if=2016-05-27-raspbian-jessie-lite.img of=/dev/rdiskN bs=1m

Plug in ethernet and power up board via microUSB cable.

    ssh pi@raspberrypi.local

Use `raspi-config` to expand filesystem, set password, and change hostname.

To start wifi, add network name and password to `/etc/wpa_supplicant/wpa_supplicant.conf`

    network={
        ssid="Artisan's Asylum"
        psk="I won't download stuff that will get us in legal trouble."
    }

Check that you got an IP address with `ifconfig`

    inet addr:172.16.10.192

(Not just an IPv6 address like this: `inet6 addr: fe80::828d:1582:1a77:60d0/64`)

Then pull the ethernet cable and SSH over wifi. The hostname will be broadcast over both interfaces via Bonjour/Avahi.
