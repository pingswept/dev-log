### Installing Prose.io on Ubuntu 12.04 (Precise Pangolin) ###

    apt-get install ruby rubygems ruby-liquid python-pygments ruby1.8-dev
    gem install jekyll

    git clone git@github.com:prose/prose.git
    
Register local copy of Prose as an OAuth application with Github here: https://github.com/settings/applications/new

Install Node.JS to run Gatekeeper.

    apt-get install nodejs npm
    npm install express
    
    git clone git@github.com:prose/gatekeeper.git
    cd gatekeeper
    node server.js

Add client ID and secret from OAuth app registration to gatekeeper/config.json.

### Making Wiegand protocol Linux kernel driver work with the Rascal ###

Forked Mark Jason Anderson's repo on Github, patched to change kernel directory, switch to pins that the Rascal connects to Arduino headers.

Forked repo: https://github.com/rascalmicro/wiegand-linux
