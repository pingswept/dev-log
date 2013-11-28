Power consumption of Cubietruck out of the box measured at 2 W, or 0.03 A @ 121.6 V AC. Probably only accurate +/-1 W, or maybe more.

Load bootloaders and filesystem image onto microSD card,
based on http://docs.cubieboard.org/zh/tutorials/ct1/installation/install_lubuntu_desktop_server_to_sd_card

    card=/dev/sde                                             
    ➜  ~  sudo dd if=/dev/zero of=${card} bs=1024 seek=544 count=128
    128+0 records in
    128+0 records out
    131072 bytes (131 kB) copied, 0.023906 s, 5.5 MB/s
    ➜  ~  sudo dd if=u-boot-sunxi-with-spl-ct-20131102.bin of=$card bs=1024 seek=8
    254+1 records in
    254+1 records out
    260260 bytes (260 kB) copied, 0.0230326 s, 11.3 MB/s
    ➜  ~  sudo fdisk /dev/sde

In fdisk, delete existing partition, then add two new partitions.
    
    Command (m for help): d
    Selected partition 1
    
    Command (m for help): n
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-7626751, default 2048): 2048
    Last sector, +sectors or +size{K,M,G} (2048-7626751, default 7626751): 133119
    
    Command (m for help): n
    Partition type:
       p   primary (1 primary, 0 extended, 3 free)
       e   extended
    Select (default p): 
    Using default response p
    Partition number (1-4, default 2): 2
    First sector (133120-7626751, default 133120): 133120
    Last sector, +sectors or +size{K,M,G} (133120-7626751, default 7626751): 
    Using default value 7626751
    
    Command (m for help): p
    
    Disk /dev/sde: 3904 MB, 3904897024 bytes
    100 heads, 35 sectors/track, 2179 cylinders, total 7626752 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000
    
       Device Boot      Start         End      Blocks   Id  System
    /dev/sde1            2048      133119       65536   83  Linux
    /dev/sde2          133120     7626751     3746816   83  Linux

    Command (m for help): w
    The partition table has been altered!
    
    Calling ioctl() to re-read partition table.
    
    WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
    The kernel still uses the old table. The new table will be used at
    the next reboot or after you run partprobe(8) or kpartx(8)
    Syncing disks.

Strangely, despite the warning message, the fdisk stuff above seems to have worked.
