## Overall plan ##

* Use one host to build a package group of Rascal-specific binary packages.
* Upload those packages to http://rascalmicro.com/repos/rascal-base
* On a target host, install Arch Linux ARM
* On the same target host, use pacman to download and install the package group.

## Subroutine: setting up Arch Linux on an SD card ##

From http://archlinuxarm.org/platforms/armv7/ti/beaglebone-black

Check in `dmesg` what device your SD card is. Unmount the card if it gets automounted.

    fdisk /dev/sdc

At the fdisk prompt, delete old partitions and create a new one:

Type `o`. This will clear out any partitions on the drive.

Type `p` to list partitions. There should be no partitions left.

Now type `n`, then `p` for primary, `1` for the first partition on the drive, `enter` to accept the default first sector, then `+64M` to set the size to 64MB.

Type `t` to change the partition type, then `e` to set it to W95 FAT16 (LBA).

Type `a` to set the bootable flag on the first partition.

Now type `n`, then `p` for primary, `2` for the second partition on the drive, and `enter` twice to select the default first and last sectors.

Write and exit by typing `w`.

That's the end of fdisk.

Create the FAT16 filesystem: `mkfs.vfat -F 16 /dev/sdc1`

Create the ext4 filesystem: `mkfs.ext4 /dev/sdc2`

Download the BeagleBone bootloader tarball and extract the files onto the first partition of the SD card. These files contain the bootloaders needed to load the kernel. The file MLO needs to be the first file put onto the FAT partition, and extracting the tarball as-is should do this for you. If you have problems getting to U-Boot, re-format and place the files manually.

    mkdir /media/boot
    mount /dev/sdc1 /media/boot
    tar xvf BeagleBone-bootloader.tar.gz -C /media/boot
    umount /media/boot

The tar line above will throw a bunch of errors about `Cannot change ownership to uid 1001, gid 1001`. This can be safely ignored; it just happens because the FAT16 filesystem doesn't have uid and gid settings, so you can't set them.
    
Download the root filesystem tarball and extract it (as root, not via sudo) to the ext3 partition on either the SD card or the USB drive. It is important to do this as root, as special files need to be created as part of the filesystem that can only be created by root.

    mkdir /media/root
    mount /dev/sdc2 /media/root
    tar xf ArchLinuxARM-am33x-latest.tar.gz -C /media/root
    umount /media/root

Then, do a few things to make stuff work better.

    pacman -S avahi nss-mdns
    systemctl restart dbus
    systemctl enable avahi-daemon.service

## Setting up the build host ##

* Create PKGBUILDS for packages.
* Set groups array in each PKGBUILD: `groups=('rascal-base')`
* `makepkg` for all PKGBUILDS
* Use repo-add to generate repo database: `repo-add /path/to/repo.db.tar.gz /path/to/*.pkg.tar.xz`
* Upload repo database and .pkg.tar.xz files to repo server

## Setting up the target ##

* Download `ArchLinuxARM-YYYY.MM-am33x-rootfs.tar.gz` from http://os.archlinuxarm.org/os/omap/
* Download `Beaglebone-bootloader.tar.gz` from the same directory.
* Burn to SD card, as described in subroutine above: "setting up Arch Linux on an SD card" 
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

### From the Arch Linux ARM tree ###

https://github.com/archlinuxarm/PKGBUILDs/tree/master

* core/binutils
* core/gcc
* extra/ffmpeg
* extra/imagemagick
* extra/opencv
* extra/python
* extra/ruby
 
### Already in the non-ARM Arch Linux repos ###
(maybe already installed by default?)

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
