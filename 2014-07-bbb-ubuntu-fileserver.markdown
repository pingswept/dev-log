Set password for user `ubuntu`
Leave root password locked; sudo from ubuntu account for root stuff.
Change hostname in '/etc/hostname'

Vim is useful.

    sudo apt-get install vim

Install Cozy from http://cozy.io/host/install.html

    sudo apt-get install python python-pip python-dev software-properties-common
    sudo pip install fabric fabtools
    wget https://raw.githubusercontent.com/cozy/cozy-setup/master/fabfile.py
    fab -H ubuntu@192.168.1.149 install
