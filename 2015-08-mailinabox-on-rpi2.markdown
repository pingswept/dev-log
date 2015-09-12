Download Ubuntu 14.04 image from http://www.finnie.org/software/raspberrypi/2015-04-06-ubuntu-trusty.zip

(Based on https://wiki.ubuntu.com/ARM/RaspberryPi)

    unzip 2015-04-06-ubuntu-trusty.zip
    sudo dd bs=4m if=2015-04-06-ubuntu-trusty.img of=/dev/rdisk1

Change password for user `ubuntu`

Install `curl`:

    sudo apt-get install curl

Bootstrap Mail-in-a-box

    curl -s https://mailinabox.email/bootstrap.sh | sudo bash
