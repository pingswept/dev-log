### Overall plan ###

* Use one host to build a package group of Rascal-specific binary packages.
* Upload those packages to http://rascalmicro.com/repos/rascal-base
* On a different host, install Arch Linux ARM
* On this same host, use pacman to download and install the package group.
