### Installing client ###

There is no owncloud-client Debian package for armhf, which is the architecture of Novena, RPi 1 and 2, and BBB.

    sudo apt-get install libowncloudsync0 libneon27-dev qtkeychain-dev libsqlite3-dev libqt4-dev libqt4-sql-sqlite
    cd repos
    git clone git://github.com/owncloud/client.git owncloud-client
    mkdir owncloud-client-build
    cd owncloud-client-build
    cmake -DCMAKE_BUILD_TYPE="Debug" ../owncloud-client
    make
    sudo make install
    
Appears to build correctly, but then:

    owncloud
    owncloud: symbol lookup error: /usr/lib/arm-linux-gnueabihf/libowncloudsync.so.0: undefined symbol: csync_set_config_dir

I think what's going on is that I inadvertently installed two incompatible versions of libowncloudsync.so.0. One was built in the build process above, but I had previously installed it via `apt-get`.

Here they are:

    ls -l /usr/lib/arm-linux-gnueabihf | grep own
    lrwxrwxrwx  1 root root       24 Oct 24  2014 libowncloudsync.so.0 -> libowncloudsync.so.1.6.4
    -rw-r--r--  1 root root   734564 Oct 24  2014 libowncloudsync.so.1.6.4
    drwxr-xr-x  2 root root     4096 May 28 17:32 owncloud
    ls -l /usr/local/lib/ | grep own             
    lrwxrwxrwx 1 root staff      20 May 28 17:30 libowncloudsync.so -> libowncloudsync.so.0
    lrwxrwxrwx 1 root staff      24 May 28 17:30 libowncloudsync.so.0 -> libowncloudsync.so.2.0.0
    -rw-r--r-- 1 root staff 7703996 May 28 17:22 libowncloudsync.so.2.0.0
    drwxr-sr-x 2 root staff    4096 May 28 17:30 owncloud


### Installing server ###

First, install Raspbian Wheezy image on RPi2.

    dd bs=4M if=2015-05-05-raspbian-wheezy.img of=/dev/sdc

Unfortunately, DHCP server at Asylum is borked, so it doesn't dispense an IP properly to the RPi 2 ("Wrong server-ID")

Connect to RPi with 3.3 V FTDI cable to GND, TX, RX, following pinout: http://pi.gadgetoid.com/pinout

Then `screen /dev/ttyUSB0 115200`

Change to static IP by editing `/etc/network/interfaces`

    auto eth0
    allow-hotplug eth0
    #iface eth0 inet manual
    iface eth0 inet static
    address 172.16.128.100  
    gateway 172.16.0.1
    netmask 255.255.0.0
    network 172.16.0.0
    broadcast 172.16.255.255

Also, add DNS to `/etc/resolvconf.conf`: `8.8.4.4`

Generate `en-us-UTF8` locale using `raspi-config`.

Based on https://debiantalk.wordpress.com/2015/02/17/how-to-install-owncloud-8-community/

    echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/Debian_7.0/ /' >> /etc/apt/sources.list.d/owncloud.list
    wget http://download.opensuse.org/repositories/isv:ownCloud:community/Debian_7.0/Release.key
    apt-key add - < Release.key
    apt-get update
    apt-get install owncloud

For HTTPS redirect, add to `/var/www/index.html`:

    <html>
      <head>
        <meta http-equiv="refresh" content="0; URL=https://mydomain.org/owncloud/">
      </head>
    </html>

Set up SSL.

    mkdir /etc/apache2/ssl
    openssl req -new -sha256 -x509 -nodes -days 365 -out /etc/apache2/ssl/your.domain.pem -keyout /etc/apache2/ssl/your.domain.key
    a2enmod ssl
    a2ensite default-ssl
    service apache2 reload

Set ServerName in 
