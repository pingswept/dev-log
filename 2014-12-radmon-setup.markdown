Add `#!/usr/bin/python` to `headlessMonitor.py`

`chmod a+x headlessMonitor.py`

In `emorpho-cpython-linux`, run `python ./setup.py install`
In `geopy-master`, run `python ./setup.py install`

### Load database schema ###

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
