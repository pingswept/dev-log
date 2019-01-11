Then

    cd /var/www
    sudo wget http://wordpress.org/latest.tar.gz
    sudo tar xzvf latest.tar.gz
    sudo mv wordpress html
    sudo rm latest.tar.gz
    sudo cp ./wp-config-sample.php ./wp-config.php
    chown -R bstafford:bstafford /var/www/html/*

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
