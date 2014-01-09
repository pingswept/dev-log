Install Avahi so that hostname resolution works

    pacman -S avahi
    pacman -S nss-mdns

    systemctl restart dbus
    systemctl start avahi-daemon
    
    systemctl enable avahi-daemon.service

Changed hostname in /etc/hostname from alarm to archbone

Have to hold down S2 (pushbutton near SD card) to boot from SD card

Install mplayer for webcam frame grabbing. (Turned out not to be as useful as fswebcam.)

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

Install uWSGI

    pacman -S uwsgi
    pacman -S uwsgi-plugin-python

Add to public.ini and editor.ini: plugins = python

If you don't install uwsgi-plugin-python, uWSGI will still start, but the module and callable options will be missing, so no app will be loaded. This will drive you insane. If you see this message, this is what is going on:

    uwsgi: unrecognized option '--module'
    getopt_long() error

This is true on Arch Linux, but it's also true on at least some versions of Ubuntu, which also puts the Python uWSGI plugin in a separate package.

OK, on to the next step.

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
    PYTHONPATH="${PYTHONPATH}:/var/www/public/:/var/www/editor/"
    export PYTHONPATH

After plugging in Logitech webcam

    [  793.397450] usb 2-1: new high-speed USB device number 2 using musb-hdrc
    [  793.666910] usb 2-1: device v046d p0802 is not supported
    [  793.672553] usb 2-1: New USB device found, idVendor=046d, idProduct=0802
    [  793.679623] usb 2-1: New USB device strings: Mfr=0, Product=0, SerialNumber=2
    [  793.687145] usb 2-1: SerialNumber: 47E46680
    [  794.872944] uvcvideo: Found UVC 1.00 device <unnamed> (046d:0802)
    [  794.921676] input: UVC Camera (046d:0802) as /devices/ocp.3/47400000.usb/47401c00.usb/musb-hdrc.1.auto/usb2/2-1/2-1:1.0/input/input0
    [  794.937572] usbcore: registered new interface driver uvcvideo
    [  794.943727] USB Video Class driver (1.1.1)
    [  795.200707] usb_audio: Warning! Unlikely big volume range (=6144), cval->res is probably wrong.
    [  795.209723] usb_audio: [5] FU [Mic Capture Volume] ch = 1, val = 1536/7680/1
    [  795.231493] usbcore: registered new interface driver snd-usb-audio

Attempting to capture a still image

    mplayer -vo png -frames 1 tv://                      
    MPlayer SVN-r36498-snapshot-4.7.2 (C) 2000-2013 MPlayer Team
    206 audio & 433 video codecs
    mplayer: could not connect to socket
    mplayer: No such file or directory
    Failed to open LIRC support. You will not be able to use your remote control.
    
    Playing tv://.
    TV file format detected.
    Selected driver: v4l2
     name: Video 4 Linux 2 input
     author: Martin Olschewski <olschewski@zpr.uni-koeln.de>
     comment: first try, more to come ;-)
    v4l2: your device driver does not support VIDIOC_G_STD ioctl, VIDIOC_G_PARM was used instead.
    Selected device: UVC Camera (046d:0802)
     Capabilities:  video capture  streaming
     supported norms:
     inputs: 0 = Camera 1;
     Current input: 0
     Current format: YUYV
    tv.c: norm_from_string(pal): Bogus norm parameter, setting default.
    v4l2: ioctl enum norm failed: Inappropriate ioctl for device
    Error: Cannot set norm!
    Selected input hasn't got a tuner!
    v4l2: ioctl set mute failed: Invalid argument
    v4l2: ioctl query control failed: Invalid argument
    ==========================================================================
    Opening video decoder: [raw] RAW Uncompressed Video
    Could not find matching colorspace - retrying with -vf scale...
    Opening video filter: [scale]
    Movie-Aspect is undefined - no prescaling applied.
    [swscaler @ 0x133c8c0] BICUBIC scaler, from yuyv422 to rgb24 using C
    VO: [png] 640x480 => 640x480 RGB 24-bit 
    [VO_PNG] Warning: compression level set to 0, compression disabled!
    [VO_PNG] Info: Use -vo png:z=<n> to set compression level from 0 to 9.
    [VO_PNG] Info: (0 = no compression, 1 = fastest, lowest - 9 best, slowest compression)
    png: output directory: .
    Selected video codec: [rawyuy2] vfm: raw (RAW YUY2)
    ==========================================================================
    Audio: no sound
    Starting playback...
    v4l2: select timeout
    V:   0.0   1/  1 ??% ??% ??,?% 0 0 
    
    v4l2: select timeout
    v4l2: ioctl set mute failed: Invalid argument
    v4l2: 0 frames successfully processed, 1 frames dropped.
    
    Exiting... (End of file)

Results in a PNG containing a green box

Install ffmpeg and fswebcam

    pacman -Sy
    pacman -S ffmpeg
    pacman -S fswebcam

This works!

    fswebcam -r 640x480 --jpeg 85 -D 1 shot.jpg

Install yaourt so we can access the AUR (Arch User Repo)

    pacman -S base-devel
    pacman -S wget
    pacman -S package-query
    wget https://aur.archlinux.org/packages/ya/yaourt/yaourt.tar.gz
    tar xzvf yaourt.tar.gz
    cd yaourt
    makepkg --asroot
    pacman -U yaourt-1.3-1-any.pkg.tar.xz
    yaourt libwebcam

Below appeared during libwebcam build. Hmmm.
    
    ** Your V4L2 does not have V4L2_CTRL_TYPE_STRING support. Compiling without raw control support.

Then try to set autofocus.

    uvcdynctrl -c
    Listing available controls for device video0:
      Brightness
      Contrast
      Saturation
      Hue
      Gamma
      Power Line Frequency
      Sharpness

Hmmm. No focus in the list. Sharpness?

Different approach?

    pacman -S v4l-utils
    ➜  ~  v4l2-ctl --all
    Driver Info (not using libv4l2):
    	Driver name   : uvcvideo
    	Card type     : IPEVO Point 2 View
    	Bus info      : usb-musb-hdrc.1.auto-1
    	Driver version: 3.12.5
    	Capabilities  : 0x84000001
    		Video Capture
    		Streaming
    		Device Capabilities
    	Device Caps   : 0x04000001
    		Video Capture
    		Streaming
    Priority: 2
    Video input : 0 (Camera 1: ok)
    Format Video Capture:
    	Width/Height  : 1024/768
    	Pixel Format  : 'MJPG'
    	Field         : None
    	Bytes per Line: 0
    	Size Image    : 1572864
    	Colorspace    : Unknown (00000000)
    Crop Capability Video Capture:
    	Bounds      : Left 0, Top 0, Width 1024, Height 768
    	Default     : Left 0, Top 0, Width 1024, Height 768
    	Pixel Aspect: 1/1
    Streaming Parameters Video Capture:
    	Capabilities     : timeperframe
    	Frames per second: 15.000 (15/1)
    	Read buffers     : 0
    error 5 getting ctrl Brightness
    error 5 getting ctrl Contrast
                         saturation (int)    : min=0 max=31 step=1 default=16 value=16
                                hue (int)    : min=-180 max=180 step=1 default=0 value=0
    ➜  ~  v4l2-ctl -c focus_absolute=250
    unknown control 'focus_absolute'
    ➜  ~  v4l2-ctl -c focus_auto=0      
    unknown control 'focus_auto'
