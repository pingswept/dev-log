Download .deb package from http://atom.io

    sudo dpkg --install atom-amd64.deb
    apm install particle-dev-complete

Ah, wait, that deb package is for AMD64, not x86.

## Building Atom from source ##

    sudo apt-get install build-essential git libgnome-keyring-dev fakeroot
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.4/install.sh | bash

Then restart terminal.

    nvm install 4
