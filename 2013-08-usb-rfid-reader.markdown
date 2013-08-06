From dmesg:

    usb 1-1: new full speed USB device using at91_ohci and address 2
    input: 深圳市思远创智能设备有限公司
    http://www.sycreader.com USB RFID Reader as /class/input/input1
    generic-usb 0003:08FF:0009.0001: input: USB HID v1.10 Keyboard [深圳市思远创智能设备有限公司
    http://www.sycreader.com USB RFID Reader] on usb-at91-1/input0

Appears in /proc/bus/input/devices

    [root@rascal166$:~]: cat /proc/bus/input/devices 
    I: Bus=0019 Vendor=0001 Product=0001 Version=0100
    N: Name="gpio-keys"
    P: Phys=gpio-keys/input0
    S: Sysfs=/class/input/input0
    U: Uniq=
    H: Handlers=event0 
    B: EV=3
    B: KEY=18 0 0 0 0 0 0 0 0
    
    I: Bus=0003 Vendor=08ff Product=0009 Version=0110
    N: Name="深圳市思远创智能设备有限公司
    http://www.sycreader.com USB RFID Reader"
    P: Phys=usb-at91-1/input0
    S: Sysfs=/class/input/input1
    U: Uniq=version 0106
    H: Handlers=kbd event1 
    B: EV=120013
    B: KEY=e080ffdf 1cfffff ffffffff fffffffe
    B: MSC=10
    B: LED=1f

Reading from /dev/input/event1

    cat /dev/input/event1
    '-      RC?
    '-      R??
    -    R??
              #-	R?
                      -	R?
                              -	R%,#-	R<,-	RD,-	RmK%-	R?K	-	R?K-	R?j%-	R?j	-	R?j-	R?'-	R-?
                                                                                                                                       -	R6?-	R[?'-	Ru?
                                                                                                                                                               -	R|?-	R?? -	R??-	R??-	R?? -	R
                                             ?-	R?-	RC$-	R-	Ri-	R?&$-	R?-	R?&.	R?'.	R?
                                                                                                                      .	R?.	R?"'.	R?"
                                                                                                                                               .	R#.	R4B.	RPB.	RZB.	Rpa.	R?a.	R?a.	RĀ.	R߀.	R?.	R?.	R0?.	R7?.	RU?".	Rp?.	Rz?.	R??".	R??.	R??.	R??(.	R?.	R?.	R:(.	RQ.	RX
The gibberish is event information that could be read with the Python evdev library. See http://stackoverflow.com/a/5060754

But there is an easier way.

    >>> import sys
    >>> sys.stdin = open('/dev/tty0', 'r')
    >>> test = raw_input()
    0680370125
    >>> test
    '0680370125'

### This works ###

Saved this as rfid-test.py, made executable with chmod a+x. Tried unplugging and replugging reader while script is running and while script is stopped; still works.

    #!/usr/bin/env python
    
    import sys
    
    while(1):
    
        sys.stdin = open('/dev/tty0', 'r')
        rfid_data = raw_input()
        print "Read code from RFID reader: {0}".format(rfid_data)
