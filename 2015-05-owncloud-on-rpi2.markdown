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
