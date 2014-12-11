Add `#!/usr/bin/python` to `headlessMonitor.py`

`chmod a+x headlessMonitor.py`

    apt-get install libftdi-dev
    git clone https://github.com/rascalmicro/emorpho-cpython.git
    git checkout -b linux origin/linux

In `emorpho-cpython-linux`, run `python ./setup.py install`
In `geopy-master`, run `python ./setup.py install`

### Load database schema ###

    mv psql-update /usr/local/psql-update
    chown postgres:postgres psql-update

    sudo -u postgres createdb radiation
    sudo -u postgres psql radiation
    sudo -u postgres psql -d radiation -f /usr/share/postgresql/9.1/contrib/postgis-1.5/postgis.sql
    sudo -u postgres psql -d radiation -f /usr/share/postgresql/9.1/contrib/postgis-1.5/spatial_ref_sys.sql
    \i /usr/local/psql-update
    
    CREATE ROLE radiation PASSWORD 'password-goes-here' login;

### Some difficulty with faked GPS data going into db ###

    psycopg2.ProgrammingError: column "location" is of type geometry but expression is of type numeric
    LINE 1: ...ps, histogram, altitude, sensor, version) VALUES (2.0, 3.0, ...
                                                                 ^
    HINT:  You will need to rewrite or cast the expression.

### Open files ###

    root@rascal-green:~# lsof | wc -l
    1682

### Memory leak ###

headlessMonitor.py is leaking at ~50 MB/hour. At a sample time of 2 seconds (1800 samples/hour), that's around 28kB per sample.

### Notes on PostGIS 2.0 build (not actually needed) ###
