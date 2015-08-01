### Prerequisites ###

Install a bunch of stuff on Ubuntu Linux 15.04.

    sudo apt-get install python-flask python-storm python-imaging python-simplejson
    sudo apt-get install python-requests python-mutagen python-watchdog

### Install supysonic ###

Download zip file from https://github.com/spl0k/supysonic

    unzip supysonic-master.zip supysonic-master-1282a5ab12
    cd supysonic-master-1282a5ab12
    sudo python setup.py install

### Create database ###

    sqlite3 supysonic.db < schema/sqlite.sql
    sudo mv supysonic.db /var/supysonic
