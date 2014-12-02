Python code on BBB to send data to Arduino

    import serial
    >>> ser = serial.Serial(port = "/dev/ttyACM1", baudrate=9600)
    >>> ser.write(chr(128) + chr(128) + chr(128) + '\n')
    4

Arduino sometimes shows up as `/dev/ttyACM0`, sometimes as `/dev/ttyACM1`.

Need to give www-data user access to serial port.

Probably more than is necessary.

    usermod -a -G sudo www-data
    sudo chmod a+rw /dev/ttyACM0

Could probably just do:

    usermod -a -G dialout www-data

Arduino code: https://gist.github.com/pingswept/19c994d4076932ce4375
