Add `#!/usr/bin/python` to `headlessMonitor.py`

`chmod a+x headlessMonitor.py`

    apt-get install libftdi-dev
    git clone https://github.com/rascalmicro/emorpho-cpython.git
    git checkout -b linux origin/linux

In `emorpho-cpython-linux`, run `python ./setup.py install`
In `geopy-master`, run `python ./setup.py install`

### Load database schema ###

Need to build PostGIS 2.x because 

    apt-get install postgresql-server-dev-9.1 libxml2-dev libproj-dev libjson0-dev libgeos-dev xsltproc docbook-xsl docbook-mathml
    wget http://download.osgeo.org/postgis/source/postgis-2.0.6.tar.gz
    tar xfz postgis-2.0.6.tar.gz
    cd postgis-2.0.6
    ./configure

Stuck here.

    make
    sudo make install
    sudo ldconfig
    sudo make comments-install


    mv /usr/local/psql-update
    chown postgres:postgres psql-update

    sudo -u postgres createdb radiation
    sudo -u postgres psql radiation
    CREATE EXTENSION postgis;
    \i /usr/local/psql-update
    
    CREATE ROLE radiation PASSWORD 'password-goes-here' login;

### Open files ###

    root@rascal-green:~# lsof | wc -l
    1682

### Memory leak ###

headlessMonitor.py is leaking at ~50 MB/hour. At a sample time of 2 seconds (1800 samples/hour), that's around 28kB per sample.
