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
    cd Ghost-0.3.2-11 
    npm install --production
    npm update
    npm start

Works!

    npm start               
    
    > ghost@0.3.2-11 start /home/brandon/Ghost-0.3.2-11
    > node index
    
    Ghost is running... 
    Listening on 127.0.0.1:2368 
    Url configured as: http://my-ghost-blog.com 
    Ctrl+C to shut down

### Install from Github HEAD ###

git clone git@github.com:TryGhost/Ghost.git ghost
git submodule update --init
sudo npm install -g grunt-cli
npm install




http://localhost:2368/ghost/
