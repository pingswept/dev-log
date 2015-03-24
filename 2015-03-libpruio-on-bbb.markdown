### Installing libpruio ###

    wget http://www.freebasic-portal.de/dlfiles/625/freebasic_1.01.0debian7_armhf.deb
    dpkg --install freebasic_1.01.0debian7_armhf.deb

The `dpkg` line will complain about missing `libxpm-dev`

This fixes the missing dependency: `apt-get -f install`
