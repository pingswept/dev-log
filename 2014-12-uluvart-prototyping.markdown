Python code on BBB to send data to Arduino

    import serial
    >>> ser = serial.Serial(port = "/dev/ttyACM1", baudrate=9600)
    >>> ser.write(chr(128) + chr(128) + chr(128) + '\n')
    4

Arduino sometimes shows up as `/dev/ttyACM0`, sometimes as `/dev/ttyACM1`.

Need to give www-data user access to serial port.

    usermod -a -G dialout www-data

Arduino code: https://gist.github.com/pingswept/19c994d4076932ce4375

### Creating tunnel ###

    autossh -i /root/.ssh/id_rsa -M 12300 -R *:12345:localhost:80 sms@rascalmicro.com -N
