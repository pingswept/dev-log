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

### Building LAPACK ###

Need to set ARCH to arm-angstrom-linux-gnueabi-ar and ranlib to arm-angstrom-linux-gnueabi-ranlib in make.inc.

```makefile
ARCH     = arm-angstrom-linux-gnueabi-ar
ARCHFLAGS= cr
RANLIB   = arm-angstrom-linux-gnueabi-ranlib
```

```sh
tar xzvf lapack-3.4.1.tgz
cd lapack-3.4.1/SRC
make
gfortran  -shared -O2 -fPIC -c sgbbrd.f -o sgbbrd.o
gfortran  -shared -O2 -fPIC -c sgbcon.f -o sgbcon.o
gfortran  -shared -O2 -fPIC -c sgbequ.f -o sgbequ.o
gfortran  -shared -O2 -fPIC -c sgbrfs.f -o sgbrfs.o
gfortran  -shared -O2 -fPIC -c sgbsv.f -o sgbsv.o
gfortran  -shared -O2 -fPIC -c sgbsvx.f -o sgbsvx.o
gfortran  -shared -O2 -fPIC -c sgbtf2.f -o sgbtf2.o
gfortran  -shared -O2 -fPIC -c sgbtrf.f -o sgbtrf.o
```