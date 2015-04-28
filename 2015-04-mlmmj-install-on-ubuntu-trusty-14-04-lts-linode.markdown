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

Add mlmmj to crontab with `crontab -e` (slightly changed from original to run every 2 minutes instead of every 2 hours, since this will be a very low traffic list). Also had to remove quotes around command to avoid `not found` message from cron.

    */2 * * * * /usr/local/bin/mlmmj-maintd -F -L /var/spool/mlmmj/XXXXXXXXXXX/

Subscribe someone.

    /usr/local/bin/mlmmj-sub -L /var/spool/mlmmj/XXXXXXXXXXX/ -a brandon@my-email.com

List the current subscribers.

    /usr/local/bin/mlmmj-list -L /var/spool/mlmmj/pysolar-discuss/

Put in `/var/spool/mlmmj/XXXXXXXX/control/footer`:

    -- 
        To unsubscribe send a blank message to XXXXXXXX+unsubscribe@XXXXXXXX.org

Put in `/var/spool/mlmmj/XXXXXXXX/control/prefix`:

    [listname]

Mails rejected with `Have to invoke either as root or as the user owninglistdir` This is because, by default, Postfix runs mlmmj-receive as user 65534, aka user `nobody`, so it can't access `/var/spool/mlmmj`.

The fix, from http://mlmmj.org/docs/readme-postfix/:

    useradd mlmmj --shell /usr/false --home-dir /var/spool/mlmmj --system
    chown -R mlmmj:mlmmj /var/spool/mlmmj

Add to `/etc/postfix/master.cf`

    # mlmmj mailing lists
    mlmmj   unix  -       n       n       -       -       pipe
      flags=ORhu user=mlmmj argv=/usr/local/bin/mlmmj-receive -F -L /var/spool/mlmmj/$nexthop

Add to `/etc/postfix/main.cf`

    ## MLMMJ STUFF ##
    
    # Only deliver one message to Mlmmj at a time
    mlmmj_destination_recipient_limit = 1
    
    # A map to forward mail to a dummy domain
    virtual_alias_maps = hash:/var/spool/mlmmj/virtual
    
    # Allow virtual alias maps to specify only the user part of the address
    # and have the +extension part preserved when forwarding, so that
    # list-name+subscribe, list-name+confsub012345678, etc. will all work
    propagate_unmatched_extensions = virtual
    
    # A map to forward mail for the dummy domain to the Mlmmj transport
    transport_maps = hash:/var/spool/mlmmj/transport

Put in `/var/spool/mlmmj/virtual`, but substitute list name and domain:

    list-name@domain.tld    domain.tld--list-name@localhost.mlmmj

Put in `/var/spool/mlmmj/transport`, but substitute list name and domain:

    domain.tld--list-name@localhost.mlmmj   mlmmj:list-dir

Then:

    chown -R mlmmj:mlmmj /var/spool/mlmmj/ # again!
    postmap /var/spool/mlmmj/virtual
    postmap /var/spool/mlmmj/transport
    postfix reload

### DNS setup ###

Make A record pointing mail.XXXXXXXXX.org at IP of Linode.

Make MX record that contains `10 mail.XXXXXXXXXX.org`

### Adding web interface ###

    apt-get install mhonarc
    wget https://git.cryptomilk.org/projects/mlmmj-webarchiver.git/snapshot/mlmmj-webarchiver-1.0.0.tar.gz
    tar xzvf mlmmj-webarchiver-1.0.0.tar.gz
    cd mlmmj-webarchiver-1.0.0
    cp mlmmj-webarchiver.sh /usr/bin/mlmmj-webarchiver.sh
    cp -r mlmmj-webarchiver /etc

Change `/etc/mlmmj-webarchiver/mlmmj-webarchiver.conf` to folder that Nginx will look at.

    WEBARCHIVE_OUT="/var/www/XXXXXXXX"

Create output directory.

    mkdir -p /var/www/XXXXXXXXX
    chown -R www-data:www-data /var/www

Create webarchive marker

    touch /var/spool/mlmmj/XXXXXXXX/control/webarchive
    chown mlmmj:mlmmj /var/spool/mlmmj/XXXXXXXX/control/webarchive
    
Customize contact email in 2 places in `/etc/mlmmj-webarchiver/mhonarc/layout.mrc`. Grep for `cynapses` to find them.

### Install Nginx. Install PHP. ###

Based on: https://www.linode.com/docs/websites/nginx/nginx-and-phpfastcgi-on-ubuntu-12-04-lts-precise-pangolin/

    apt-get install nginx php5-cli php5-cgi php5-fpm spawn-fcgi psmisc

Add Nginx config file at `/etc/nginx/sites-available/XXXXXXXXXX`

    server {
        server_name lists.XXXXXXXXXX.org;
        access_log /var/log/nginx/XXXXXXXXXX/access.log;
        error_log /var/log/nginx/XXXXXXXXXX/error.log;
        root /var/www/XXXXXXXXXX;
    
        location / {
            index  index.html index.htm;
        }
    
        location ~ \.php$ {
            include /etc/nginx/fastcgi_params;
            fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME /var/www/XXXXXXXXX$fastcgi_script_name;
        }
    }

Enable site

    ln -s /etc/nginx/sites-available/XXXXXXXXX /etc/nginx/sites-enabled/XXXXXXXXX

Create cron job from README:

    create /etc/cron.d/mlmmj-webarchiver with the following content:
        MAILTO=root
        */10 * * * * root /usr/bin/mlmmj-webarchiver.sh 2>/dev/null
