Install Avahi so that hostname resolution works

    pacman -S avahi
    pacman -S nss-mdns

    systemctl restart dbus
    systemctl start avahi-daemon
    
    systemctl enable avahi-daemon.service

Changed hostname in /etc/hostname from alarm to archbone

Have to hold down S2 (pushbutton near SD card) to boot from SD card

Install mplayer for webcam frame grabbing.

    pacman -S mplayer
    resolving dependencies...
    :: There are 7 providers available for ttf-font:
    :: Repository extra
       1) ttf-bitstream-vera  2) ttf-dejavu  3) ttf-freefont  4) ttf-linux-libertine
    :: Repository community
       5) ttf-droid  6) ttf-liberation  7) ttf-ubuntu-font-family
    
    Enter a number (default=1): 1

This mplayer command would install a slew of mplayer dependencies also, but it doesn't work because a bunch of the dependencies aren't found. Might be that the package lists aren't up to date.

Upgrading.

    [root@archbone ~]# pacman -Syu
    :: Synchronizing package databases...
     core                            52.7 KiB   255K/s 00:00 [##############################] 100%
     extra                          554.9 KiB   491K/s 00:01 [##############################] 100%
     community                      593.2 KiB   664K/s 00:01 [##############################] 100%
     alarm is up to date
     aur                             19.2 KiB  3.75M/s 00:00 [##############################] 100%
    :: Starting full system upgrade...
    :: Replace sysvinit-tools with core/procps-ng? [Y/n] y
    resolving dependencies...
    looking for inter-conflicts...
    
    Packages (18): coreutils-8.22-2  cronie-1.4.11-1  file-5.16-1  inetutils-1.9.1.341-2
                   libldap-2.4.38-1  libpipeline-1.2.5-1  linux-am33x-3.12.5-1
                   linux-api-headers-3.12.4-1  man-pages-3.55-1  mkinitcpio-16-1  mpfr-3.1.2.p5-1
                   pacman-mirrorlist-20131209-1  pcre-8.34-1  procps-ng-3.3.9-1  systemd-208-3
                   systemd-sysvcompat-208-3  sysvinit-tools-2.88-12 [removal]  util-linux-2.24-2
    
    Total Download Size:    28.09 MiB
    Total Installed Size:   66.94 MiB
    Net Upgrade Size:       0.41 MiB

Hmmm. This appears to be upgrading to the 3.12 kernel. Well, it still boots properly.

Install Nginx

    pacman -S nginx

    systemctl enable nginx.service
        ln -s '/usr/lib/systemd/system/nginx.service' '/etc/systemd/system/multi-user.target.wants/nginx.service'

General Linux tools

    pacman -S vim
    pacman -S git
    pacman -S zsh

    [root@archbone var]# chsh
    Changing shell for root.
    New shell [/bin/bash]: /bin/zsh
    Shell changed.

    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh

Install other repos

    ➜  ~  cd /var
    ➜  /var  mkdir www
    ➜  /var  cd www
    ➜  www  ls
    ➜  www  git clone https://github.com/rascalmicro/red.git editor
    Cloning into 'editor'...
    remote: Counting objects: 1378, done.
    remote: Compressing objects: 100% (639/639), done.
    remote: Total 1378 (delta 758), reused 1349 (delta 733)
    Receiving objects: 100% (1378/1378), 956.00 KiB | 169.00 KiB/s, done.
    Resolving deltas: 100% (758/758), done.
    Checking connectivity... done.
    ➜  www  git clone https://github.com/rascalmicro/demos.git public
    Cloning into 'public'...
    remote: Counting objects: 1218, done.
    remote: Compressing objects: 100% (630/630), done.
    remote: Total 1218 (delta 600), reused 1170 (delta 581)
    Receiving objects: 100% (1218/1218), 2.11 MiB | 283.00 KiB/s, done.
    Resolving deltas: 100% (600/600), done.
    Checking connectivity... done.
    ➜  www  ls
    editor  public

    mkdir /etc/uwsgi

https://raw.github.com/rascalmicro/openembedded-rascal/rascal/recipes/uwsgi/files/editor.ini
https://raw.github.com/rascalmicro/openembedded-rascal/rascal/recipes/uwsgi/files/public.ini

    mkdir /var/log/uwsgi
    touch /var/log/uwsgi/public.log
    touch /var/log/uwsgi/emperor.log
    touch /var/log/uwsgi/editor.log 

    /usr/bin/uwsgi --emperor /etc/uwsgi --daemonize /var/log/uwsgi/emperor.log

    pacman -S python-flask
    resolving dependencies...
    looking for inter-conflicts...
    
    Packages (7): python-3.3.3-1  python-itsdangerous-0.23-1  python-jinja-2.7.1-2
                  python-markupsafe-0.18-2  python-setuptools-2.0.1-1  python-werkzeug-0.9.4-1
                  python-flask-0.10.1-4
    
    Total Download Size:    12.52 MiB
    Total Installed Size:   90.53 MiB
    
    :: Proceed with installation? [Y/n] y
    :: Retrieving packages ...
     python-3.3.3-1-armv7h           11.3 MiB   816K/s 00:14 [##############################] 100%
     python-setuptools-2.0.1-1-any  312.8 KiB  1415K/s 00:00 [##############################] 100%
     python-markupsafe-0.18-2-...    20.7 KiB   861K/s 00:00 [##############################] 100%
     python-werkzeug-0.9.4-1-any    465.8 KiB  1640K/s 00:00 [##############################] 100%
     python-jinja-2.7.1-2-any       260.3 KiB  1783K/s 00:00 [##############################] 100%
     python-itsdangerous-0.23-...    16.9 KiB   393K/s 00:00 [##############################] 100%
     python-flask-0.10.1-4-any      157.6 KiB  1394K/s 00:00 [##############################] 100%
    (7/7) checking keys in keyring                           [##############################] 100%
    (7/7) checking package integrity                         [##############################] 100%
    (7/7) loading package files                              [##############################] 100%
    (7/7) checking for file conflicts                        [##############################] 100%
    (7/7) checking available disk space                      [##############################] 100%
    (1/7) installing python                                  [##############################] 100%
    Optional dependencies for python
        tk: for tkinter
        sqlite [installed]
    (2/7) installing python-werkzeug                         [##############################] 100%
    (3/7) installing python-setuptools                       [##############################] 100%
    (4/7) installing python-markupsafe                       [##############################] 100%
    (5/7) installing python-jinja                            [##############################] 100%
    (6/7) installing python-itsdangerous                     [##############################] 100%
    (7/7) installing python-flask                            [##############################] 100%
    ➜  /etc  pacman -S python-flask-login
    error: target not found: python-flask-login

Got stuck

Installing Flask with Pacman makes Python 3.3.3 the default version. This makes server.py fail to load. (There may be other problems too.) Trying to run Python 2 works, but then Python can't find Flask.

    pip install pytronics bitstring webcolors
