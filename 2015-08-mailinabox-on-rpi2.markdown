Download Ubuntu 14.04 image from http://www.finnie.org/software/raspberrypi/2015-04-06-ubuntu-trusty.zip

(Based on https://wiki.ubuntu.com/ARM/RaspberryPi)

    unzip 2015-04-06-ubuntu-trusty.zip
    sudo dd bs=4m if=2015-04-06-ubuntu-trusty.img of=/dev/rdisk1

Change password for user `ubuntu`

Install `curl`:

    sudo apt-get install curl

Bootstrap Mail-in-a-box

    curl -s https://mailinabox.email/bootstrap.sh | sudo bash

Type in email address, hostname, and set country code.

    E: Unable to locate package dovecot-lucene

Hmmm. Need `dovecot-lucene` for armhf. Try `wget http://http.us.debian.org/debian/pool/main/d/dovecot/dovecot-lucene_2.2.13-12~deb8u1_armhf.deb`

    sudo apt-get install dovecot-core

(Also trying in parallel on BBB.)
Disk image from https://rcn-ee.com/rootfs/2015-09-11/elinux/ubuntu-14.04.3-console-armhf-2015-09-11.tar.xz

    sudo ./setup_sdcard.sh --mmc /dev/sdc --dtb beaglebone

### Fresh attempt ###

    sudo apt-get install git vim curl
    git clone https://github.com/mail-in-a-box/mailinabox.git
    cd mailinabox
    vim setup/mail-dovecot.sh

Remove `dovecot-lucene` from `apt_install` command here: https://github.com/mail-in-a-box/mailinabox/blob/master/setup/mail-dovecot.sh#L29

    sudo ./setup/start.sh

