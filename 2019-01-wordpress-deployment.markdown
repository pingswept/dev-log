(See updated version for Nolop at https://github.com/tufts-nolop/nolop-software-infrastructure/blob/master/nolop-org-deployment.md )

As root, install web server, DB, and connection between them

    apt-get update
    apt-get install nginx mariadb-server php-fpm php-mysql

Then

    cd /var/www
    wget http://wordpress.org/latest.tar.gz
    tar xzvf latest.tar.gz
    mv wordpress html
    rm latest.tar.gz
    cd html
    cp ./wp-config-sample.php ./wp-config.php
    chown -R www-data:www-data /var/www/html

Open up `wp-config.php` and add database credentials.

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

Configure PHP-FPM and PHP-MySQL

    systemctl restart nginx
    systemctl restart php7.2-fpm

Create file `nginx-wordpress.conf` in `/etc/nginx/sites-available` containing
https://raw.githubusercontent.com/tufts-nolop/nolop-software-infrastructure/master/nginx-wordpress.conf

    cd /etc/nginx/sites-enabled
    rm default
    ln -s /etc/nginx/sites-available/nginx-wordpress.conf default
    root@li905-38:/etc/nginx/sites-enabled# ls -l
    total 0
    lrwxrwxrwx 1 root root 47 Oct 30 14:50 default -> /etc/nginx/sites-available/nginx-wordpress.conf

Set up SSL certificates

    apt-get install software-properties-common
    add-apt-repository universe
    add-apt-repository ppa:certbot/certbot
    apt-get update
    apt-get install certbot python-certbot-nginx

Comment out the SSL cert lines at the end of `nginx.conf` and run

    certbot certonly --nginx

Re-enable the SSL cert lines. (Might be easier some other way.)

It appears from `systemctl list-timers` that the Certbot renewal service is installed automatically. Test with `certbot renew --dry-run`

Raise upload limits

Add to both server blocks in `nginx-wordpress.conf`:

    client_max_body_size 100M;

Also fix `/etc/php/7.2/fpm/php.ini` by changing:

    upload_max_filesize = 64M
    post_max_size = 64M

Restart Nginx and PHP.
