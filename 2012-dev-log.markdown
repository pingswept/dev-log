```python
>>> f = open('/dev/spidev1.0', 'r')
>>> f.read()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IOError: [Errno 90] Message too long
```

Minimum SPI speed is 132096120 Hz / 255 = 518024 Hz.

Maximum speed is 132 MHz.

```bash
[root@rascal14:~]: cat /etc/init.d/rascal-gpio.sh 
for pin in 55 56 64 65 66 67 68 69 70 71 72 73 96 97 98 99 100 101 107
do
    echo $pin > /sys/class/gpio/export
    echo out > /sys/class/gpio/gpio$pin/direction
done
```