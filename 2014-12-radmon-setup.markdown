Add `#!/usr/bin/python` to `headlessMonitor.py`

`chmod a+x headlessMonitor.py`

In `emorpho-cpython-linux`, run `python ./setup.py install`
In `geopy-master`, run `python ./setup.py install`

### Load database schema ###

    sudo -u postgres psql

### Open files ###

    root@rascal-green:~# lsof | wc -l
    1682
