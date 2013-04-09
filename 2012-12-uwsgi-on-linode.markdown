### Running control freak with uWSGI on Linode ###

Install some packages

    sudo apt-get install uwsgi uwsgi-plugin-http uwsgi-plugin-python

Put this in /etc/uwsgi/apps-available and symlink to it from /etc/uwsgi/apps-enabled

    [uwsgi]
    http = 0.0.0.0:3031
    master = true
    plugin = python
    file = /var/www/editor/editor/__init__.py
    callable = editor
    catch-exceptions = True
    uid = www-data
    gid = www-data

Also need to change authentication scheme. Ubuntu Linux stores password hashes in /etc/shadow. Not sure how the hashes are generated.
