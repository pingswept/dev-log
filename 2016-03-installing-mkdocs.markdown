
    sudo apt-get install python3-pip
    sudo pip3 install mkdocs
    sudo pip3 install mkdocs-cinder # or some other theme if you want

Set locale to `en_US.UTF-8` with `raspi-config`.

    mkdocs new docs

Add to `mkdocs.yml`:

    theme: 'cinder'

Run development server.

    sudo mkdocs serve -a 0.0.0.0:80
