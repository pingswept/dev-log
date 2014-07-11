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

    passwd
    passwd debian

Edit /etc/hostname

    apt-get install nginx python-flask
    systemctl enable nginx.service

Remove stuff

    apt-get remove apache2 apache2-mpm-worker apache2-utils apache2.2-bin apache2.2-common nodejs

Get nodejs remnants off port 80 with these commands from http://stackoverflow.com/questions/16554756/how-do-i-release-port-80-on-a-beaglebone-so-i-can-use-it

    systemctl disable cloud9.service
    systemctl disable gateone.service
    systemctl disable gdm.service     # this disables the Gnome desktop which is maybe irrelevant here, but saves a ton of memory
    systemctl disable bonescript.service
    systemctl disable bonescript.socket
    systemctl disable bonescript-autorun.service

List Debian packages by size

    dpkg-query -W --showformat='${Installed-Size;10}\t${Package}\n' | sort -k1,1n

Remove some more stuff

    apt-get remove xscreensaver xscreensaver-data xserver-xorg-core xserver-common
    apt-get remove libgtk2.0-common # probably breaks OpenCV
    apt-get remove libgtk-3-common # removes Gstreamer and Numpy
    apt-get autoremove

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

Install Supervisor

    apt-get install supervisor

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
    pip install gevent-websocket
    pip install Flask-Sockets

### Hmmm ###

    pip uninstall Flask-Sockets
    Uninstalling Flask-Sockets:
    Proceed (y/n)? Y
      Successfully uninstalled Flask-Sockets

Flask-uWSGI-WebSocket might be better.      
      
    pip install Flask-uWSGI-WebSocket
    Downloading/unpacking Flask-uWSGI-WebSocket
      Downloading Flask-uWSGI-WebSocket-0.2.5.tar.gz
      Running setup.py egg_info for package Flask-uWSGI-WebSocket
        
        file flask_uwsgi_websocket.py (for module flask_uwsgi_websocket) not found
    Requirement already satisfied (use --upgrade to upgrade): Flask in /usr/lib/python2.7/dist-packages (from Flask-uWSGI-WebSocket)
    Requirement already satisfied (use --upgrade to upgrade): uwsgi in /usr/local/lib/python2.7/dist-packages (from Flask-uWSGI-WebSocket)
    Requirement already satisfied (use --upgrade to upgrade): Werkzeug>=0.6.1 in /usr/lib/python2.7/dist-packages (from Flask->Flask-uWSGI-WebSocket)
    Requirement already satisfied (use --upgrade to upgrade): Jinja2>=2.4 in /usr/lib/python2.7/dist-packages (from Flask->Flask-uWSGI-WebSocket)
    Installing collected packages: Flask-uWSGI-WebSocket
      Running setup.py install for Flask-uWSGI-WebSocket
        file flask_uwsgi_websocket.py (for module flask_uwsgi_websocket) not found
        
        file flask_uwsgi_websocket.py (for module flask_uwsgi_websocket) not found
        file flask_uwsgi_websocket.py (for module flask_uwsgi_websocket) not found
        file flask_uwsgi_websocket.py (for module flask_uwsgi_websocket) not found
    Successfully installed Flask-uWSGI-WebSocket
    Cleaning up...

Got it working by modifying one line of echo example slightly: https://github.com/zeekay/flask-uwsgi-websocket/blob/master/examples/echo/templates/index.html#L20

Change `ws = new WebSocket('ws://127.0.0.1:5000/websocket');` to `ws = new WebSocket('ws://rascal2.local:8080/websocket');`

Then start with `uwsgi --master --http :8080 --http-websockets --wsgi echo:app`

For uWSGI chat example, need Redis and Python client for Redis

    apt-get install redis-server
    pip install redis


