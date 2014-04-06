Using SD card with 2013-09-05 Angstrom image.
Added to uEnv.txt via USB device filesystem

    capemgr.disable_partno=BB-BONE-EMMC-2G

Successful boot with 3.1 MP camera cape, rev A2

    U-Boot SPL 2013.04-dirty (Jul 10 2013 - 14:02:53)
    musb-hdrc: ConfigData=0xde (UTMI-8, dyn FIFOs, HB-ISO Rx, HB-ISO Tx, SoftConn)
    musb-hdrc: MHDRC RTL version 2.0 
    musb-hdrc: setup fifo_mode 4
    musb-hdrc: 28/31 max ep, 16384/16384 memory
    USB Peripheral mode controller at 47401000 using PIO, IRQ 0
    musb-hdrc: ConfigData=0xde (UTMI-8, dyn FIFOs, HB-ISO Rx, HB-ISO Tx, SoftConn)
    musb-hdrc: MHDRC RTL version 2.0 
    musb-hdrc: setup fifo_mode 4
    musb-hdrc: 28/31 max ep, 16384/16384 memory
    USB Host mode controller at 47401800 using PIO, IRQ 0
    OMAP SD/MMC: 0
    reading u-boot.img
    reading u-boot.img
    
    
    U-Boot 2013.04-dirty (Jul 10 2013 - 14:02:53)
    
    I2C:   ready
    DRAM:  512 MiB
    WARNING: Caches not enabled
    NAND:  No NAND device found!!!
    0 MiB
    MMC:   OMAP SD/MMC: 0, OMAP SD/MMC: 1
    *** Warning - readenv() failed, using default environment
    
    musb-hdrc: ConfigData=0xde (UTMI-8, dyn FIFOs, HB-ISO Rx, HB-ISO Tx, SoftConn)
    musb-hdrc: MHDRC RTL version 2.0 
    musb-hdrc: setup fifo_mode 4
    musb-hdrc: 28/31 max ep, 16384/16384 memory
    USB Peripheral mode controller at 47401000 using PIO, IRQ 0
    musb-hdrc: ConfigData=0xde (UTMI-8, dyn FIFOs, HB-ISO Rx, HB-ISO Tx, SoftConn)
    musb-hdrc: MHDRC RTL version 2.0 
    musb-hdrc: setup fifo_mode 4
    musb-hdrc: 28/31 max ep, 16384/16384 memory
    USB Host mode controller at 47401800 using PIO, IRQ 0
    Net:   <ethaddr> not set. Validating first E-fuse MAC
    cpsw, usb_ether
    Hit any key to stop autoboot:  0 
    gpio: pin 53 (gpio 53) value is 1
    mmc0 is current device
    micro SD card found
    mmc0 is current device
    gpio: pin 54 (gpio 54) value is 1
    SD/MMC found on device 0
    reading uEnv.txt
    53 bytes read in 3 ms (16.6 KiB/s)
    Loaded environment from uEnv.txt
    Importing environment from mmc ...
    gpio: pin 55 (gpio 55) value is 1
    4385024 bytes read in 804 ms (5.2 MiB/s)
    gpio: pin 56 (gpio 56) value is 1
    24808 bytes read in 77 ms (314.5 KiB/s)
    Booting from mmc ...
    ## Booting kernel from Legacy Image at 80007fc0 ...
       Image Name:   Angstrom/3.8.13/beaglebone
       Image Type:   ARM Linux Kernel Image (uncompressed)
       Data Size:    4384960 Bytes = 4.2 MiB
       Load Address: 80008000
       Entry Point:  80008000
       Verifying Checksum ... OK
    ## Flattened Device Tree blob at 80f80000
       Booting using the fdt blob at 0x80f80000
       XIP Kernel Image ... OK
    OK
       Using Device Tree in place at 80f80000, end 80f890e7
    
    Starting kernel ...
    
    Uncompressing Linux... done, booting the kernel.
    [    0.196426] omap2_mbox_probe: platform not supported
    [    0.207015] tps65217-bl tps65217-bl: no platform data provided
    [    0.283535] bone-capemgr bone_capemgr.8: slot #0: No cape found
    [    0.320642] bone-capemgr bone_capemgr.8: slot #1: No cape found
    [    0.381188] bone-capemgr bone_capemgr.8: slot #3: No cape found
    [    0.404874] bone-capemgr bone_capemgr.8: slot #6: BB-BONELT-HDMIN conflict P8.45 (#5:BB-BONELT-HDMI)
    [    0.414471] bone-capemgr bone_capemgr.8: slot #6: Failed verification
    [    0.435713] bone-capemgr bone_capemgr.8: loader: failed to load slot-6 BB-BONELT-HDMIN:00A0 (prio 2)
    [    0.551835] cssp-camera 18000000.camera: Loaded OK.
    [    0.560268] omap_hsmmc mmc.4: of_parse_phandle_with_args of 'reset' failed
    [    0.596660] pinctrl-single 44e10800.pinmux: pin 44e10854 already requested by 44e10800.pinmux; cannot claim for gpio-leds.7
    [    0.608399] pinctrl-single 44e10800.pinmux: pin-21 (gpio-leds.7) status -22
    [    0.615711] pinctrl-single 44e10800.pinmux: could not request pin 21 on device pinctrl-single
    systemd-fsck[84]: Angstrom: clean, 50486/218160 files, 314481/872448 blocks
    
    .---O---.                                           
    |       |                  .-.           o o        
    |   |   |-----.-----.-----.| |   .----..-----.-----.
    |       |     | __  |  ---'| '--.|  .-'|     |     |
    |   |   |  |  |     |---  ||  --'|  |  |  '  | | | |
    '---'---'--'--'--.  |-----''----''--'  '-----'-'-'-'
                    -'  |
                    '---'
    
    The Angstrom Distribution beaglebone ttyO0
    
    Angstrom v2012.12 - Kernel 3.8.13
    
    beaglebone login: [   24.245047] libphy: PHY 4a101000.mdio:01 not found
    [   24.250093] net eth0: phy 4a101000.mdio:01 not found on slave 1
    root
    Last login: Sat Jan  1 00:00:26 UTC 2000 on ttyO0
    root@beaglebone:~# ls
    Desktop  this-is-the-sd-card-filesystem
    root@beaglebone:~#

