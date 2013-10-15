    sudo apt-get install npm
    cd Ghost-0.3.2-11 
    npm install --production


Arrg. Need a newer version of Node.

    npm ERR! Unsupported
    npm ERR! Not compatible with your version of node/npm: sqlite3@2.1.16
    npm ERR! Required: {"node":">= 0.6.13 < 0.11.0"}
    npm ERR! Actual:   {"npm":"1.1.4","node":"0.6.12"}

Start over

    sudo apt-get remove npm
    sudo apt-get autoremove
    cd node-v0.10.20
    ./configure
    make
    sudo make install
    node --version
    v0.10.20
