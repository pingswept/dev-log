### Setting up a Raspberry Pi Zero W to work with Dotstar LED strips ###

Add `enable_uart=1` in `config.txt` on boot partition of SD card.

Connect via UART at 115200 bps.

The Zero W has no Ethernet port, so we need wifi. Use `raspi-config` to set up wifi and SSH.

Log in via SSH.

    sudo apt-get update
    sudo apt-get install git
    git clone https://github.com/adafruit/Adafruit_DotStar_Pi.git
    cd Adafruit_Dotstar_Pi

Slightly modify strandtest.py:

    datapin  = 17
    clockpin = 27
    strip    = Adafruit_DotStar(numpixels, datapin, clockpin, 2500000)

Seems like the strip starts to freak out around a clock speed of 3 MHz. 2.5 MHz seems to work fine. This is about 3 times faster than what my old Fadecandy did with the Neopixel strips (800 kHz). The Fadecandy could only handle 64 pixels smoothly, so maybe this setup will be able to hit 192 pixels, which is 6 m of strip, or $120 of strip.

The 2.5 MHz limit is using a BSS138-based level shifter from Adafruit that claims to max out at 2 MHz. Could probably go faster with http://www.ti.com/product/TXS0102
