Based on http://elinux.org/BeagleBone_Black_Extracting_eMMC_contents and http://stackoverflow.com/questions/17834561/duplicating-identical-beaglebone-black-setups

### Prepare the card ###

* Download zipball from https://s3.amazonaws.com/beagle/beagleboneblack-save-emmc.zip onto a Linux laptop (might also work on OS X).

* Plug new or freshly-formatted-with-FAT32 SD card into laptop. On OS X, my cards usually show up as `/Volumes/NO NAME`.

* Unzip zipball to card: `unzip ./beagleboneblack-save-emmc.zip -d /Volumes/NO\ NAME`

* Make first (only) partition on card bootable using `fdisk`. (This part may not work on OS X, as `fdisk` appears to be different.)

On Linux, that would look like this:

    fdisk /dev/sdc
    a
    1
    w

Not sure how to do this on OS X. Something like: `sudo fdisk -e /dev/rdisk2`

Afterwards, the partition table should show `*` in the Boot column for the FAT32 partition on the card.

On OS X, that looks like this:

    sudo fdisk /dev/rdisk2
    Password:
    Disk: /dev/rdisk2	geometry: 945/128/63 [7626752 sectors]
    Signature: 0xAA55
             Starting       Ending
     #: id  cyl  hd sec -  cyl  hd sec [     start -       size]
    ------------------------------------------------------------------------
    *1: 0B    1   2   3 -  945  99  35 [      8192 -    7618560] Win95 FAT-32
     2: 00    0   0   0 -    0   0   0 [         0 -          0] unused      
     3: 00    0   0   0 -    0   0   0 [         0 -          0] unused      
     4: 00    0   0   0 -    0   0   0 [         0 -          0] unused 

Check that card contents look like this:

    -rwxrwxrwx@ 1 brandon  staff      100312 Sep 25  2013 MLO
    -rwxrwxrwx@ 1 brandon  staff       24884 Sep 26  2013 am335x-boneblack.dtb
    -rwxrwxrwx@ 1 brandon  staff         217 Sep 26  2013 autorun.sh
    -rwxrwxrwx@ 1 brandon  staff      379308 Sep 25  2013 u-boot.img
    -rwxrwxrwx@ 1 brandon  staff         236 Sep 26  2013 uEnv.txt
    -rwxrwxrwx@ 1 brandon  staff    11748096 Sep 26  2013 uImage

### Boot off of the card ###

Put the card in the BBB and cycle the power while holding down S2. Wait 10 or 20 seconds for the autorun.sh o the card to start. LED 0 (furthest from Ethernet jack) should start single-blinking slowly.

Wait 7 minutes or so. When the LED stops blinking, pull the power plug and then the card. The card should now contain a .img file of the eMMC contents, like this:

    ls -lh
    total 4218496
    -rwxrwxrwx  1 brandon  staff   2.0G Jan  1  2000 BeagleBoneBlack-eMMC-image-29698.img
    -rwxrwxrwx@ 1 brandon  staff    98K Sep 24  2013 MLO
    -rwxrwxrwx@ 1 brandon  staff    24K Sep 25  2013 am335x-boneblack.dtb
    -rwxrwxrwx@ 1 brandon  staff   217B Sep 26  2013 autorun.sh
    -rwxrwxrwx@ 1 brandon  staff   370K Sep 24  2013 u-boot.img
    -rwxrwxrwx@ 1 brandon  staff   236B Sep 26  2013 uEnv.txt
    -rwxrwxrwx@ 1 brandon  staff    11M Sep 26  2013 uImage

### Copying onto new board ###

Edit `autorun.sh` to be this:

    #!/bin/sh
    echo timer > /sys/class/leds/beaglebone\:green\:usr0/trigger 
    dd if=/mnt/BeagleBoneBlack-eMMC-image-29698.img of=/dev/mmcblk1 bs=16M
    UUID=$(/sbin/blkid -c /dev/null -s UUID -o value /dev/mmcblk1p2)
    mkdir -p /mnt
    mount /dev/mmcblk1p2 /mnt
    sed -i "s/^uuid=.*\$/uuid=$UUID/" /mnt/boot/uEnv.txt
    umount /mnt 
    sync
    echo default-on > /sys/class/leds/beaglebone\:green\:usr0/trigger

Be sure to update the image filename to be correct. Also, make sure the file is a .img, not .img.gz. Otherwise, you have to gunzip first.

If LED1 just comes on after a couple of seconds and then stays on forever, the problem is likely a bad filename, (so the copying fails and we jump to the end of the script).

Ran into problem:

    Running /dev/mmcblk0p1/autorun.sh...
    dd: writing '/dev/mmcblk1': No space left on device
    17+0 records in
    15+1 records out
    [   21.711487] EXT3-fs (mmcblk1p2): error: couldn't mount because of unsupported optional features (240)
    [   21.722737] EXT2-fs (mmcblk1p2): error: couldn't mount because of unsupported optional features (244)
    sed: bad option in substitution expression
    
    Welcome to Buildroot
    beaglebone login: root
    # df -h
    Filesystem                Size      Used Available Use% Mounted on
    devtmpfs                245.7M    245.7M         0 100% /dev
    tmpfs                   249.3M         0    249.3M   0% /dev/shm
    tmpfs                   249.3M     44.0K    249.3M   0% /tmp

### Notes about BBB kernels ###

 * "armv7" is for mainline omap3+ devices (BeagleBoard/PandaBoard)
 * "bone" which is specifically for the BeagleBone
 * "lpae" is Large Physical Address Extensions, only for Cortex A15
