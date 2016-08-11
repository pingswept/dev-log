    unzip Downloads/2016-05-27-raspbian-jessie-lite.zip
    sudo dd bs=4M if=2016-05-27-raspbian-jessie-lite.img of=/dev/sdc

Use `sudo raspi-config` to expand filesystem, set password, set locale, and set hostname.

After reboot, log back in.

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install git nginx python3-pip vim
    sudo pip3 install mkdocs
    sudo pip3 install mkdocs-cinder
    git clone https://github.com/NewAmericanPublicArt/records.git
    
