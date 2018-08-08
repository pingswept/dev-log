### Wordpress dev server on Tufts Redhat VM ###

Get Tufts to set up a VM running Redhat 7.5 and SSH in.

    Red Hat Enterprise Linux Server release 7.5 (Maipo)
    TAG   : ESAI                            installed 23 December 2015
    GROUP : prod                            patched  2 August 2018

    sudo yum update
    sudo yum install httpd mariadb mariadb-server php php-common php-mysql php-gd php-xml php-mbstring php-mcrypt
    
    =========================================================================================================================
    Package                           Arch            Version                     Repository                            Size
    =========================================================================================================================
    Installing:
     httpd                            x86_64          2.4.6-80.el7_5.1            rhel-7-server-rpms                   1.2 M
     mariadb                          x86_64          1:5.5.56-2.el7              rhel-7-server-rpms                   9.1 M
     mariadb-server                   x86_64          1:5.5.56-2.el7              rhel-7-server-rpms                    11 M
     php                              x86_64          5.4.16-45.el7               rhel-7-server-rpms                   1.4 M
     php-common                       x86_64          5.4.16-45.el7               rhel-7-server-rpms                   565 k
     php-gd                           x86_64          5.4.16-45.el7               rhel-7-server-rpms                   128 k
     php-mbstring                     x86_64          5.4.16-45.el7               rhel-7-server-optional-rpms          505 k
     php-mcrypt                       x86_64          5.4.16-7.el7                epel                                  20 k
     php-mysql                        x86_64          5.4.16-45.el7               rhel-7-server-rpms                   101 k
     php-xml                          x86_64          5.4.16-45.el7               rhel-7-server-rpms                   126 k
    Installing for dependencies:
     httpd-tools                      x86_64          2.4.6-80.el7_5.1            rhel-7-server-rpms                    89 k
     libX11                           x86_64          1.6.5-1.el7                 rhel-7-server-rpms                   606 k
     libX11-common                    noarch          1.6.5-1.el7                 rhel-7-server-rpms                   164 k
     libXau                           x86_64          1.0.8-2.1.el7               rhel-7-server-rpms                    29 k
     libXpm                           x86_64          3.5.12-1.el7                rhel-7-server-rpms                    56 k
     libmcrypt                        x86_64          2.5.8-13.el7                epel                                  99 k
     libpng                           x86_64          2:1.5.13-7.el7_2            rhel-7-server-rpms                   213 k
     libxcb                           x86_64          1.12-1.el7                  rhel-7-server-rpms                   211 k
     libzip                           x86_64          0.10.1-8.el7                rhel-7-server-rpms                    49 k
     mailcap                          noarch          2.1.41-2.el7                rhel-7-server-rpms                    31 k
     perl-Compress-Raw-Bzip2          x86_64          2.061-3.el7                 rhel-7-server-rpms                    32 k
     perl-Compress-Raw-Zlib           x86_64          1:2.061-4.el7               rhel-7-server-rpms                    57 k
     perl-DBD-MySQL                   x86_64          4.023-6.el7                 rhel-7-server-rpms                   140 k
     perl-DBI                         x86_64          1.627-4.el7                 rhel-7-server-rpms                   802 k
     perl-IO-Compress                 noarch          2.061-2.el7                 rhel-7-server-rpms                   260 k
     perl-Net-Daemon                  noarch          0.48-5.el7                  rhel-7-server-rpms                    51 k
     perl-PlRPC                       noarch          0.2020-14.el7               rhel-7-server-rpms                    36 k
     php-cli                          x86_64          5.4.16-45.el7               rhel-7-server-rpms                   2.7 M
     php-pdo                          x86_64          5.4.16-45.el7               rhel-7-server-rpms                    99 k
     t1lib                            x86_64          5.1.2-14.el7                rhel-7-server-rpms                   166 k

