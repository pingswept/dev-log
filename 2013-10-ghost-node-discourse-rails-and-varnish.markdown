Installing on an Amazon EC2 t1.micro instance running Ubuntu Server 12.04.3 LTS - ami-a73264ce (64-bit)

### General EC2 Ubuntu fixups ###

    sudo apt-get update
    sudo apt-get install build-essential git zsh
    sudo -s
    chsh -s /bin/zsh ubuntu # change the shell to Zsh for the default user, ubuntu
    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh

Also, copy vimrc and gitconfig from https://github.com/pingswept/linux-tool-configs

### Install Node ###

For Ghost, need a newer version of Node: Required: {"node":">= 0.6.13 < 0.11.0"}

    wget http://nodejs.org/dist/v0.10.22/node-v0.10.22.tar.gz
    tar xzf node-v0.10.22.tar.gz
    cd node-v0.10.22
    ./configure
    make
    sudo make install
    node --version
        v0.10.22

### Install Ghost ###

First, we need the Bourbon gem, which requires rubygems. Since we'll want versions of Ruby and Rails later, we'll install rbenv first.

    sudo apt-get install libssl-dev # will need libssl headers to build OpenSSL gem

    git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
    
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
    echo 'eval "$(rbenv init -)"' >> ~/.zshrc
    exec $SHELL -l
    
    git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    
    rbenv install 2.0.0-p353 # stable as of 2013-12-04
    rbenv rehash
    rbenv global 2.0.0-p353
    which ruby
        /home/ubuntu/.rbenv/shims/ruby
    ruby -v
        ruby 2.0.0p353 (2013-11-22 revision 43784) [x86_64-linux]

First, install Bourbon gem.

    gem install bourbon
    Fetching: sass-3.2.12.gem (100%)
    Successfully installed sass-3.2.12
    Fetching: thor-0.18.1.gem (100%)
    Successfully installed thor-0.18.1
    Fetching: bourbon-3.1.8.gem (100%)
    Successfully installed bourbon-3.1.8
    
    rbenv rehash # so that shims get updated, so newly installed gems work

Then, clone and do some more stuff.

(Here, we're cloning from Github, which is stupid, but it's the only way to get the pages feature right now. In the future, should use Ghost 0.4.)

    git clone https://github.com/TryGhost/Ghost.git ghost-696cfe7018
    git submodule update --init
    sudo npm install -g grunt-cli
    npm install

Use grunt to build CSS and JS files

    grunt init
    Running "shell:bourbon" (shell) task
    
    Running "update_submodules" task
    
    Running "sass:compress" (sass) task
    File "./core/client/assets/css/screen.css" created.
    
    Running "handlebars:core" (handlebars) task
    File "core/client/tpl/hbs-tpl.js" created.
    
    Running "concat:dev" (concat) task
    File "core/built/scripts/vendor.js" created.
    File "core/built/scripts/helpers.js" created.
    File "core/built/scripts/templates.js" created.
    File "core/built/scripts/models.js" created.
    File "core/built/scripts/views.js" created.
    
    Running "concat:prod" (concat) task
    File "core/built/scripts/ghost.js" created.
    
    Done, without errors.

Actually start ghost:

    npm start # from within ghost directory

Need to open a port for web serving on EC2 instance before the site will be visible externally.

http://localhost:2368/ghost/

### Install supervisor ###

    sudo apt-get install supervisor

Add Ghost conf file so that Supervisor runs Ghost on startup.

    sudo vim /etc/supervisor/conf.d/ghost.conf

    [program:ghost]
    command = node /home/ubuntu/ghost-696cfe7018/index.js
    directory = /home/ubuntu/ghost-696cfe7018
    user = ubuntu
    autostart = true
    autorestart = true
    stdout_logfile = /var/log/supervisor/ghost.log
    stderr_logfile = /var/log/supervisor/ghost_err.log
    environment = NODE_ENV="production"

    sudo /etc/init.d/supervisor stop
    sudo /etc/init.d/supervisor start
    
Rather than starting and stopping Supervisorm probably better to do (seems to work, but needs more testing):

    sudo supervisorctl reread

### Install Varnish ###

Varnish has a package repo for the Ubuntu LTS releases.

    curl http://repo.varnish-cache.org/debian/GPG-key.txt | sudo apt-key add -
    echo "deb http://repo.varnish-cache.org/ubuntu/ precise varnish-3.0" | sudo tee -a /etc/apt/sources.list
    sudo apt-get update
    sudo apt-get install varnish

Update /etc/varnish/default.vcl

    backend default {
        .host = "127.0.0.1";
        .port = "2368"; # for Ghost running on node.js
    }

Update /etc/defaults/varnish

    DAEMON_OPTS="-a :80 \
                 -T localhost:6082 \
                 -f /etc/varnish/default.vcl \
                 -S /etc/varnish/secret \                                                                     
                 -s malloc,256m"

The current release of Ghost doesn't handle cookies properly, which means Varnish breaks authentication, and CSS and JS are marked as not cacheable, which slows stuff down. This stuff will supposedly be fixed in 0.4, but here are some workarounds, at least for the cookies: http://we.je/using-varnish-not-nginx-to-run-ghost/

### Install Rails 4 ###

    gem install rails -v 4.0.1
    rbenv rehash

### Install Postgres ###

Postgres is needed for Discourse. The postrgresql-contrib package is needed for the hstore extension that Discourse uses.

    sudo apt-get install postgresql postgresql-contrib
    sudo -u postgres createuser ubuntu -s -P # not sure this was necessary, maybe just needed to set password
    sudo -u postgres createdb dtest
    sudo -u postgres psql -c "grant all privileges on database dtest to ubuntu"
    sudo -u postgres psql dtest -c "create extension hstore; create extension pg_trgm"

### Install Discourse ###
