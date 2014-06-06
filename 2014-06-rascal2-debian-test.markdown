Download Debian file img.xz from http://debian.beagleboard.org/images/BBB-eMMC-flasher-debian-7.5-2014-05-14-2gb.img.xz

Unzip with Unarchiver (on OS X)

Next bit from https://help.ubuntu.com/community/Installation/FromImgFiles

`diskutil list` then insert SD card, then `diskutil list` to figure out SD card device name

Assuming card is disk1:

    diskutil unmountDisk /dev/disk1
    sudo dd if=/path/to/downloaded.img of=/dev/rdisk1 bs=1m
    diskutil eject /dev/disk1

`dd` takes ~5 min.

On first boot, SD card will copy itself to eMMC, which unfortunately is only 2 GB. Takes 5-10 min.

Change passwd for debian, root accounts.

Edit /etc/hostname

    apt-get install nginx
    apt-get install python-flask

Remove stuff

    apt-get remove apache2 apache2-mpm-worker apache2-utils apache2.2-bin apache2.2-common
    apt-get remove nodejs

Could install uWSGI 1.2 from packages

    apt-get install uwsgi
    apt-get install uwsgi-plugin-python python-uwsgidecorators

Instead, build uWSGI from source

    apt-get install python-dev
    pip install uwsgi
    pip install pytronics bitstring webcolors Flask-Login

    cd /var/www

Running on port 5000 without Nginx frontend

    /usr/local/bin/uwsgi --http :5000 --wsgi-file public --callable public

With Nginx

    /usr/local/bin/uwsgi --socket :5000 --wsgi-file /var/www/public --callable public
    /usr/local/bin/uwsgi --socket :5001 --wsgi-file /var/www/editor --callable editor

Install Red and demos

    cd /var/www
    git clone https://github.com/rascalmicro/red.git editor
    git clone https://github.com/rascalmicro/demos.git public
    touch /var/log/uwsgi/public.log
    touch /var/log/uwsgi/emperor.log
    touch /var/log/uwsgi/editor.log 

Install CodeMirror (also a git submodule, but needs an update?)

    cd /var/www/editor/static
    rm -rf codemirror
    wget https://github.com/marijnh/CodeMirror/archive/4.2.0.tar.gz
    tar xzvf 4.2.0.tar.gz
    mv CodeMirror-4.2.0/ codemirror

