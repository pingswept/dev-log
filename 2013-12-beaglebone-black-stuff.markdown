Install Avahi so that hostname resolution works

    pacman -S avahi
    pacman -S nss-mdns

    systemctl restart dbus
    systemctl start avahi-daemon
    
    systemctl enable avahi-daemon.service

Changed hostname in /etc/hostname from alarm to archbone

Have to hold down S2 (pushbutton near SD card) to boot from SD card
