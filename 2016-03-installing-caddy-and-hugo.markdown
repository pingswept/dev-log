Expand filesystem, change default password:

    sudo raspi-config

    sudo apt-get install git vim

Default version of `go`, which is 1.3.3., won't build Caddy, so we have to install a version of Go from backports.

Add to `/etc/apt/sources.list`:

    deb http://http.debian.net/debian jessie-backports main

Then:

    sudo apt-get -t jessie-backports install "golang"

Add to `~/.bashrc`

    export GOPATH="${HOME}/go"
    export PATH="${GOPATH}/bin:${PATH}"

    source ~/.bashrc
    go get github.com/mholt/caddy
    caddy
