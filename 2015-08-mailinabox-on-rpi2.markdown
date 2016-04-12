# Mail-in-a-box on a Raspberry Pi 2 #

This is not supported by anyone in any way. It's probably not even a good idea. You should probably give up now.

### Install Ubuntu 14.04 ###

Download Ubuntu 14.04 image from http://www.finnie.org/software/raspberrypi/2015-04-06-ubuntu-trusty.zip

(Based on https://wiki.ubuntu.com/ARM/RaspberryPi)

    unzip 2015-04-06-ubuntu-trusty.zip
    sudo dd bs=4m if=2015-04-06-ubuntu-trusty.img of=/dev/rdisk1

Boot the RPi2 with an HDMI monitor and keyboard attached.

Install an SSH server.

    sudo apt-get update
    sudo apt-get install openssh-server
    sudo reboot

Change password for user `ubuntu` from default, which is `ubuntu`

    passwd

### Resize file system ###

From https://ubuntu-mate.org/raspberry-pi/

    sudo fdisk /dev/mmcblk0

Delete the second partition (d, 2), then re-create it using the defaults (n, p, 2, enter, enter), then write and exit (w). Reboot the system, then:

    sudo resize2fs /dev/mmcblk0p2

### Get Mail-in-a-box and slightly modify it ###

Install `curl`, `git`, and `vim`:

    sudo apt-get install curl git vim

Clone Mail-in-a-box repository

    git clone https://github.com/mail-in-a-box/mailinabox.git
    cd mailinabox
    vim setup/mail-dovecot.sh

Remove `dovecot-lucene` from `apt_install` command here:  
https://github.com/mail-in-a-box/mailinabox/blob/master/setup/mail-dovecot.sh#L29

Also remove reference to `fts-lucene` from `tools/editconf.py` command here:  
https://github.com/mail-in-a-box/mailinabox/blob/master/setup/mail-dovecot.sh#L102

...and again, here:  
https://github.com/mail-in-a-box/mailinabox/blob/master/setup/mail-dovecot.sh#L106

For some reason, add-apt-repository fails on the second run, but we don't need it anyway, so comment out  
https://github.com/mail-in-a-box/mailinabox/blob/a8074ae3e452bbc482d8f8910452378e5f9f0005/setup/system.sh#L27

### Run the install ###

Set `ARM=1` to disable x86_64 architecture check in `preflight.sh`.

    ARM=1
    sudo ./setup/start.sh

Type in email address, hostname, and set country code.

### Error about NeedToInstallFile .well-known/acme ###

In trying to provision SSL certificates, ran into an error in which a file needed to be installed in a certain location for a Let's Encrypt security check. The actual problem was just that my firewall was blocking the SSL port, 443. After I forwarded port 443 to the server, the check worked.

### Free memory dropping below 20% ###

This error was caused by irqbalance leaking memory. You can restart irqbalance, which frees all of the leaked memory, with:

    /usr/sbin/service irqbalance restart

A short term fix would be to restart irqbalance on a daily basis via cron as described here: http://www.pclinuxos.com/forum/index.php?topic=100977.0 but note that the the path to `service` is `/usr/sbin/service` not just `/sbin/service`.

In the long term, presumably some responsible individual will fix irqbalance. As of 2016-04-09, Mail-in-a-box uses irqbalance 1.0.6-2ubuntu0.14.04.4. It appears that the bug was reported and fixed in 2014 here: https://github.com/Irqbalance/irqbalance/issues/5

Might also try recompiling a fixed version from source, if you care: http://www.circuidipity.com/raspberry-pi-ram-irqbalance.html
