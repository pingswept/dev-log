### Installing client ###

There is no owncloud-client for armhf, which is the architecture of Novena, RPi 1 and 2, and BBB.



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

Based on https://debiantalk.wordpress.com/2015/02/17/how-to-install-owncloud-8-community/

    echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/Debian_7.0/ /' >> /etc/apt/sources.list.d/owncloud.list
    wget http://download.opensuse.org/repositories/isv:ownCloud:community/Debian_7.0/Release.key
    apt-key add - < Release.key
    apt-get update
    apt-get install owncloud
