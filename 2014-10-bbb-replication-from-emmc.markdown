Based on http://elinux.org/BeagleBone_Black_Extracting_eMMC_contents and http://stackoverflow.com/questions/17834561/duplicating-identical-beaglebone-black-setups

    # Download zipball from https://s3.amazonaws.com/beagle/beagleboneblack-save-emmc.zip onto a Linux laptop (might also work on OS X).

    # Plug new or freshly-formatted-with-FAT32 SD card into laptop.

    # Unzip zipball to card: `unzip ./beagleboneblack-save-emmc.zip -d /path/to/sd/card`

    # Make first (only) partition on card bootable using `fdisk`. (This part may not work on OS X, as `fdisk` appears to be different.) That would look like this.
    
    fdisk /dev/sdc
    a
    1
    w
    
    # Check that card contents look like this:

    -rwxrwxrwx@ 1 brandon  staff      100312 Sep 25  2013 MLO
    -rwxrwxrwx@ 1 brandon  staff       24884 Sep 26  2013 am335x-boneblack.dtb
    -rwxrwxrwx@ 1 brandon  staff         217 Sep 26  2013 autorun.sh
    -rwxrwxrwx@ 1 brandon  staff      379308 Sep 25  2013 u-boot.img
    -rwxrwxrwx@ 1 brandon  staff         236 Sep 26  2013 uEnv.txt
    -rwxrwxrwx@ 1 brandon  staff    11748096 Sep 26  2013 uImage
