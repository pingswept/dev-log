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

### Running server ###

Start server for testing

    sudo python cgi-bin/server.py 0.0.0.0

Running as root because permissions on `/var/supysonic/supysonic.log` are set wrong. Should probably be running as `www-data`.

### Add users and music ###

    sudo ./bin/supysonic-cli
    supysonic> user list
    Name		Admin	Email
    ----		-----	-----
    
    supysonic> user add brandon -a -p the-password-goes-here -e brandon@example.com
    supysonic> user list
    Name		Admin	Email
    ----		-----	-----
    brandon         *	brandon@example.com
    
    supysonic> folder add Music /home/brandon/Music
    Folder 'Music' added
    supysonic> folder list
    Name		Path
    ----		----
    Music           /home/brandon/Music
    supysonic> folder scan Music
    Scanning 'Music': 0% (1/4852)
    Scanning 'Music': 19% (937/4852)
    Scanning 'Music': 43% (2092/4852)
    Scanning 'Music': 63% (3096/4852)
    Scanning 'Music': 83% (4029/4852)
    Scanning 'Music': 99% (4836/4852)
    Scanning 'Music': 100% (4852/4852)
    Scanning done
    Added: 190 artists, 387 albums, 4819 tracks
    Deleted: 0 artists, 0 albums, 0 tracks
    supysonic> exit

### Running with uWSGI ###

When running in development mode, Supysonic is single-threaded, so you can't browse folders while music is streaming. To get multiple threads, you need a WSGI server like uWSGI or Gunicorn.

To install uWSGI on Debian

    sudo apt-get install uwsgi uwsgi-plugin-python

Start it up to test

    sudo uwsgi --http-socket :5000 --processes 8 --plugin python --wsgi-file cgi-bin/supysonic.wsgi

Then put it in `/etc/uwsgi/apps-available/supysonic.ini`

Enable with `sudo ln -s /etc/uwsgi/apps-available/supysonic.ini supysonic.ini`

    sudo chown -R www-data:www-data supysonic

Test .ini file: `uwsgi --ini /etc/uwsgi/apps-enabled/supysonic.ini`

`ini` file

    [uwsgi]
    http-socket = :5000
    processes = 8
    plugin = python
    wsgi-file = /home/brandon/supysonic-master-1282a5ab12/cgi-bin/supysonic.wsgi
    immediate-uid = www-data
    gid = www-data

