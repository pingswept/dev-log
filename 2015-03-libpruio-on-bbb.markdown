### Installing FreeBasic compiler ###

    wget http://www.freebasic-portal.de/dlfiles/625/freebasic_1.01.0debian7_armhf.deb
    dpkg --install freebasic_1.01.0debian7_armhf.deb

The `dpkg` line will complain about missing `libxpm-dev`

This fixes the missing dependency: `apt-get -f install`

### Install PRUSS driver kit ###

    wget http://www.freebasic-portal.de/dlfiles/539/FB_prussdrv-0.0.tar.bz2
    tar xjf FB_prussdrv-0.0.tar.bz2

Useful links:

* http://users.freebasic-portal.de/tjf/Projekte/libpruio/doc/html/_cha_preparation.html
* http://hackaday.com/2015/02/16/library-upgrade-to-pru-gives-fast-io-on-beaglebone/ (see comments too)
