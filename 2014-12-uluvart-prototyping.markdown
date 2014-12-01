Python code on BBB to send data to Arduino

    import serial
    >>> ser = serial.Serial(port = "/dev/ttyACM1", baudrate=9600)
    >>> ser.write(chr(128) + chr(128) + chr(128))
    3

Arduino sometimes shows up as `/dev/ttyACM0`, sometimes as `/dev/ttyACM1`.
