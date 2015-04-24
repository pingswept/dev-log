### Installing mlmmj on Ubuntu 14.04 LTS Trusty on Linode ###

    apt-get update
    apt-get install build-essential

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

Make a mailing list.

    root@localhost:~/mlmmj-1.2.19b1# mlmmj-make-ml -L XXXXXXXXX
    Creating Directorys below /var/spool/mlmmj. Use '-s spooldir' to change
    The Domain for the List? [] : XXXXXX.org
    The emailaddress of the list owner? [postmaster] : 
    
    For the list texts you can choose between the following languages or
    give a absolute path to a directory containing the texts.
    
    Available languages:
    ast  de  en  fi  fr  gr  it  pt  sk  zh-cn
    The path to texts for the list? [en] : 
    
    Don't forget to add this to /etc/aliases:
    XXXXXXXXXXXX:  "|/usr/local/bin/mlmmj-receive -L /var/spool/mlmmj/XXXXXXXXXXX/"
    
    If you're not starting mlmmj-maintd in daemon mode,
    don't forget to add this to your crontab:
    0 */2 * * * "/usr/local/bin/mlmmj-maintd -F -L /var/spool/mlmmj/XXXXXXXXXXX/"
    
     FINAL NOTES
    1) The mailinglist directory have to be owned by the user running the 
    mailserver (i.e. starting the binaries to work the list)
    2) Run newaliases

Add the line `XXXXXXXXXXXX:  "|/usr/local/bin/mlmmj-receive -L /var/spool/mlmmj/XXXXXXXXXXX/"` to `/etc/aliases`

Run `newaliases` to reload the aliases file.