Set up a MariaDB database.

    [bstaff01@nolopwp-dev-01 ~]$ sudo systemctl start mariadb.service
    [bstaff01@nolopwp-dev-01 ~]$ sudo mysql_secure_installation

    NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
          SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

    In order to log into MariaDB to secure it, we'll need the current
    password for the root user.  If you've just installed MariaDB, and
    you haven't set the root password yet, the password will be blank,
    so you should just press enter here.

    Enter current password for root (enter for none):
    OK, successfully used password, moving on...

    Setting the root password ensures that nobody can log into the MariaDB
    root user without the proper authorisation.

    Set root password? [Y/n] y
    New password:
    Re-enter new password:
    Password updated successfully!
    Reloading privilege tables..
     ... Success!


    By default, a MariaDB installation has an anonymous user, allowing anyone
    to log into MariaDB without having to have a user account created for
    them.  This is intended only for testing, and to make the installation
    go a bit smoother.  You should remove them before moving into a
    production environment.

    Remove anonymous users? [Y/n] y
     ... Success!

    Normally, root should only be allowed to connect from 'localhost'.  This
    ensures that someone cannot guess at the root password from the network.

    Disallow root login remotely? [Y/n] y
     ... Success!

    By default, MariaDB comes with a database named 'test' that anyone can
    access.  This is also intended only for testing, and should be removed
    before moving into a production environment.

    Remove test database and access to it? [Y/n] y
     - Dropping test database...
     ... Success!
     - Removing privileges on test database...
     ... Success!

    Reloading the privilege tables will ensure that all changes made so far
    will take effect immediately.

    Reload privilege tables now? [Y/n] y
     ... Success!

    Cleaning up...

    All done!  If you've completed all of the above steps, your MariaDB
    installation should now be secure.

    Thanks for using MariaDB!

Install Wordpress

    ===================================================================================================================================================
    Package                                Arch                    Version                         Repository                                    Size ===================================================================================================================================================Installing:
     wordpress                              noarch                  4.9.6-1.el7                     epel                                         7.0 M Installing for dependencies:
     libc-client                            x86_64                  2007f-16.el7                    epel                                         562 k 
     php-IDNA_Convert                       noarch                  0.8.0-2.el7                     epel                                          36 k 
     php-PHPMailer                          noarch                  5.2.26-1.el7                    epel                                         135 k 
     php-fedora-autoloader                  noarch                  1.0.0-1.el7                     epel                                         9.6 k 
     php-getid3                             noarch                  1:1.9.15-1.el7                  epel                                         433 k 
     php-imap                               x86_64                  5.4.16-7.el7                    epel                                          38 k 
     php-intl                               x86_64                  5.4.16-45.el7                   rhel-7-server-optional-rpms                   97 k 
     php-process                            x86_64                  5.4.16-45.el7                   rhel-7-server-rpms                            56 k 
     php-simplepie                          noarch                  1.3.1-4.el7                     epel                                         463 k

But then realize the Yum package is living in `/usr/share/wordpress` and this is crazy.

So `yum remove wordpress`

