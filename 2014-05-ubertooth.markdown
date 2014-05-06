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

These are the files installed as part of Ubertooth tools (from ~/ubertooth-2014-02-R2/host/build/install_manifest.txt)

Verified that they are actually there.

    /usr/local/lib/libubertooth.so.0.2
    /usr/local/lib/libubertooth.so.0
    /usr/local/lib/libubertooth.so
    /usr/local/include/ubertooth.h
    /usr/local/include/ubertooth_control.h
    /usr/local/include/ubertooth_interface.h
    /usr/local/bin/ubertooth-rx
    /usr/local/bin/ubertooth-dump
    /usr/local/bin/ubertooth-util
    /usr/local/bin/ubertooth-btle
    /usr/local/bin/ubertooth-follow
    /usr/local/bin/ubertooth-scan
    /usr/local/bin/ubertooth-debug
    /usr/local/bin/ubertooth-specan-ui
    /usr/local/bin/ubertooth-dfu

It seems that nothing can find libubertooth.so.0.

    ubertooth-rx
    ubertooth-rx: error while loading shared libraries: libubertooth.so.0: cannot open shared object file: No such file or directory

Similar problems from ubertooth scan and kismet.
