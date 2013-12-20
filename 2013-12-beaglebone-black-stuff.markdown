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

(installs a slew of mplayer dependencies also)
