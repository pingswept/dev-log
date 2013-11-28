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
