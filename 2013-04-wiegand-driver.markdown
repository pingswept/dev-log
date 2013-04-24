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

### Monitoring /sys/kernel/wiegand/read with udev ###

    [root@rascal14:~]: opkg install udev-dev
    Installing udev-dev (151-r25.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/udev-dev_151-r25.6_armv5te.ipk.
    *** lots of errors snipped ***
    Collected errors:
     * check_data_file_clashes: Package libgcc-s-dev wants to install file /usr/lib/libgcc_s.so
        But that file is already provided by package  * libgcc-dev
     * opkg_install_cmd: Cannot install package udev-dev.
    [root@rascal14:~]: opkg remove libgcc-dev
    No packages removed.
    Collected errors:
     * print_dependents_warning: Package libgcc-dev is depended upon by packages:
     * print_dependents_warning: 	python-dev
     * print_dependents_warning: 	openssl-dev
     * print_dependents_warning: 	tcl-dev
     * print_dependents_warning: 	libglib-2.0-dev
     * print_dependents_warning: 	gettext-dev
     * print_dependents_warning: 	libsqlite3-dev
     * print_dependents_warning: These might cease to work if package libgcc-dev is removed.
    
     * print_dependents_warning: Force removal of this package with --force-depends.
     * print_dependents_warning: Force removal of this package and its dependents
     * print_dependents_warning: with --force-removal-of-dependent-packages.
    [root@rascal14:~]: opkg remove --force-depends libgcc-dev
    Removing package libgcc-dev from root...
    [root@rascal14:~]: opkg install udev-dev
    Installing udev-dev (151-r25.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/udev-dev_151-r25.6_armv5te.ipk.
    libattr-dev: unsatisfied recommendation for virtual-locale-fr-dev
    libattr-dev: unsatisfied recommendation for virtual-locale-de-dev
    libattr-dev: unsatisfied recommendation for virtual-locale-es-dev
    libattr-dev: unsatisfied recommendation for virtual-locale-gl-dev
    libattr-dev: unsatisfied recommendation for virtual-locale-nl-dev
    libattr-dev: unsatisfied recommendation for virtual-locale-pl-dev
    libattr-dev: unsatisfied recommendation for virtual-locale-sv-dev
    libattr-dev: unsatisfied recommendation for virtual-locale-cs-dev
    libacl-dev: unsatisfied recommendation for virtual-locale-fr-dev
    libacl-dev: unsatisfied recommendation for virtual-locale-de-dev
    libacl-dev: unsatisfied recommendation for virtual-locale-es-dev
    libacl-dev: unsatisfied recommendation for virtual-locale-gl-dev
    libacl-dev: unsatisfied recommendation for virtual-locale-pl-dev
    libacl-dev: unsatisfied recommendation for virtual-locale-sv-dev
    udev-dev: unsatisfied recommendation for libudev-dev
    udev-dev: unsatisfied recommendation for gobject-2.0-dev
    udev-dev: unsatisfied recommendation for gthread-2.0-dev
    udev-dev: unsatisfied recommendation for module-init-tools-depmod-dev
    udev-dev: unsatisfied recommendation for udev-utils-dev
    udev-dev: unsatisfied recommendation for libgudev-dev
    Installing usbutils-dev (0.86-r1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/usbutils-dev_0.86-r1.6_armv5te.ipk.
    Installing libacl-dev (2.2.49-r5.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libacl-dev_2.2.49-r5.6_armv5te.ipk.
    Installing libattr-dev (2.4.44-r4.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libattr-dev_2.4.44-r4.6_armv5te.ipk.
    Installing update-rc.d-dev (0.7-r2.6) to root...
    Downloading http://rascalmicro.com/packages/beta/all/update-rc.d-dev_0.7-r2.6_all.ipk.
    Configuring update-rc.d-dev.
    Configuring usbutils-dev.
    Configuring libattr-dev.
    Configuring libacl-dev.
    Configuring udev-dev.

But then we get a GCC error:

    [root@rascal14:~]: gcc -Wall -g -o udev_example udev_example.c -ludev
    /usr/lib/gcc/arm-angstrom-linux-gnueabi/4.3.3/../../../../arm-angstrom-linux-gnueabi/bin/ld: cannot find -lgcc_s
    collect2: ld returned 1 exit status

Strangely, this can be fixed by reinstalling libgcc-dev.

    [root@rascal14:~]: opkg install libgcc-dev
    Installing libgcc-dev (4.3.3-r23.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgcc-dev_4.3.3-r23.1.6_armv5te.ipk.
    Configuring libgcc-dev.
    [root@rascal14:~]: gcc -Wall -g -o udev_example udev_example.c -ludev

Compilation then succeeds and the example (from http://www.signal11.us/oss/udev/ ) runs.
