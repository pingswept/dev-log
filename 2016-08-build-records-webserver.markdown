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

As root:

    cd /etc/nginx/sites-enabled
    ln -s /etc/nginx/sites-available/records.newamericanpublicart.com records.newamericanpublicart.com
    rm default

Make `/etc/nginx/sites-available/records.newamericanpublicart.com`

    server {
        location / {
            root /var/www/records.newamericanpublicart.com;
        }
    }

Create directory for data

    cd /var/www    
    mkdir records.newamericanpublicart.com

All the data directories and the files contained within should be owned and writable only by `root`, but world-readable. This means file permissions of 755 for directories and 644 for files.

Add `build-napa-docs.sh` as a cronjob.

    crontab -e

Add the line:

    * * * * * root /home/pi/records/build-napa-docs.sh
