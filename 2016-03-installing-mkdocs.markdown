
    sudo apt-get install python3-pip
    sudo pip3 install mkdocs

Set locale to `en_US.UTF-8` with `raspi-config`.

    mkdocs new docs

Add to `mkdocs.yml`:

    theme: 'material'

Run development server.

    sudo mkdocs serve -a 0.0.0.0:80
