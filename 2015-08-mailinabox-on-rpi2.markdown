Download Ubuntu 14.04 image from http://www.finnie.org/software/raspberrypi/2015-04-06-ubuntu-trusty.zip

(Based on https://wiki.ubuntu.com/ARM/RaspberryPi)

    unzip 2015-04-06-ubuntu-trusty.zip
    sudo dd bs=4m if=2015-04-06-ubuntu-trusty.img of=/dev/rdisk1

Change password for user `ubuntu`

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

Remove `dovecot-lucene` from `apt_install` command here: https://github.com/mail-in-a-box/mailinabox/blob/master/setup/mail-dovecot.sh#L29

Also edit `/etc/dovecot/conf.d/10-mail.conf` to remove reference to `fts-lucene`

For some reason, add-apt-repository fails on the second run, but we don't need it anyway, so comment out https://github.com/mail-in-a-box/mailinabox/blob/a8074ae3e452bbc482d8f8910452378e5f9f0005/setup/system.sh#L27

### Run the install ###

    sudo ./setup/start.sh

Type in email address, hostname, and set country code.
