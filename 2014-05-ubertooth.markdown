On Ubuntu 12.04 LTS, following https://github.com/greatscottgadgets/ubertooth/wiki/Build-Guide

### Prerequisites ###

    sudo apt-get install cmake libusb-1.0-0-dev make gcc libbluetooth-dev

### Install PyUSB ###

    wget https://github.com/walac/pyusb/archive/1.0.0b1.tar.gz -O pyusb-1.0.0b1.tar.gz
    tar xvf pyusb-1.0.0b1.tar.gz
    cd pyusb-1.0.0b1
    sudo python setup.py install

### Install libbtbb ###

    wget https://github.com/greatscottgadgets/libbtbb/archive/2014-02-R2.tar.gz -O libbtbb-2014-02-R2.tar.gz
    tar xf libbtbb-2014-02-R2.tar.gz
    cd libbtbb-2014-02-R2
    mkdir build
    cd build
    cmake ..
    make
    sudo make install

### Ubertooth tools ###

    wget https://github.com/greatscottgadgets/ubertooth/archive/2014-02-R2.tar.gz -O ubertooth-2014-02-R2.tar.gz
    tar xf ubertooth-2014-02-R2.tar.gz
    cd ubertooth-2014-02-R2/host
    mkdir build
    cd build
    cmake ..
    make
    sudo make install

### Kismet wireless sniffer ###

    sudo apt-get install libpcap0.8-dev libcap-dev pkg-config build-essential libnl-dev libncurses-dev libpcre3-dev libpcap-dev libcap-dev
    wget https://kismetwireless.net/code/kismet-2013-03-R1b.tar.xz
    tar xf kismet-2013-03-R1b.tar.xz
    cd kismet-2013-03-R1b
    ln -s ../ubertooth-2014-02-R2/host/kismet/plugin-ubertooth .
    ./configure
    make && make plugins
    sudo make suidinstall
    sudo make plugins-install
    Add "pcapbtbb" to the "logtypes=..." line in /usr/local/etc/kismet.conf
