### Installing Prose.io on Ubuntu 12.04 (Precise Pangolin) ###

    sudo apt-get install nodejs npm
    npm install express

    git clone git@github.com:prose/prose.git
    
Register local copy of Prose as an OAuth application with Github here: https://github.com/settings/applications/new

    git clone git@github.com:prose/gatekeeper.git
    cd gatekeeper
    node server.js

Add client ID and secret from OAuth app registration to gatekeeper/config.json.
