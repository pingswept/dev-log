Install Ipython from Github

    virtualenv venv-ipython
    cd venv-ipython
    git clone --recursive https://github.com/ipython/ipython.git
    cd ipython
    git submodule update
    sudo python setup.py install

Install some dependencies

    sudo apt-get install libzmq-dev
    sudo pip install pyzmq
    sudo pip install tornado
    ipython notebook
    sudo apt-get install python-matplotlib
