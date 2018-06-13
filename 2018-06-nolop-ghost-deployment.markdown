Create a 1 GB Linode running Ubuntu 18.04 LTS.

Default disk and swap choices (25344 MB ext4 and 256 MB swap, respectively)

SSH into the Linode as `root`.

    apt-get update
    apt-get upgrade
    apt-get install nginx nodejs npm mysql-server
    adduser bstafford
    usermod -aG sudo bstafford
    

Log in as normal user `bstafford`.

    sudo npm i -g ghost-cli
    sudo mkdir -p /var/www/ghost
    sudo chown bstafford:bstafford /var/www/ghost
    sudo chmod 775 /var/www/ghost
    cd /var/www/ghost
    ghost install


