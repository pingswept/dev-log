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

Then, connect via FTDI serial cable do a few things to make stuff work better.

    screen /dev/tty.usbserial-A4013ENA 115200

Log in with credentials `root`/`root`.

Set root password.

    passwd

Update packages (will update to 3.8.13-22 kernel (or newer?))

    [root@alarm ~]# pacman -Syu
    :: Synchronizing package databases...
     core                     194.0 KiB   980K/s 00:00 [######################] 100%
     extra                      2.0 MiB  2.11M/s 00:01 [######################] 100%
     community                  2.3 MiB  1816K/s 00:01 [######################] 100%
     alarm                     58.0 KiB  3.33M/s 00:00 [######################] 100%
     aur                       63.3 KiB  3.25M/s 00:00 [######################] 100%
    :: Starting full system upgrade...
    resolving dependencies...
    looking for inter-conflicts...
    
    Packages (9): device-mapper-2.02.106-2  gawk-4.1.1-1
                  linux-am33x-legacy-3.8.13-22  linux-api-headers-3.14.1-1
                  lvm2-2.02.106-2  man-pages-3.66-1  ntp-4.2.7.p441-1
                  pacman-4.1.2-6  psmisc-22.21-2
    
    Total Download Size:    25.68 MiB
    Total Installed Size:   41.16 MiB
    Net Upgrade Size:       2.60 MiB
    
    :: Proceed with installation? [Y/n] Y
    :: Retrieving packages ...
     device-mapper-2.02....   162.3 KiB   854K/s 00:00 [######################] 100%
     gawk-4.1.1-1-armv7h      865.3 KiB  2.07M/s 00:00 [######################] 100%
     linux-am33x-legacy-...    15.9 MiB  2.21M/s 00:07 [######################] 100%
     linux-api-headers-3...   701.6 KiB   833K/s 00:01 [######################] 100%
     lvm2-2.02.106-2-armv7h   740.2 KiB   386K/s 00:02 [######################] 100%
     man-pages-3.66-1-any       5.2 MiB  1014K/s 00:05 [######################] 100%
     pacman-4.1.2-6-armv7h    561.4 KiB   428K/s 00:01 [######################] 100%
     psmisc-22.21-2-armv7h     98.8 KiB   365K/s 00:00 [######################] 100%
     ntp-4.2.7.p441-1-armv7h 1618.1 KiB   530K/s 00:03 [######################] 100%
     (9/9) checking keys in keyring                     [######################] 100%
     (9/9) checking package integrity                   [######################] 100%
     (9/9) loading package files                        [######################] 100%
     (9/9) checking for file conflicts                  [######################] 100%
     (9/9) checking available disk space                [######################] 100%
     (1/9) upgrading device-mapper                      [######################] 100%
     (2/9) upgrading gawk                               [######################] 100%
     (3/9) upgrading linux-am33x-legacy                 [######################] 100%
     >>> Updating module dependencies. Please wait ...
     (4/9) upgrading linux-api-headers                  [######################] 100%
     (5/9) upgrading lvm2                               [######################] 100%
     (6/9) upgrading man-pages                          [######################] 100%
     (7/9) upgrading ntp                                [######################] 100%
     (8/9) upgrading pacman                             [######################] 100%
     (9/9) upgrading psmisc                             [######################] 100%
     synchronizing filesystem...

Note: the upgrade above failed on the first attempt. It seems that what happened was that the SD card timed out while trying to write the new kernel, which led to a kernel panic. On the second attempt, it worked. The only difference I could find was that I upgraded Avahi (as below) before doing the full system upgrade.

Successfully rebooted with new kernel.

Install Avahi.

    pacman -S avahi nss-mdns
    systemctl restart dbus
    systemctl enable avahi-daemon.service

Avahi won't start successfully

Edit /etc/avahi/avahi-daemon.conf from:
    #use-iff-running=no enable-dbus=yes disallow-other-stacks=no
to:
    #use-iff-running=no enable-dbus=yes                                             
    disallow-other-stacks=yes

Still fails on startup.

Change the hostname.

    nano /etc/hostname

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

* core/binutils (gcc depends on this)
* core/gcc
* extra/python
* extra/ruby

Packages with a weird mesagl dependency? Also one requires X11 stuff.
* extra/ffmpeg
* extra/imagemagick
* extra/opencv
 
### Already in the non-ARM Arch Linux repos ###
(maybe already installed by default?)

** From the community repo **
* community/ack
* community/i2c-tools (ACCH! Depends on Python 2!)
* community/iotop (ACCH! Depends on Python 2! But has been ported to Python 3.)
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

Already installed by default
* core/curl
* core/logrotate
* core/nano
* core/openssh
* core/perl
* core/usbutils

Need to be installed
* core/gcc-fortran
* core/make
* core/libevent (for websockets. Optional dep: "python2: to use event_rpcgen.py")
* core/wireless_tools (note the underscore)

** From the extra repo **
* extra/avahi
* extra/git
* extra/gpsd (ACCH! depends on Python 2)
* extra/htop
* extra/lsof
* extra/mtr
* extra/nginx
* extra/nss-mdns
* extra/ntp (installed by pacman -Syu ?)
* extra/python-numpy
* extra/python-pip
* extra/vim
* extra/zsh

** From Arch User Repository **
* python2-daemon (need to figure out how to install existing Py3k port of Supervisor, then fix Rascal docs) 
* python-flask-login

Unknown source, maybe part of an existing package?

* openssh-sftp-server?
* usb-gadget-mode?

### Problem while installing packages ###

    synchronizing filesystem...
    [ 6780.501496] INFO: task pacman:424 blocked for more than 60 seconds.
    [ 6780.508121] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
    [ 6780.516520] Kernel panic - not syncing: hung_task: blocked tasks
    [ 6780.522811] [<c00132d8>] (unwind_backtrace+0x0/0xe0) from [<c05f4070>] (panic+0x84/0x1d4)
    [ 6780.531368] [<c05f4070>] (panic+0x84/0x1d4) from [<c0093058>] (watchdog+0x210/0x248)
    [ 6780.539479] [<c0093058>] (watchdog+0x210/0x248) from [<c00599b0>] (kthread+0xa0/0xac)
    [ 6780.547670] [<c00599b0>] (kthread+0xa0/0xac) from [<c000d6f8>] (ret_from_fork+0x14/0x3c)
    [ 6780.556131] drm_kms_helper: panic occurred, switching back to text console

Results in:

    Booting from mmc ...
    Wrong Image Format for bootm command
    ERROR: can't get kernel image!
    U-Boot# 

Put the SD card in USB adapter on Ubuntu laptop, and it mounted fine. Then rebooted, and whatever went wrong is now fixed. Hmmmmm.
