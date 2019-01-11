Then

    cd /var/www
    sudo wget http://wordpress.org/latest.tar.gz
    sudo tar xzvf latest.tar.gz
    sudo mv wordpress html
    sudo rm latest.tar.gz
    sudo cp ./wp-config-sample.php ./wp-config.php
    chown -R bstafford:bstafford /var/www/html/*

Open up `wp-config.php` and add database credentials.

Install DB server

    sudo apt install mariadb-server

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

Install PHP-FPM and PHP-MySQL

    apt-get install php-fpm php-mysql
    systemctl restart nginx
    systemctl restart php7.2-fpm
    chown -R www-data:www-data html/

Put in `/etc/nginx/sites-available/default`
(SSL not working yet)

    server {
            listen 80 default_server;
            listen [::]:80 default_server;

            # SSL configuration
            #
            listen 443 ssl default_server;
            listen [::]:443 ssl default_server;

            root /var/www/html;

            # Add index.php to the list if you are using PHP
            index index.php index.html index.htm index.nginx-debian.html;

            server_name _;

            location / {
                    # First attempt to serve request as file, then
                    # as directory, then fall back to displaying a 404.
                    try_files $uri $uri/ =404;
            }

            # pass PHP scripts to FastCGI server
            #
            location ~ \.php$ {
                    include snippets/fastcgi-php.conf;

                    # With php-fpm (or other unix sockets):
                    fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
            }
}
