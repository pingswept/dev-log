### Running control freak with uWSGI on Linode ###

Install some packages

    sudo apt-get install uwsgi uwsgi-plugin-http uwsgi-plugin-python

Put this in /etc/uwsgi/apps-available/control-freak.ini and symlink to it from /etc/uwsgi/apps-enabled

    [uwsgi]
    http = 0.0.0.0:3031
    master = true
    plugin = python
    file = /var/www/editor/__init__.py
    callable = editor
    catch-exceptions = True
    uid = www-data
    gid = www-data

Clone Control Freak git repo.

    git clone git@github.com:rascalmicro/control-freak.git
    git submodule init
    git submodule update # pulls in CodeMirror editor repo

Move editor directory up a level so that it is the root dir (check against the file path listed in control-freak.ini.

Also need to change authentication scheme. Ubuntu Linux stores password hashes in /etc/shadow. Not sure how the hashes are generated.

### Setting up Blogofile and draft sites ###

    mkdir /var/drafts
    chown -R www-data:www-data drafts
    git clone git@github.com:rascalmicro/rascal-www.git rascalmicro.com

    wget https://github.com/EnigmaCurry/blogofile/archive/0.7.1.tar.gz
