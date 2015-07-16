First, install Raspbian Wheezy image on RPi2.

    dd bs=4M if=2015-05-05-raspbian-wheezy.img of=/dev/sdc

Unfortunately, DHCP server at Asylum is borked, so it doesn't dispense an IP properly to the RPi 2 ("Wrong server-ID")

Connect to RPi 2 with 3.3 V FTDI cable to GND, TX, RX, following pinout: http://pi.gadgetoid.com/pinout

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

Or uncomment relevant locale in `/etc/locale.gen` and run `locale-gen`

### Python 3.4 ###

Flask works with Python 3, but only 3.3+, while Raspbian Jessie only has 3.2. So, we need to build Python 3.4. Wait, maybe I just haven't upgraded to Jessie yet. Accch.
