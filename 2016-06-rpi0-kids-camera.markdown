
Download Jessie Lite with PiTFT kernel from Adafruit: https://adafruit-download.s3.amazonaws.com/2016-03-25_Jessie_Lite_PiTFT22.zip

    unzip 2016-03-25_Jessie_Lite_PiTFT22.zip
    df -h
    unmount /dev/disk2s1
    sudo dd bs=1m if=/Users/brandon/Downloads/2016-03-25-22-lite.img of=/dev/rdisk2
    sudo diskutil eject /dev/rdisk2

Connect a USB keyboard to USB port on Pi Zero with micro USB OTG adapter like this: https://www.amazon.com/Monoprice-Micro-USB-Adapter-109724/dp/B009Z6A0KA

Kernel is Linux 4.1.14+, but it looks like I need kernel 4.4 (as used by Raspbian 2016-05-10) to get usb gadget support (`g_serial`, `g_ether`, and `g_mass_storage` specifically), as suggested by http://blog.gbaman.info/?p=791

Or maybe I can get load the kernel modules manually, but they aren't built into the kernel until 4.4?

Well, at least I have a console. I'll stick with that until I discover that I need internet connectivity, or Adafruit upgrades their PiTFT kernel to 4.4 or newer in a few weeks/months or whatever.
