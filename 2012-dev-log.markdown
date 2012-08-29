```python
>>> f = open('/dev/spidev1.0', 'r')
>>> f.read()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IOError: [Errno 90] Message too long
```

Minimum SPI speed is 132096120 Hz / 255 = 518024 Hz.

Maximum speed is 132 MHz.