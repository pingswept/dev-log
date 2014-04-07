Originally installed on an Amazon EC2 t1.micro instance running Ubuntu Server 12.04.3 LTS - ami-a73264ce (64-bit) with Ghost 0.4.0, then upgraded to 0.4.1

Later tested on Linode 1024 using 64-bit Ubuntu 12.04 LTS with Ghost 0.4.2.

### General EC2 Ubuntu fixups ###

    sudo apt-get update
    sudo apt-get install build-essential git zsh sqlite3 # sqlite3 is the command line tool
    sudo -s
    chsh -s /bin/zsh ubuntu # change the shell to Zsh for the default user, ubuntu

Log out, then back in again.

    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
    curl -o ~/.vimrc https://raw.github.com/pingswept/linux-tool-configs/master/vimrc

Also, copy gitconfig from https://github.com/pingswept/linux-tool-configs

### Install Node ###

For Ghost, need a newer version of Node: Required: {"node":">= 0.6.13 < 0.11.0"}

    wget http://nodejs.org/dist/v0.10.26/node-v0.10.26.tar.gz
    tar xzf node-v0.10.26.tar.gz
    cd node-v0.10.26
    ./configure
    make
    sudo make install
    node --version
        v0.10.26

### Install Ghost ###

First, we need the Bourbon gem, which requires rubygems.

    sudo apt-get install rubygems
    echo "gem: --no-ri --no-rdoc" > ~/.gemrc

Then, install Bourbon gem.

    sudo gem install bourbon
    Fetching: sass-3.2.13.gem (100%)
    Fetching: thor-0.18.1.gem (100%)
    Fetching: bourbon-3.1.8.gem (100%)
    Successfully installed sass-3.2.13
    Successfully installed thor-0.18.1
    Successfully installed bourbon-3.1.8
    3 gems installed

Install Bundler gem.

    sudo gem install bundler

Download the Ghost tarball

    wget https://github.com/TryGhost/Ghost/archive/0.4.2.tar.gz
    tar xzvf 0.4.2.tar.gz
    mv Ghost-0.4.2 /var/
    
    cd /var/Ghost-0.4.2
    sudo npm install -g grunt-cli
    npm install

Use grunt to build CSS and JS files. Have to use --force because there is a grunt task to init git submodules, but we downloaded the tarball rather than cloning the git repo.

    grunt init --force
    
    Running "shell:bourbon" (shell) task

    Running "update_submodules" task
    Warning: fatal: Not a git repository (or any of the parent directories): .git Used --force, continuing.
    
    Running "sass:compress" (sass) task
    File "./core/client/assets/css/screen.css" created.
    
    Running "handlebars:core" (handlebars) task
    File core/client/tpl/hbs-tpl.js created.
    
    Running "concat:dev" (concat) task
    File "core/built/scripts/vendor.js" created.
    File "core/built/scripts/helpers.js" created.
    File "core/built/scripts/templates.js" created.
    File "core/built/scripts/models.js" created.
    File "core/built/scripts/views.js" created.
    
    Running "concat:prod" (concat) task
    File "core/built/scripts/ghost.js" created.

Install theme

(If you want to be able to commit theme changes back to Github, generate SSH keys and add public key to Github account.)

    cd content/themes
    git clone git@github.com:rascalmicro/rascal-ghost-theme.git

Test that Ghost works!

    npm start # from within ghost directory

Build the production files

    grunt init prod --force

Need to open a port for web serving on EC2 instance before the site will be visible externally.

http://localhost:2368/ghost/

### Install Nginx as a front end ###

    sudo apt-get install nginx

    cd /etc/nginx/
    sudo rm sites-enabled/default
    sudo vim sites-available/ghost

Stick this in `sites-available/ghost`

    server {
        listen 0.0.0.0:80;
        server_name *your-domain-name*;
        access_log /var/log/nginx/*your-domain-name*.log;
    
        location / {
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header HOST $http_host;
            proxy_set_header X-NginX-Proxy true;
    
            proxy_pass http://127.0.0.1:2368;
            proxy_redirect off;
        }
    }

Activate site with symbolic link

    sudo ln -s /etc/nginx/sites-available/ghost /etc/nginx/sites-enabled/ghost

Double check the symbolic link

    sudo vim /etc/nginx/sites-enabled/ghost

Should see contents of `/etc/nginx/sites-available/ghost`

** nginx-extras doesn't contain the fancy index module for Ubuntu 12.04 **

Add `nginx-extras` for fancy indexing module.

    sudo apt-get install nginx-extras

** nginx-extras doesn't contain the fancy index module for Ubuntu 12.04 **

### Install supervisor ###

    sudo apt-get install supervisor

Add Ghost conf file so that Supervisor runs Ghost on startup.

    sudo vim /etc/supervisor/conf.d/ghost.conf

    [program:ghost]
    command = node /var/Ghost-0.4.2/index.js
    directory = /var/Ghost-0.4.2
    user = www-data
    autostart = true
    autorestart = true
    stdout_logfile = /var/log/supervisor/ghost.log
    stderr_logfile = /var/log/supervisor/ghost_err.log
    environment = NODE_ENV="production"

    sudo /etc/init.d/supervisor stop
    sudo /etc/init.d/supervisor start
    
Rather than starting and stopping Supervisor, probably better to do (seems to work, but needs more testing):

    sudo supervisorctl reread

### Upgrade to Ghost 0.4.1 ###

    sudo apt-get install unzip
    wget https://github.com/TryGhost/Ghost/archive/0.4.1.zip
    mv Ghost-0.4.0/core .
    mv Ghost-0.4.0 Ghost-0.4.1 # rename in preparation for overwrite
    unzip -uo 0.4.1.zip -d . # unzip and overwrite
    npm install --production
    grunt init --force # build CSS and JS
    grunt prod
    
Edit `/etc/supervisor/conf.d/ghost.conf` to use NODE_ENV production and update folder name to Ghost-0.4.1

    sudo supervisorctl start ghost

Copying created_at dates to publication date column

    cd content/data
    sqlite3 ghost-dev.db
    update posts set published_at = created_at;
