### Installing Prose.io on Ubuntu 12.04 (Precise Pangolin) ###

    apt-get install ruby rubygems ruby-liquid python-pygments ruby1.8-dev
    gem install jekyll

    git clone git@github.com:prose/prose.git
    
Register local copy of Prose as an OAuth application with Github here: https://github.com/settings/applications/new

Install Node.JS to run Gatekeeper.

    apt-get install nodejs npm
    npm install express
    
    git clone git@github.com:prose/gatekeeper.git
    cd gatekeeper
    node server.js

Add client ID and secret from OAuth app registration to gatekeeper/config.json.

### Making Wiegand protocol Linux kernel driver work with the Rascal ###

Forked Mark Jason Anderson's repo on Github, patched to change kernel directory, switch to pins that the Rascal connects to Arduino headers.

Forked repo: https://github.com/rascalmicro/wiegand-linux

Pinout is now:
    DATA0:     pin 2 (PB7)
    DATA1:     pin 3 (PB6)
    green LED: pin 4 (PB9)
    unknown:   pin 5 (PB8)
    ground:    GND

Built on Amazon EC2 machine used for building Rascal kernel

    export CROSS_COMPILE=arm-linux-
    PATH=$PATH:/opt/eldk/usr/bin:/opt/eldk/bin:/opt/cs/bin
    make all

This results in the creation of wiegand-gpio.ko, a binary that can then be scp'd to the Rascal.

On the Rascal, the binary can be inserted into the running kernel with:

    insmod wiegand-gpio # assuming you're in the same directory as the driver

After this, the following shows up in dmesg:

    wiegand intialising
    wiegand ready

To make the kernel load on boot, the binary should be moved to /lib/modules/2.6.36\+/ and the full filename, wiegand-gpio.ko, added to /etc/modules.
