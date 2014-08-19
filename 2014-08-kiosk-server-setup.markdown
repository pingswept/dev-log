Create a 2048 MB Linode running Ubuntu 14.04 LTS and log in as root over SSH.

    apt-get install python-pip python-dev # need python-dev for build of reportlab later
    pip install bottle

Running Bottle 0.12.7

    pip install dnspython reportlab sqlalchemy sqlsoup wtforms

SQLalchemy installed, but compilation of C extension failed, which will make it a bit slower.

    apt-get install git
    apt-get install gunicorn

(Should probably worry about creating www-data user at this point.)

Make directory `/var/www/wsgi-scripts`

    cd /var/www/wsgi-scripts
    git clone https://github.com/ArtisansAsylum/kiosk.git

The next problem is that kiosk.py imports a module called madb, which is "Member App DB." It appears to just hold database credentials, `db.user, db.pwd, db.host, db.name`

Comment out the db related stuff (which obviously breaks the DB connection for now).

Make the Bottle app runnable using gunicorn, based on http://blog.yprez.com/running-a-bottle-app-with-gunicorn.html

    gunicorn -w 4 -b 0.0.0.0:80 kiosk:application

Install `supervisor` to run at boot.

    apt-get install supervisor

Add this to `/etc/supervisor/conf.d/gunicorn.conf`

    [program:gunicorn]
    command=/usr/bin/gunicorn -w 4 -b 0.0.0.0:80 kiosk:application
    directory=/var/www/wsgi-scripts/kiosk
    autostart=true
    autorestart=true
    redirect_stderr=true
