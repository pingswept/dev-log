    unzip Downloads/2016-05-27-raspbian-jessie-lite.zip
    sudo dd bs=4M if=2016-05-27-raspbian-jessie-lite.img of=/dev/sdc

Use `sudo raspi-config` to expand filesystem, set password, and set hostname.

After reboot, log back in.

    git clone https://github.com/NewAmericanPublicArt/records.git
