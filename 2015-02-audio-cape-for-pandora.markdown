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

Load cape.
    echo BB-BONE-AUDI-02 > /sys/devices/bone_capemgr*/slots

Then install `asound.state`

    wget https://raw.githubusercontent.com/CircuitCo/BeagleBone-Audio/files/asound.state
    mv ./asound.state /var/lib/alsa/asound.state

Then `aplay` works.

Then install pianobar. The version in the Wheezy repos has TLS problems, and the recommended fix is to build from source.

    git clone https://github.com/PromyLOPh/pianobar.git
