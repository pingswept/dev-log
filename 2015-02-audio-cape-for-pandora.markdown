Mostly from http://elinux.org/BBB_Audio_Cape_RevB_Getting_Started

    root@beaglebone:~# wget http://elinux.org/images/1/10/BB-BONE-AUDI-02-00A0.zip
    root@beaglebone:~# unzip BB-BONE-AUDI-02-00A0.zip
    root@beaglebone:~# apt-get update
    root@beaglebone:~# apt-get install unzip

Compile the device tree file and move it to /lib/firmware:

   root@beaglebone:~# dtc -O dtb -o BB-BONE-AUDI-02-00A0.dtbo -b 0 -@ BB-BONE-AUDI-02-00A0.dts
   root@beaglebone:~# mv BB-BONE-AUDI-02-00A0.dtbo /lib/firmware

Uncomment in `/boot/uboot/uEnv.txt`:
    optargs=capemgr.disable_partno=BB-BONELT-HDMI,BB-BONELT-HDMIN

Reboot
