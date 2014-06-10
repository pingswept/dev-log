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

Get nodejs remnants off port 80 with these commands from http://stackoverflow.com/questions/16554756/how-do-i-release-port-80-on-a-beaglebone-so-i-can-use-it

    systemctl disable cloud9.service
    systemctl disable gateone.service
    systemctl disable gdm.service     # this disables the Gnome desktop which is maybe irrelevant here, but saves a ton of memory
    systemctl disable bonescript.service
    systemctl disable bonescript.socket
    systemctl disable bonescript-autorun.service

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
    chown -R www-data /var/log/uwsgi
    chgrp -R www-data /var/log/uwsgi

Install CodeMirror (also a git submodule, but needs an update?)

    cd /var/www/editor/static
    rm -rf codemirror
    wget https://github.com/marijnh/CodeMirror/archive/4.2.0.tar.gz
    tar xzvf 4.2.0.tar.gz
    mv CodeMirror-4.2.0/ codemirror

Other tools

    apt-get install zsh
    chsh
    Changing the login shell for root
    Enter the new value, or press ENTER for the default
    Login Shell [/bin/bash]: /bin/zsh
    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh

Set up uWSGI.

`/etc/uwsgi/emperor.ini`

    [uwsgi]
    emperor = /etc/uwsgi/vassals
    logto = /var/log/uwsgi/emperor.log
    uid = www-data
    gid = www-data

`/etc/uwsgi/vassals/editor.ini`

    [uwsgi]
    socket = 127.0.0.1:5001
    logto = /var/log/uwsgi/editor.log
    processes = 1
    env = PYTHONPATH=$PYTHONPATH:/var/www
    wsgi-file = /var/www/editor
    callable = editor

`/etc/uwsgi/vassals/public.ini`

    [uwsgi]
    socket = 127.0.0.1:5000
    logto = /var/log/uwsgi/public.log
    processes = 1
    env = PYTHONPATH=$PYTHONPATH:/var/www
    wsgi-file = /var/www/public
    callable = public
    catch-exceptions = True

Then set up Systemd startup script in `/etc/systemd/system/uwsgi.service`

    [Unit]
    Description=uWSGI Emperor
    After=syslog.target
    
    [Service]
    ExecStart=/usr/local/bin/uwsgi --ini /etc/uwsgi/emperor.ini
    Restart=always
    KillSignal=SIGQUIT
    Type=notify
    NotifyAccess=main
    
    [Install]
    WantedBy=multi-user.target

Enable uWSGI: `systemctl enable uwsgi.service`

### Getting websockets working ###

Install Gevent with pip to get version 1.0.1, as uWSGI needs 1.x or newer for websockets, I think. This also installs Greenlet.

    pip install gevent

    
