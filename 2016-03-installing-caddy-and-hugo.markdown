    sudo apt-get install git golang

Add to `~/.bashrc`

    export GOPATH="${HOME}/go"
    export PATH="${GOPATH}/bin:${PATH}"

source ~/.bashrc
go get github.com/mholt/caddy
