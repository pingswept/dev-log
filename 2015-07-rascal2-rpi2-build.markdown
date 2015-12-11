First, install Raspbian Wheezy image on RPi2.

    dd bs=4M if=2015-05-05-raspbian-wheezy.img of=/dev/sdc
    
On OS X (note lower-case m in block size)

    sudo diskutil unmount /dev/disk1s1
    sudo dd bs=4m if=2015-05-05-raspbian-wheezy.img of=/dev/rdisk1

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

Upgrade to Jessie? Run the build script?

### Install Fadecandy ###

    git clone git://github.com/scanlime/fadecandy
    cd fadecandy/server
    make submodules
    make
    sudo mv fcserver /usr/local/bin

Create `fadecandy.conf` in `/etc/supervisor/conf.d`

    [program:fadecandy]
    command = fcserver /etc/fcserver.json
    user = root
    autostart = true
    autorestart = true
    stdout_logfile = /var/log/supervisor/fadecandy.log
    stderr_logfile = /var/log/supervisor/fadecandy_err.log

### Install Skunk's flames ###

    git clone https://github.com/rascalmicro/flaming-skunk.git
    
Move `sternoslomo.conf` to `/etc/supervisor/conf.d/sternoslomo.conf` to start `flames.py` at boot.
    
Put `fcserver.json` at `/etc/fcserver.json` and update the Fadecandy serial in the file to the correct one, which you can get from `dmesg`.
