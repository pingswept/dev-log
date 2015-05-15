### Add a non-root user ###

    adduser bstafford
    sudo usermod -a -G sudo bstafford # add to sudoers

### General Ubuntu fixups ###

    sudo apt-get update
    sudo apt-get install build-essential curl git openssh-server sqlite3 vim zsh
    sudo -s
    chsh -s /bin/zsh bstafford # or ubuntu, default Ubuntu user on EC2

Log out, then back in again.

    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
    curl -o ~/.vimrc https://raw.github.com/pingswept/linux-tool-configs/master/vimrc

Also, copy gitconfig from https://github.com/pingswept/linux-tool-configs

### Tweaking vim more ###

On OS X, install XCode, then:

    brew install macvim --override-system-vim
    brew linkapps

Restart terminal.

    brew install ctags-exuberant

Then https://github.com/kepbod/ivim
