### Installing FreeBasic compiler ###

    wget http://www.freebasic-portal.de/dlfiles/625/freebasic_1.01.0debian7_armhf.deb
    dpkg --install freebasic_1.01.0debian7_armhf.deb

The `dpkg` line will complain about missing `libxpm-dev`

This fixes the missing dependency: `apt-get -f install`

### Install PRUSS driver kit ###

    wget http://www.freebasic-portal.de/dlfiles/539/FB_prussdrv-0.0.tar.bz2
    tar xjf FB_prussdrv-0.0.tar.bz2
    mkdir /usr/include/freebasic/BBB
    cd FB_prussdrv-0.0.tar.bz2
    cp include/* /usr/include/freebasic/BBB/ # copy over the .bi files

Maybe want to copy over `pasm` as well, but the version installed is newer (0.86) than the version in FB_prussdrv-0.0.tar.bz2 (0.84), so I'll skip that.

### Install libpruio ###

    wget http://www.freebasic-portal.de/dlfiles/592/libpruio-0.2.tar.bz2
    tar xjf libpruio-0.2.tar.bz2
    cd libpruio-0.2/src
    cp c_wrapper/libpruio.so /usr/lib
    ldconfig
    cp c_wrapper/pruio*.h* /usr/include 
    cp config/libpruio-0A00.dtbo /lib/firmware 
    cp pruio/pruio*.bi /usr/include/freebasic/BBB 
    cp pruio/pruio.hp /usr/include/freebasic/BBB

Useful links:

* http://users.freebasic-portal.de/tjf/Projekte/libpruio/doc/html/_cha_preparation.html
* http://hackaday.com/2015/02/16/library-upgrade-to-pru-gives-fast-io-on-beaglebone/ (see comments too)
* Summary of Device Tree syntax: http://www.devicetree.org/Device_Tree_Usage

### Different types of Device Tree files ###

* `.dts`: main source file for board-level info
* `.dtsi`: source file, include file, like C header, for platform-level info that is used on multiple boards
* `.dtb`: binary file output from `dtc`, the Device Tree compiler
* `.dtbo`: overlay binary file, fragment of hardware info that is added to main `.dtb`
