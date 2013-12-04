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

http://localhost:2368/ghost/

### Install supervisor ###

    sudo apt-get install supervisor

### Install Varnish ###

Varnish has a package repo for the Ubuntu LTS releases.

    curl http://repo.varnish-cache.org/debian/GPG-key.txt | sudo apt-key add -
    echo "deb http://repo.varnish-cache.org/ubuntu/ precise varnish-3.0" | sudo tee -a /etc/apt/sources.list
    sudo apt-get update
    sudo apt-get install varnish