Camera cape is loaded

    root@beaglebone:~# cat /sys/devices/bone_capemgr.*/slots
     0: 54:PF--- 
     1: 55:PF--- 
     2: 56:P---L BeagleBone 3.1MP CAMERA CAPE,00A2,Beagleboardtoys,BB-BONE-CAM3-01
     3: 57:PF--- 
     4: ff:P-O-- Bone-LT-eMMC-2G,00A0,Texas Instrument,BB-BONE-EMMC-2G
     5: ff:P-O-L Bone-Black-HDMI,00A0,Texas Instrument,BB-BONELT-HDMI

Image cleaner from https://gist.github.com/lelandbatey/8677901

    #!/bin/bash
    convert $1 -morphology Convolve DoG:15,100,0 -negate -normalize -blur 0x1 -channel RBG -level 60%,91%,0.1 $2

In order to compile fswebcam, need GD library installed.

    opkg install gd gd-dev

Works, but reports

    gd-dev: unsatisfied recommendation for jpeg-dev

This could be a problem, as fswebcam claims: "Its only requirements are that the GD library be installed with JPEG, PNG
and FreeType support."

### fswebcam build log ###

(Had to make configure executable with chmod first.)

    root@beaglebone:~/fswebcam# ./configure --prefix=/usr
    checking for gcc... gcc
    checking whether the C compiler works... yes
    checking for C compiler default output file name... a.out
    checking for suffix of executables... 
    checking whether we are cross compiling... no
    checking for suffix of object files... o
    checking whether we are using the GNU C compiler... yes
    checking whether gcc accepts -g... yes
    checking for gcc option to accept ISO C89... none needed
    checking how to run the C preprocessor... gcc -E
    checking for grep that handles long lines and -e... /bin/grep
    checking for egrep... /bin/grep -E
    checking for ANSI C header files... yes
    checking for sys/types.h... yes
    checking for sys/stat.h... yes
    checking for stdlib.h... yes
    checking for string.h... yes
    checking for memory.h... yes
    checking for strings.h... yes
    checking for inttypes.h... yes
    checking for stdint.h... yes
    checking for unistd.h... yes
    checking for stdlib.h... (cached) yes
    checking for unistd.h... (cached) yes
    checking for sys/param.h... yes
    checking for getpagesize... yes
    checking for working mmap... yes
    checking for gdImageCreateTrueColor in -lgd... yes
    checking for gdImageStringFT in -lgd... yes
    checking for gdImageJpeg in -lgd... yes
    checking for gdImagePngEx in -lgd... yes
    
       Buffer type ........... 16 bit
       PNG support ........... yes
       JPEG support .......... yes
       Freetype 2.x support .. yes
       V4L1 support .......... yes
       V4L2 support .......... yes
    
    configure: creating ./config.status
    config.status: creating Makefile
    config.status: creating config.h
    root@beaglebone:~/fswebcam# make
    gcc -g -O2 -DHAVE_CONFIG_H -c fswebcam.c -o fswebcam.o
    gcc -g -O2 -DHAVE_CONFIG_H -c log.c -o log.o
    gcc -g -O2 -DHAVE_CONFIG_H -c effects.c -o effects.o
    gcc -g -O2 -DHAVE_CONFIG_H -c parse.c -o parse.o
    gcc -g -O2 -DHAVE_CONFIG_H -c src.c -o src.o
    gcc -g -O2 -DHAVE_CONFIG_H -c src_test.c -o src_test.o
    gcc -g -O2 -DHAVE_CONFIG_H -c src_raw.c -o src_raw.o
    gcc -g -O2 -DHAVE_CONFIG_H -c src_file.c -o src_file.o
    gcc -g -O2 -DHAVE_CONFIG_H -c src_v4l1.c -o src_v4l1.o
    gcc -g -O2 -DHAVE_CONFIG_H -c src_v4l2.c -o src_v4l2.o
    gcc -g -O2 -DHAVE_CONFIG_H -c dec_rgb.c -o dec_rgb.o
    gcc -g -O2 -DHAVE_CONFIG_H -c dec_yuv.c -o dec_yuv.o
    gcc -g -O2 -DHAVE_CONFIG_H -c dec_grey.c -o dec_grey.o
    gcc -g -O2 -DHAVE_CONFIG_H -c dec_bayer.c -o dec_bayer.o
    gcc -g -O2 -DHAVE_CONFIG_H -c dec_jpeg.c -o dec_jpeg.o
    gcc -g -O2 -DHAVE_CONFIG_H -c dec_png.c -o dec_png.o
    gcc -g -O2 -DHAVE_CONFIG_H -c dec_s561.c -o dec_s561.o
    gcc -o fswebcam fswebcam.o log.o effects.o parse.o src.o src_test.o src_raw.o src_file.o src_v4l1.o src_v4l2.o dec_rgb.o dec_yuv.o dec_grey.o dec_bayer.o dec_jpeg.o dec_png.o dec_s561.o -lgd 
    gzip -c --best fswebcam.1 > fswebcam.1.gz
    root@beaglebone:~/fswebcam# make install
    mkdir -p /usr/bin
    mkdir -p /usr/share/man/man1
    install -m 755 fswebcam /usr/bin
    install -m 644 fswebcam.1.gz /usr/share/man/man1
