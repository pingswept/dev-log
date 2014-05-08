### Overall plan ###

* Use one host to build a package group of Rascal-specific binary packages.
* Upload those packages to http://rascalmicro.com/repos/rascal-base
* On a target host, install Arch Linux ARM
* On the same target host, use pacman to download and install the package group.

### Setting up the build host ###

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
