Logitech C920: http://www.amazon.com/Logitech-Webcam-Widescreen-Calling-Recording/dp/B006JH8T3S/

From dmesg

    [  163.600127] usb usb1: usb wakeup-resume
    [  163.600220] usb usb1: usb auto-resume
    [  163.600269] hub 1-0:1.0: hub_resume
    [  163.600359] hub 1-0:1.0: port 1: status 0101 change 0001
    [  163.702577] hub 1-0:1.0: state 7 ports 1 chg 0002 evt 0000
    [  163.702679] hub 1-0:1.0: port 1, status 0101, change 0000, 12 Mb/s
    [  163.807953] usb 1-1: new high-speed USB device number 2 using musb-hdrc
    [  163.929207] usb 1-1: skipped 1 descriptor after configuration
    [  163.929256] usb 1-1: skipped 10 descriptors after interface
    [  163.929294] usb 1-1: skipped 1 descriptor after endpoint
    [  163.929331] usb 1-1: skipped 60 descriptors after interface
    [  163.929387] usb 1-1: skipped 1 descriptor after endpoint
    [  163.929420] usb 1-1: skipped 4 descriptors after interface
    [  163.929452] usb 1-1: skipped 2 descriptors after interface
    [  163.929486] usb 1-1: skipped 1 descriptor after endpoint
    [  163.929517] usb 1-1: skipped 2 descriptors after interface
    [  163.929549] usb 1-1: skipped 1 descriptor after endpoint
    [  163.929581] usb 1-1: skipped 2 descriptors after interface
    [  163.929613] usb 1-1: skipped 1 descriptor after endpoint
    [  163.929797] usb 1-1: default language 0x0409
    [  163.930062] usb 1-1: USB interface quirks for this device: 2
    [  163.930099] usb 1-1: udev 2, busnum 1, minor = 1
    [  163.930135] usb 1-1: New USB device found, idVendor=046d, idProduct=082d
    [  163.930170] usb 1-1: New USB device strings: Mfr=0, Product=2, SerialNumber=1
    [  163.930202] usb 1-1: Product: HD Pro Webcam C920
    [  163.930285] usb 1-1: SerialNumber: FDB9EE6F
    [  163.931568] usb 1-1: usb_probe_device
    [  163.931612] usb 1-1: configuration #1 chosen from 1 choice
    [  163.931825] usb 1-1: adding 1-1:1.0 (config #1, interface 0)
    [  163.932603] usb 1-1: adding 1-1:1.1 (config #1, interface 1)
    [  163.933164] usb 1-1: adding 1-1:1.2 (config #1, interface 2)
    [  163.933628] usb 1-1: adding 1-1:1.3 (config #1, interface 3)
    [  163.937247] hub 1-0:1.0: state 7 ports 1 chg 0000 evt 0002
    [  163.937343] hub 1-0:1.0: port 1 enable change, status 00000503
    [  164.161953] uvcvideo 1-1:1.0: usb_probe_interface
    [  164.162021] uvcvideo 1-1:1.0: usb_probe_interface - got id
    [  164.162444] uvcvideo: Found UVC 1.00 device HD Pro Webcam C920 (046d:082d)
    [  164.175683] input: HD Pro Webcam C920 as /devices/ocp.3/47400000.usb/musb-hdrc.1.auto/usb1/1-1/1-1:1.0/input/input1
    [  164.184377] usbcore: registered new interface driver uvcvideo
    [  164.184420] USB Video Class driver (1.1.1)
    [  164.238709] snd-usb-audio 1-1:1.2: usb_probe_interface
    [  164.238797] snd-usb-audio 1-1:1.2: usb_probe_interface - got id
    [  164.437698] usbcore: registered new interface driver snd-usb-audio

Install ffmpeg

    apt-get install ffmpeg
    apt-get install fswebcam
    apt-get install v4l-utils
    apt-get install uvcdynctrl

Capture a JPEG-- good, but blurry

    fswebcam -r 1920x1080 --jpeg 85 -D 1 /var/www/public/static/images/shot.jpg

Try to focus

    v4l2-ctl --all
    Driver Info (not using libv4l2):
    	Driver name   : uvcvideo
    	Card type     : HD Pro Webcam C920
    	Bus info      : usb-musb-hdrc.1.auto-1
    	Driver version: 3.8.13
    	Capabilities  : 0x84000001
    		Video Capture
    		Streaming
    Format Video Capture:
    	Width/Height  : 1920/1080
    	Pixel Format  : 'MJPG'
    	Field         : None
    	Bytes per Line: 0
    	Size Image    : 4147200
    	Colorspace    : SRGB
    Crop Capability Video Capture:
    	Bounds      : Left 0, Top 0, Width 1920, Height 1080
    	Default     : Left 0, Top 0, Width 1920, Height 1080
    	Pixel Aspect: 1/1
    Video input : 0 (Camera 1: ok)
    Streaming Parameters Video Capture:
    	Capabilities     : timeperframe
    	Frames per second: 30.000 (30/1)
    	Read buffers     : 0
    Priority: 2

Test script

    #!/usr/bin/python
    
    import Adafruit_BBIO.GPIO as GPIO
    
    GPIO.setup("P8_14", GPIO.IN)
    while(1):
        GPIO.wait_for_edge("P8_14", GPIO.RISING)
        print("Button smacked! Time to take a picture.")

Install Python bindings for Tumblr

    pip install pytumblr
