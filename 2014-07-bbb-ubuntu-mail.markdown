Installed Ubuntu 14.04 to eMMC successfully.

Ethernet PHY appears to be flaky. On some boots, Ethernet fails, and appears in `dmesg`:

    [  399.182341] libphy: PHY 4a101000.mdio:00 not found
    [  399.187508] net eth0: phy 4a101000.mdio:00 not found on slave 0
    [  399.193839] libphy: PHY 4a101000.mdio:01 not found
    [  399.198983] net eth0: phy 4a101000.mdio:01 not found on slave 1

Power cycling suggested here: https://groups.google.com/d/msg/beagleboard/9mctrG26Mc8/4FjzQDdjrEIJ

Cycled the power, and it seems to work now.

### Install Mail-in-a-box. ###

    sudo apt-get install vim
    sudo apt-get remove apache2
    git clone ttps://github.com/joshdata/mailinabox
    cd mailinabox

Edit `setup/start.sh` to lower minimum memory threshold to 450 MB so we pass.

    sudo setup/start.sh

(Next time, edit reference to `setup/webmail.sh` out of `setup/start.sh` so Roundcube won't get installed.)

### Install Owncloud ###

From directions here: http://software.opensuse.org/download/package?project=isv:ownCloud:community&package=owncloud

    sudo sh -c "echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/xUbuntu_14.04/ /' >> /etc/apt/sources.list.d/owncloud.list"
    wget http://download.opensuse.org/repositories/isv:ownCloud:community/xUbuntu_14.04/Release.key
    sudo apt-key add - < Release.key  
    sudo apt-get update
    sudo apt-get install owncloud
