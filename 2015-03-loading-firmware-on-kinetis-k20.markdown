Testing with a Fadecandy, which uses a `MK20DX128VLF5`

From `dmesg`

    [961075.924410] usb 1-1.4.2: new full-speed USB device number 11 using ci_hdrc
    [961076.036389] usb 1-1.4.2: New USB device found, idVendor=1d50, idProduct=607a
    [961076.043560] usb 1-1.4.2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    [961076.051230] usb 1-1.4.2: Product: Fadecandy
    [961076.055568] usb 1-1.4.2: Manufacturer: scanlime
    [961076.060198] usb 1-1.4.2: SerialNumber: KGAUESRHTFYHWPUV

Fadecandy recommends using `dfu-util` here: https://github.com/scanlime/fadecandy/blob/master/bin/README.md

### Using `dfu-util` ###

dfu-util webpage is usually down, so check the archive: https://web.archive.org/web/20140913013358/http://dfu-util.gnumonks.org/
    ➜  ~  dfu-util -l
    dfu-util 0.8
    dfu-util: Cannot open DFU device 1d50:607a
