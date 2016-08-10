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
    
Result of build

    Done, without errors.

    Execution Time (2016-08-09 21:14:21 UTC)
    download-electron                4m 4.4s  ▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 46%
    download-electron-chromedriver     10.7s  ▇ 2%
    build                           2m 40.6s  ▇▇▇▇▇▇▇▇▇▇ 30%
    coffee:glob_to_multiple             8.9s  ▇ 2%
    prebuild-less:src                  40.3s  ▇▇▇ 8%
    generate-asar                        58s  ▇▇▇▇ 11%
    Total 8m 53.7s

    
    sudo PATH="$PATH:/usr/local/bin" script/grunt install
    
Need PATH amendment or can't find node with sudo: `/usr/bin/env: ‘node’: No such file or directory`

Compare:

    which node
    /home/brandon/.nvm/versions/node/v4.4.7/bin/node

    sudo which node
    (no results, meaning node not found)

    sudo PATH="$PATH:/usr/local/bin" which node
    /home/brandon/.nvm/versions/node/v4.4.7/bin/node

