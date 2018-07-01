### Set up Raspberry Pi 3 ###

 * Burn Raspbian Stretch Lite to SD card.
 * Add file called `ssh` to boot partition on SD card.
 * Connect SHT31-D temperature sensor as in http://www.pibits.net/code/raspberry-pi-sht31-sensor-example.php
 * Connect relay to BCM26 and ground: https://www.amazon.com/Iot-Relay-Enclosed-High-power-Raspberry/dp/B00WV7GMA2
 * SSH into Pi
 * Update default password
 * `sudo apt-get install python-smbus`
 * Enable I2C bus using `sudo raspi-config`

Check that the sensor is working.

    pi@raspberrypi:~ $ i2cdetect 1
    WARNING! This program can confuse your I2C bus, cause data loss and worse!
    I will probe file /dev/i2c-1.
    I will probe address range 0x03-0x77.
    Continue? [Y/n] y
         0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
    00:          -- -- -- -- -- -- -- -- -- -- -- -- --
    10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    40: -- -- -- -- 44 -- -- -- -- -- -- -- -- -- -- --
    50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    70: -- -- -- -- -- -- -- --

Run Python test code from http://www.pibits.net/code/raspberry-pi-sht31-sensor-example.php

    import smbus
    import time

    # Get I2C bus
    bus = smbus.SMBus(1)

    # SHT31 address, 0x44(68)
    bus.write_i2c_block_data(0x44, 0x2C, [0x06])

    time.sleep(0.5)

    # SHT31 address, 0x44(68)
    # Read data back from 0x00(00), 6 bytes
    # Temp MSB, Temp LSB, Temp CRC, Humididty MSB, Humidity LSB, Humidity CRC
    data = bus.read_i2c_block_data(0x44, 0x00, 6)

    # Convert the data
    temp = data[0] * 256 + data[1]
    cTemp = -45 + (175 * temp / 65535.0)
    fTemp = -49 + (315 * temp / 65535.0)
    humidity = 100 * (data[3] * 256 + data[4]) / 65535.0

    # Output data to screen
    print "Temperature in Celsius is : %.2f C" %cTemp
    print "Temperature in Fahrenheit is : %.2f F" %fTemp
    print "Relative Humidity is : %.2f %%RH" %humidity

If you see:

    pi@raspberrypi:~ $ sudo python sht31.py
    Traceback (most recent call last):
      File "sht31.py", line 5, in <module>
        bus = smbus.SMBus(1)
    IOError: [Errno 2] No such file or directory

that probably means you forgot to enable the I2C bus with `raspi-config`.

### Install `supervisor` ###

`sudo apt-get install supervisor`

Add to `/etc/supervisor/conf.d/ventilate.conf`:

    [program:ventilate]
    directory=/home/pi
    command=python ventilate.py

Add contents of https://gist.github.com/pingswept/6238c24461965ef0ccd98426e975309b to `/home/pi/ventilate.py`

### Connect to Adafruit IO ###

    sudo apt-get install python-pip
    sudo pip install adafruit-io
