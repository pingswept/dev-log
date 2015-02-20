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

Install a bunch of prereqs. `Gmake` is GNU make, which is already installed.

    apt-get install libgnutls-dev
    apt-get install libgnutls26
    apt-get install libgcrypt11-dev ligcrypt11
    apt-get install gnutls-bin
    apt-get install libjson0
    apt-get install libavfilter-dev
    apt-get install libao-dev

Then `make` fails with:

    root@rascal-pink:~/pianobar# make clean
    CLEAN
    root@rascal-pink:~/pianobar# make
        CC  src/main.c
        CC  src/player.c
    src/player.c:40:36: fatal error: libavfilter/buffersink.h: No such file or directory

Unfortunately, the more recent versions of pianobar don't work with libav 0.8, so this is a dead end on Wheezy. See https://github.com/PromyLOPh/pianobar/issues/444#issuecomment-41953117

On the BBB Jessie 2015-02-15 image, pianobar installs via apt and seems to run well, except that the audio shield may not work because maybe capemgr doesn't exist any more.

Next: trying to get the audio cape working as with Wheezy.
