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
