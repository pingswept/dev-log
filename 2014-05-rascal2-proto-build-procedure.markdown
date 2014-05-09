### Overall plan ###

* Use one host to build a package group of Rascal-specific binary packages.
* Upload those packages to http://rascalmicro.com/repos/rascal-base
* On a target host, install Arch Linux ARM
* On the same target host, use pacman to download and install the package group.

### Setting up the build host ###

* Create PKGBUILDS for packages.
* Set groups array in each PKGBUILD: `groups=('rascal-base')`
* `makepkg` for all PKGBUILDS
* Use repo-add to generate repo database: `repo-add /path/to/repo.db.tar.gz /path/to/*.pkg.tar.xz`
* Upload repo database and .pkg.tar.xz files to repo server

### Setting up the target ###

* Download `ArchLinuxARM-YYYY.MM-am33x-rootfs.tar.gz` from http://os.archlinuxarm.org/os/omap/
* Download `Beaglebone-bootloader.tar.gz` from the same directory.
* Burn to SD card, as described on Installation tab here: http://archlinuxarm.org/platforms/armv7/ti/beaglebone-black
* Put card in target and boot, holding down S2 to force SD card boot.
* Change hostname?
* Add repo to `/etc/pacman.conf`

Like this:

    [repo-name]
    Server = http://rascalmicro.com/repos/rascal-base
    SigLevel = Optional

(Change SigLevel to Required later.)

* `pacman -S rascal-base`

This will include (tentatively):
* blinkm
* demos
* pytronics
* red


Install a bunch of other packages (if needed?)

From the Arch Linux ARM tree: https://github.com/archlinuxarm/PKGBUILDs/tree/master

* core/binutils
* core/gcc
* extra/ffmpeg
* extra/imagemagick
* extra/opencv
* extra/python
* extra/ruby
 
Already in the Arch Linux repos (maybe already installed by default?)
* community/ack
* community/iotop

* core/curl
* core/gcc-fortran
* core/logrotate
* core/make
* core/openssh
* core/usbutils
* core/wireless-tools

* extra/avahi
* extra/git
* extra/htop
* extra/vim

* i2c-tools
* iperf 
* ipython
* libevent (for websockets?)
* lsof
* mtr
* nano
* nginx
* ntpdate
* perl
* sysstat
* usb-gadget-mode?
* uwsgi


Unknown source?

* openssh-sftp-server


        python-daemon \
        python-dev \
        python-flask \
        python-flask-login \
        python-gevent \
        python-greenlet \
        python-imaging \
        python-jinja2 \
        python-matplotlib \
        python-misc \
        python-modules \
        python-numpy \
        python-pip \
        python-pyserial \
        python-pytronics \
        python-simplejson \
        python-sqlalchemy \
        python-werkzeug \
