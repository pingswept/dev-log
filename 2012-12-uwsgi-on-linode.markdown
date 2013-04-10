### Running Red with uWSGI on Linode ###

Install some packages

    sudo apt-get install uwsgi uwsgi-plugin-http uwsgi-plugin-python

Put this in /etc/uwsgi/apps-available/red.ini and symlink to it from /etc/uwsgi/apps-enabled

    [uwsgi]
    http = 0.0.0.0:3031
    master = true
    plugin = python
    file = /var/www/red/__init__.py
    callable = editor
    catch-exceptions = True
    uid = www-data
    gid = www-data

Clone Red git repo.

    git clone git@github.com:rascalmicro/red.git
    git submodule init
    git submodule update # pulls in CodeMirror editor repo

Move editor directory up a level so that it is the root dir (check against the file path listed in red.ini.

Also need to change authentication scheme. Ubuntu Linux stores password hashes in /etc/shadow. Not sure how the hashes are generated.

### Setting up draft sites and Blogofile ###

    mkdir /var/drafts
    chown -R www-data:www-data drafts
    git clone git@github.com:rascalmicro/rascal-www.git rascalmicro.com

    git clone git@github.com:EnigmaCurry/blogofile.git
    cd blogofile
    git checkout 9f6a94cfebe0cbd49b23 # This rev of Blogofile works. 0.7 and 0.7.1 don't for my setup.
    python setup.py install --record files.txt