Then

    cd /var/www/html
    sudo wget http://wordpress.org/latest.tar.gz
    sudo tar xzvf latest.tar.gz
    sudo mv ./wordpress/* .
    sudo rmdir wordpress
    sudo rm latest.tar.gz
    chown -R apache:apache /var/www/html/*

Create database for Wordpress.

    [bstaff01@nolopwp-dev-01 ~]$ mysql -u root -p
    Enter password:
    MariaDB [(none)]> CREATE USER wordpress@localhost IDENTIFIED BY "secret-password-goes-here";
    Query OK, 0 rows affected (0.00 sec)

    MariaDB [(none)]> CREATE DATABASE wordpress;
    Query OK, 1 row affected (0.00 sec)

    MariaDB [(none)]> GRANT ALL ON wordpress.* TO wordpress@localhost;
    Query OK, 0 rows affected (0.00 sec)

    MariaDB [(none)]> FLUSH PRIVILEGES;
    Query OK, 0 rows affected (0.00 sec)

    MariaDB [(none)]> exit;
    Bye

Add a virtual host for Wordpress to Apache

    sudo vim /etc/httpd/conf/httpd.conf

    <VirtualHost *:80>
    ServerAdmin my.email@address.com
    DocumentRoot /var/www/html
    ServerName wordpress
    ErrorLog /var/log/httpd/wordpress-error-log
    CustomLog /var/log/httpd/wordpress-access-log common
    </VirtualHost>

    sudo systemctl restart httpd.service

Add Wordpress as a hostname? Make `/etc/hosts` look like this (add the `wordpress` at the end of line 1):

    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 wordpress
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
                CHANGEME.uit.tufts.edu    template

Add database name, database username, and database password to `/etc/wordpress/wp-config.php`.

Visit http://nolopwp-dev-01.uit.tufts.edu/wp-admin/

### Wordpress test on Ubuntu ###

Start a t2-micro instance of Ubuntu 16.04 on Amazon EC2: `ubuntu/images/hvm-ssd/ubuntu-xenial-16.04-amd64-server-20180627 (ami-759bc50a)`

Add a security group to allow login via SSH.

    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    Permissions 0777 for 'name-of-file.pem' are too open.
    It is required that your private key files are NOT accessible by others.
    This private key will be ignored.

Copy `.pem` key file to home directory and modify permissions to 400 with `chmod 400 name-of-file.pem`

(For some reason, I couldn't change the `.pem` permissions in its original location-- maybe because of restrictive directory permissions or something?)

    ssh -i name-of-file.pem ubuntu@54.91.215.77

Update and upgrade

    sudo apt-get update
    sudo apt-get upgrade

Install Wordpress and MySQL

    sudo apt install wordpress
    sudo apt install mysql-server

Create `/etc/apache2/sites-available/wordpress.conf` containing:

    Alias /blog /usr/share/wordpress
    <Directory /usr/share/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Order allow,deny
        Allow from all
    </Directory>
    <Directory /usr/share/wordpress/wp-content>
        Options FollowSymLinks
        Order allow,deny
        Allow from all
    </Directory>

Enable the site and restart Apache.

    sudo a2ensite wordpress
    sudo service apache2 reload

Create install file at `/etc/wordpress/config-54.91.215.77.php` containing:

    <?php
    define('DB_NAME', 'wordpress');
    define('DB_USER', 'wordpress');
    define('DB_PASSWORD', 'yourpasswordhere');
    define('DB_HOST', 'localhost');
    define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
    define('FS_METHOD', 'direct');
    ?>

Create a file called `wordpress.sql` with this in it:

    CREATE DATABASE wordpress;
    GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
    ON wordpress.*
    TO wordpress@localhost
    IDENTIFIED BY 'yourpasswordhere';
    FLUSH PRIVILEGES;

Run those commands.

    cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf

Visit the install page with a web browser at http://public-ip/blog/wp-admin/install.php

To install plugins, need to change file permissions (or do some weird FTP stuff)

    cd /usr/share/wordpress
    sudo chown -R www-data wp-content/

Make sure that `define('FS_METHOD', 'direct');` is in `/etc/wordpress/config-54.91.215.77.php`

### Ghost for main web pages ###

Create a 1 GB Linode running Ubuntu 18.04 LTS.

Default disk and swap choices (25344 MB ext4 and 256 MB swap, respectively)

SSH into the Linode as `root`.

    apt-get update
    apt-get upgrade
    apt-get install nginx nodejs npm
    adduser bstafford
    usermod -aG sudo bstafford
    

Log in as normal user `bstafford`.

    sudo npm i -g ghost-cli
    sudo mkdir -p /var/www/ghost && sudo chown bstafford:bstafford /var/www/ghost && sudo chmod 775 /var/www/ghost
    cd /var/www/ghost
    ghost install --db sqlite3

Set up "web forwarding" on Gandi.net to send `http://www.nolop.org` to `https://nolop.org`.

Log of the `ghost install` command

    ✔ Checking system Node.js version
    ✔ Checking logged in user
    ✔ Checking current folder permissions
    System checks failed with message: 'Linux version is not Ubuntu 16'
    Some features of Ghost-CLI may not work without additional configuration.
    For local installs we recommend using `ghost install local` instead.
    ? Continue anyway? Yes
    ℹ Checking operating system compatibility [skipped]
    ✔ Checking memory availability
    ✔ Checking for latest Ghost version
    ✔ Setting up install directory
    ✔ Downloading and installing Ghost v1.24.3
    ✔ Finishing install process
    ? Enter your blog URL: https://nolop.org
    ✔ Configuring Ghost
    ✔ Setting up instance
    Running sudo command: useradd --system --user-group ghost
    Running sudo command: chown -R ghost:ghost /var/www/ghost/content
    ✔ Setting up "ghost" system user
    ? Do you wish to set up Nginx? Yes
    ✔ Creating nginx config file at /var/www/ghost/system/files/nolop.org.conf
    Running sudo command: ln -sf /var/www/ghost/system/files/nolop.org.conf /etc/nginx/sites-available/nolop.org.conf
    Running sudo command: ln -sf /etc/nginx/sites-available/nolop.org.conf /etc/nginx/sites-enabled/nolop.org.conf
    Running sudo command: nginx -s reload
    ✔ Setting up Nginx
    ? Do you wish to set up SSL? Yes
    ? Enter your email (used for Let's Encrypt notifications) *****************
    Running sudo command: mkdir -p /etc/letsencrypt
    Running sudo command: ./acme.sh --install --home /etc/letsencrypt
    Running sudo command: /etc/letsencrypt/acme.sh --issue --home /etc/letsencrypt --domain nolop.org --webroot /var/www/ghost/system/nginx-root --reloadcmd "nginx -s reload" --accountemail ***********************
    Running sudo command: openssl dhparam -out /etc/nginx/snippets/dhparam.pem 2048
    Running sudo command: mv /tmp/ssl-params.conf /etc/nginx/snippets/ssl-params.conf
    ✔ Creating ssl config file at /var/www/ghost/system/files/nolop.org-ssl.conf
    Running sudo command: ln -sf /var/www/ghost/system/files/nolop.org-ssl.conf /etc/nginx/sites-available/nolop.org-ssl.conf Running sudo command: ln -sf /etc/nginx/sites-available/nolop.org-ssl.conf /etc/nginx/sites-enabled/nolop.org-ssl.conf
    Running sudo command: nginx -s reload
    ✔ Setting up SSL
    ? Do you wish to set up Systemd? Yes
    ✔ Creating systemd service file at /var/www/ghost/system/files/ghost_nolop-org.service
    Running sudo command: ln -sf /var/www/ghost/system/files/ghost_nolop-org.service /lib/systemd/system/ghost_nolop-org.service
    Running sudo command: systemctl daemon-reload
    ✔ Setting up Systemd
    Running sudo command: /var/www/ghost/current/node_modules/.bin/knex-migrator-migrate --init --mgpath /var/www/ghost/current
    ✔ Running database migrations
    ? Do you want to start Ghost? Yes
    Running sudo command: systemctl is-active ghost_nolop-org
    ✔ Ensuring user is not logged in as ghost user
    ✔ Checking if logged in user is directory owner
    ✔ Checking current folder permissions
    Running sudo command: systemctl is-active ghost_nolop-org
    ✔ Validating config
    ✔ Checking folder permissions
    ✔ Checking file permissions
    ✔ Checking content folder ownership
    ✔ Checking memory availability
    Running sudo command: systemctl start ghost_nolop-org
    ✔ Starting Ghost
    Running sudo command: systemctl is-enabled ghost_nolop-org
    Running sudo command: systemctl enable ghost_nolop-org --quiet
    ✔ Starting Ghost
    You can access your publication at https://nolop.org
    Next, go to to your admin interface at https://nolop.org/ghost/ to complete the setup of your publication

    Ghost uses direct mail by default
    To set up an alternative email method read our docs at https://docs.ghost.org/docs/mail-config

### Editing theme slightly ###

Download default casper theme from admin dashboard, change name to casper-nolop, reupload, and activate.

Log into server and become `ghost` user via `sudo -s -u ghost`.

Edit theme files and copy changes manually to Github repo (https://github.com/tufts-nolop/nolop-org-theme).

After editing files, restart Ghost to see changes take effect.

    systemctl restart ghost_nolop-org
    ==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
    Authentication is required to restart 'ghost_nolop-org.service'.
    Authenticating as: Brandon Stafford,,, (bstafford)
    Password:
    ==== AUTHENTICATION COMPLETE ===

This works for editing .hbs files, but if there's a more complicated process involving Node and Gulp if the CSS or JS files need to be changed.

### Django for tracking tool usage ###

Switch to Python 3 so we can use Django 2.x

    update-alternatives --install /usr/bin/python python /usr/bin/python3 1

Install Pip 3 and make it look like Pip

    root@li905-38:/var/www# apt install python3-pip -y
    root@li905-38:/var/www# ln -s /usr/bin/pip3 /usr/bin/pip
    root@li905-38:/var/www# which pip
    /usr/bin/pip
    root@li905-38:/var/www# pip --version
    pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.6)

Install latest stable Django

    pip install Django==2.0.6

Log out from `root` account and log in as `bstafford`

Start a Django project in `/home/bstafford`

    django-admin startproject noloptools

Add IP address to `noloptools/noloptools/settings.py`

    ALLOWED_HOSTS = ['45.56.103.38']

Start init database and start test server

    cd /var/www/noloptools/
    python manage.py migrate
    python manage.py createsuperuser
    python manage.py runserver 0:8000
