**Notes on boot times**

Boot: around 10 seconds, but takes more like 30 seconds for Avahi broadcast so .local address resolves.

Shutdown: around 15 seconds.

### Install supervisor ###

Supervisor is a Python program that runs other programs as subprocesses: http://supervisord.org/introduction.html

    pacman -S supervisor   
    resolving dependencies...
    looking for inter-conflicts...

    Packages (2): python2-meld3-0.6.10-2  supervisor-3.0-1

Start with `systemctl`. Note that the service unit is called supervisord, not just supervisor.

    systemctl start supervisord                      
    ➜  ~  systemctl | grep super     
    supervisord.service    loaded active running    Process Monitoring and Control Daemon

Enabling on boot under `systemd`

    ➜  ~  systemctl is-enabled supervisord
    disabled
    ➜  ~  systemctl enable supervisord
    ln -s '/usr/lib/systemd/system/supervisord.service' '/etc/systemd/system/multi-user.target.wants/supervisord.service'
    ➜  ~  systemctl is-enabled supervisord
    enabled

After reboot

    ➜  ~  systemctl status supervisord    
    supervisord.service - Process Monitoring and Control Daemon
       Loaded: loaded (/usr/lib/systemd/system/supervisord.service; enabled)
       Active: active (running) since Fri 1999-12-31 18:06:55 MST; 14 years 2 months ago
      Process: 134 ExecStart=/usr/bin/supervisord (code=exited, status=0/SUCCESS)
     Main PID: 154 (supervisord)
       CGroup: /system.slice/supervisord.service
               └─154 /usr/bin/python2 /usr/bin/supervisord
    
    Dec 31 18:06:55 rascal2 supervisord[134]: /usr/lib/python2.7/site-packages/supervisor/options.py:295: UserWarning:...
    Dec 31 18:06:55 rascal2 supervisord[134]: 'Supervisord is running as root and it is searching '
    Dec 31 18:06:55 rascal2 systemd[1]: Started Process Monitoring and Control Daemon.

Looks like clock hadn't been synced by NTP when log was written. After restart of supervisord, but not hard reboot of board, active running time was accurate.

Running `kiosk.py` under supervisor 

    ➜  ~  cat /etc/supervisor.d/kiosk.ini 
    [program:kiosk]
    command=/root/kiosk.py

    supervisorctl start kiosk

(Doesn't actually work now because kiosk.py has syntax errors under Python 3.)

### Sound playback ###

Previously used `aplay` to play WAVs through USB soundcard.

New approach: use `mpg123` to play MP3s through TLV320AIC3104-based audio cape.

    pacman -S mpg123
    warning: mpg123-1.18.1-1 is up to date -- reinstalling
    resolving dependencies...
    looking for inter-conflicts...
    
    Packages (1): mpg123-1.18.1-1

According to http://mpg123.org/, mpg123 1.19.0 improves ARM playback. Might be good.

Cape rev 02 should be available very soon (March 2014?), according to: https://groups.google.com/d/topic/beagleboard/jBLsE5lXEmM/discussion
For 

### USB RFID card reader ###

    [ 3880.005376] musb-hdrc musb-hdrc.1.auto: VBUS_ERROR in a_wait_bcon (91, <VBusValid), retry #1, port1 00000100
    [ 3880.545655] usb 2-1: new full-speed USB device number 2 using musb-hdrc
    [ 3880.682049] usb 2-1: device v08ff p0009 is not supported
    [ 3880.687673] usb 2-1: New USB device found, idVendor=08ff, idProduct=0009
    [ 3880.694743] usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    [ 3880.702265] usb 2-1: Product: USB RFID Reader
    [ 3880.706869] usb 2-1: Manufacturer: 深圳市思远创智能设备有限公司
    http://www.sycreader.com
    [ 3880.718255] usb 2-1: SerialNumber: version 0106
    [ 3880.734367] input: 深圳市思远创智能设备有限公司
    http://www.sycreader.com USB RFID Reader as /devices/ocp.3/47400000.usb/47401c00.usb/musb-hdrc.1.auto/usb2/2-1/2-1:1.0/input/input0
    [ 3880.755528] hid-generic 0003:08FF:0009.0001: input,hidraw0: USB HID v1.10 Keyboard [深圳市思远创智能设备有限公司
    http://www.sycreader.com USB RFID Reader] on usb-musb-hdrc.1.auto-1/input0

### Pulling files from Soundcloud ###

    import requests
    import simplejson as json
    
    r = requests.get('http://api.soundcloud.com/groups/81828/tracks.json?client_id=XXXXXXXXXXXXX')
    tracks = json.loads(r.text)
    for track in tracks:
        if track['downloadable']:
            print(track['download_url'])
