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
    python manage.py runserver 45.56.103.38:8000
