Expand filesystem, change default password:

    sudo raspi-config

    sudo apt-get install git mercurial vim

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

Need to install caddy-hugo plugin

    go get github.com/hacdias/caddy-hugo
    go/src/github.com/hacdias/caddy-hugo/hugo.go:18:2: no buildable Go source files in /home/pi/go/src/github.com/hacdias/caddy-hugo/routes/assets

Build hugo

    go get -v github.com/spf13/hugo

Install a theme.

Then:

    hugo --bind=0.0.0.0 server
    
Then look at port 1313
