### Installing mlmmj on Ubuntu 14.04 LTS Trusty on Linode ###

    wget http://mlmmj.org/releases/mlmmj-1.2.19b1.tar.gz
    tar xzvf mlmmj-1.2.19b1.tar.gz
    cd mlmmj-1.2.19b1

As root:

    ./configure
    make
    make install

Install mailserver. Prompted for domain name at this point.

    apt-get install postfix

During configuration, chose "internet site."

`recipient_delimiter = +` mentioned in mlmmj docs is already in `/etc/postfix/main.cf`.
