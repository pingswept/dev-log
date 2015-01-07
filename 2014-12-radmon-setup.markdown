### Base system ###

Download https://rcn-ee.net/rootfs/bb.org/testing/2014-12-19/lxqt/BBB-eMMC-flasher-debian-jessie-lxqt-armhf-2014-12-19-2gb.img.xz

Unzip with Unarchiver (on OS X)

Next bit from https://help.ubuntu.com/community/Installation/FromImgFiles

`diskutil list` then insert SD card, then `diskutil list` to figure out SD card device name

Assuming card is disk1:

    diskutil unmountDisk /dev/disk1
    sudo dd if=/path/to/downloaded.img of=/dev/rdisk1 bs=1m
    diskutil eject /dev/disk1

`dd` takes ~5 min.

On first boot, SD card will copy itself to eMMC. Takes 5-10 min.

Change passwd for debian, root accounts.

    passwd
    passwd debian

Edit /etc/hostname to set the hostname however you like.

Remove stuff

    apt-get remove nodejs xserver-xorg-core xserver-common

Get Node.js remnants off port 80 with these commands from http://stackoverflow.com/questions/16554756/how-do-i-release-port-80-on-a-beaglebone-so-i-can-use-it

    systemctl disable cloud9.service
    systemctl disable gateone.service
    systemctl disable gdm.service     # this disables the Gnome desktop which is maybe irrelevant here, but saves a ton of memory
    systemctl disable bonescript.service
    systemctl disable bonescript.socket
    systemctl disable bonescript-autorun.service
    systemctl disable cloud9.socket

Update repo lists and install stuff we'll need.

    sudo apt-get update

    sudo apt-get install htop gpsd-clients libpq-dev lsof postgis postgresql python-matplotlib python-mpltoolkits.basemap python-psycopg2 python-scipy supervisor

### Run headless monitor under Supervisor ###

Copy to `/etc/supervisor/conf.d/radmon.conf`

    [program:radmon]
    command=/root/headlessMonitor.py
    redirect_stderr=true
    stdout_logfile=/var/log/radmon.log
    stdout_logfile_maxbytes=10MB
    stdout_logfile_backups=10

### Adding radmon-specific stuff ###
Add `#!/usr/bin/python` to `headlessMonitor.py`

`chmod a+x headlessMonitor.py`

    apt-get install libftdi-dev
    git clone https://github.com/rascalmicro/emorpho-cpython.git
    git checkout -b linux origin/linux

In `emorpho-cpython-linux`, run `python ./setup.py install`
In `geopy-master`, run `python ./setup.py install`

### Load database schema ###

    mv psql-update /usr/local/psql-update
    chown postgres:postgres psql-update

    sudo -u postgres createdb radiation
    sudo -u postgres psql radiation
    sudo -u postgres psql -d radiation -f /usr/share/postgresql/9.1/contrib/postgis-1.5/postgis.sql
    sudo -u postgres psql -d radiation -f /usr/share/postgresql/9.1/contrib/postgis-1.5/spatial_ref_sys.sql
    \i /usr/local/psql-update
    
    CREATE ROLE radiation PASSWORD 'password-goes-here' login;

### Some difficulty with faked GPS data going into db ###

    psycopg2.ProgrammingError: column "location" is of type geometry but expression is of type numeric
    LINE 1: ...ps, histogram, altitude, sensor, version) VALUES (2.0, 3.0, ...
                                                                 ^
    HINT:  You will need to rewrite or cast the expression.

### Open files ###

    root@rascal-green:~# lsof | wc -l
    1682

### Memory leak ###

headlessMonitor.py is leaking at ~50 MB/hour. At a sample time of 2 seconds (1800 samples/hour), that's around 28kB per sample.

### Notes on PostGIS 2.0 build (not actually needed) ###
