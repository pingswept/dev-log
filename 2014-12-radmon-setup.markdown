Add `#!/usr/bin/python` to `headlessMonitor.py`

`chmod a+x headlessMonitor.py`

In `emorpho-cpython-linux`, run `python ./setup.py install`
In `geopy-master`, run `python ./setup.py install`

### Load database schema ###

    mv /usr/local/psql-update
    chown postgres:postgres psql-update

    sudo -u postgres psql
    CREATE EXTENSION postgis;
    \i /usr/local/psql-update
    
    CREATE ROLE radiation; (Need to set permissions?)

### Open files ###

    root@rascal-green:~# lsof | wc -l
    1682
