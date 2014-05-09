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

** From the Arch Linux ARM tree **

https://github.com/archlinuxarm/PKGBUILDs/tree/master

* core/binutils
* core/gcc
* extra/ffmpeg
* extra/imagemagick
* extra/opencv
* extra/python
* extra/ruby
 
** Already in the non-ARM Arch Linux repos ** (maybe already installed by default?)

** From the community repo **
* community/ack
* community/i2c-tools
* community/iotop
* community/iperf
* community/ipython
* community/python-flask
* community/python2-gevent (need Python 3 port?)
* community/python-greenlet
* community/python-jinja2
* community/python-markdown
* community/python-matplotlib
* community/python-pillow
* community/python-pyserial
* community/python-scipy
* community/python-simplejson
* community/python-sqlalchemy
* community/python-werkzeug
* community/sysstat
* community/uwsgi
* community/uwsgi-plugin-python (or python2?)

** From the core repo **
* core/curl
* core/gcc-fortran
* core/libevent (for websockets?)
* core/logrotate
* core/make
* core/nano
* core/openssh
* core/perl
* core/usbutils
* core/wireless-tools

** From the extra repo **
* extra/avahi
* extra/git
* extra/gpsd
* extra/htop
* extra/lsof
* extra/mtr
* extra/nginx
* extra/nss-mdns
* extra/ntp
* extra/python-numpy
* extra/python-pip
* extra/vim
* extra/zsh

** From Arch User Repository **
* python-daemon
* python-flask-login

Unknown source, maybe part of an existing package?

* openssh-sftp-server?
* usb-gadget-mode?
