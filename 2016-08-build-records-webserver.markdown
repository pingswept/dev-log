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

Check that `cron` is actually executing it properly:

    tail -f /var/log/syslog

Should see something like:

    Aug 13 19:33:01 chandler-www CRON[31032]: (root) CMD (root /home/pi/records/build-napa-docs.sh)
    Aug 13 19:33:01 chandler-www CRON[31028]: (CRON) info (No MTA installed, discarding output)

Add `index.html` to `/var/www/records.newamericanpublicart.com`

    <html>
    <head>
    </head>
    <body>
    <ul>
    <li><a href="blue-hour-baltimore">Blue Hour Baltimore</a></li>
    <li><a href="color-commons">Color Commons</a></li>
    <li><a href="ourself">Ourself</a></li>
    </ul>
    </body>
    </html>
