Download .deb package from http://atom.io

    sudo dpkg --install atom-amd64.deb
    apm install particle-dev-complete

Ah, wait, that deb package is for AMD64, not x86.

## Building Atom from source ##

    sudo apt-get install build-essential git libgnome-keyring-dev fakeroot
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.4/install.sh | bash

Then restart terminal.

    nvm install 4
    npm install -g npm
    cd repos
    git clone https://github.com/atom/atom
    git fetch -p
    git checkout $(git describe --tags `git rev-list --tags --max-count=1`)
    ./script/build
    
    sudo script/grunt install
    
