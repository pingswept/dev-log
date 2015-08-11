First, install Raspbian Wheezy image on RPi2.

    dd bs=4M if=2015-05-05-raspbian-wheezy.img of=/dev/sdc
    
On OS X (note lower-case m in block size)

    sudo diskutil unmount /dev/disk1s1
    sudo dd bs=4m if=2015-05-05-raspbian-wheezy.img of=/dev/rdisk1

Expand filesystem, set user password, generate `en-us-UTF8` locale, and set hostname using `raspi-config`.

Remove `/home/pi/python_games`.

Upgrade to Jessie

    sudo apt-get update
    sudo apt-get upgrade
    sudo sed -i /deb/s/wheezy/jessie/g /etc/apt/sources.list
    sudo sed -i /deb/s/wheezy/jessie/g /etc/apt/sources.list.d/*.list
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade

404 error for http://raspberrypi.collabora.com/dists/jessie/rpi/binary-armhf/Packages, but that's probably just because Collabora hasn't released packages for Jessie yet.

During upgrade, answer Y to all questions.
