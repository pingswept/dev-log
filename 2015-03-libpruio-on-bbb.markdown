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

RCN says that the best way to get libpruio working on new Debian is to donwgrade the kernel to 3.8.x.

Downgraded to 3.8.13-bone70

    apt-get update
    apt-get install linux-image-3.8.13-bone70
    reboot

When I try to load libpruio, this happens:

    sudo echo libpruio >  /sys/devices/bone_capemgr.9/slots
    echo: write error: No such file or directory

Here's what appears in dmesg:

    [  199.719576] bone-capemgr bone_capemgr.9: part_number 'libpruio', version 'N/A'
    [  199.719757] bone-capemgr bone_capemgr.9: slot #8: generic override
    [  199.719802] bone-capemgr bone_capemgr.9: bone: Using override eeprom data at slot 8
    [  199.719849] bone-capemgr bone_capemgr.9: slot #8: 'Override Board Name,00A0,Override Manuf,libpruio'
    [  199.726206] bone-capemgr bone_capemgr.9: slot #8: Requesting part number/version based 'libpruio-00A0.dtbo
    [  199.726269] bone-capemgr bone_capemgr.9: slot #8: Requesting firmware 'libpruio-00A0.dtbo' for board-name 'Override Board Name', version '00A0'
    [  199.750455] bone-capemgr bone_capemgr.9: failed to load firmware 'libpruio-00A0.dtbo'

The [libpruio installation instructions](http://users.freebasic-portal.de/tjf/Projekte/libpruio/doc/html/_cha_preparation.html) say that you can also do this:

    sudo echo BB-BONE-PRU-01 > /sys/devices/bone_capemgr.*/slots

This makes this appear in dmesg:

    [  216.578880] bone-capemgr bone_capemgr.9: part_number 'BB-BONE-PRU-01', version 'N/A'
    [  216.579067] bone-capemgr bone_capemgr.9: slot #9: generic override
    [  216.579112] bone-capemgr bone_capemgr.9: bone: Using override eeprom data at slot 9
    [  216.579159] bone-capemgr bone_capemgr.9: slot #9: 'Override Board Name,00A0,Override Manuf,BB-BONE-PRU-01'
    [  216.579411] bone-capemgr bone_capemgr.9: slot #9: Requesting part number/version based 'BB-BONE-PRU-01-00A0.dtbo
    [  216.579459] bone-capemgr bone_capemgr.9: slot #9: Requesting firmware 'BB-BONE-PRU-01-00A0.dtbo' for board-name 'Override Board Name', version '00A0'
    [  216.579519] bone-capemgr bone_capemgr.9: slot #9: dtbo 'BB-BONE-PRU-01-00A0.dtbo' loaded; converting to live tree
    [  216.586296] bone-capemgr bone_capemgr.9: slot #9: #2 overlays
    [  216.608605] omap_hwmod: pruss: failed to hardreset
    [  216.625429] bone-capemgr bone_capemgr.9: slot #9: Applied #2 overlays.

The "failed to hardreset" is mildly alarming, but afterwards, uio_pruss appears in lsmod and the first example works:

    sudo src/examples/1
     FA40 EE00 EF20 E700 DC70 D680 DA30 EEF0
     F3B0 EE50 EF30 E680 DB60 D8B0 D690 EE90
     F390 EE00 EF10 E780 DF40 D950 D760 EEC0
     F3B0 EEB0 EF90 E7A0 DF00 DE00 D800 EEB0
     F350 EE90 EF80 E710 DDB0 DCE0 D7D0 EEC0
     F350 EEB0 EFF0 E7C0 DE30 D8C0 D900 EEC0
     F400 EE50 EF30 E6B0 DD50 D970 D8D0 EF00
     F3E0 EE50 EEF0 E720 DEC0 DD00 DA40 EEB0
     F370 EE30 EF20 E720 DC60 DC30 DA10 EEA0
     F3C0 EDC0 EF20 E730 E020 DB50 D780 EEC0
     F3A0 ED90 EF10 E800 DF80 DD70 D6E0 EEF0
     F340 EE90 EFD0 E730 DD10 DA30 D7F0 EEB0
     F3D0 EF20 F000 E710 DE10 DAF0 DA90 EED0

Useful links:

* http://users.freebasic-portal.de/tjf/Projekte/libpruio/doc/html/_cha_preparation.html
* http://hackaday.com/2015/02/16/library-upgrade-to-pru-gives-fast-io-on-beaglebone/ (see comments too)
* Summary of Device Tree syntax: http://www.devicetree.org/Device_Tree_Usage

### Different types of Device Tree files ###

* `.dts`: main source file for board-level info
* `.dtsi`: source file, include file, like C header, for platform-level info that is used on multiple boards
* `.dtb`: binary file output from `dtc`, the Device Tree compiler
* `.dtbo`: overlay binary file, fragment of hardware info that is added to main `.dtb`
