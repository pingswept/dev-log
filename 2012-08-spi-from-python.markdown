### Testing SPI ###

Sends data out MOSI and pulses clock at ~15 MHz, but chip select stays low. Expect SS0 on PB3, AKA M16. (Checked SS0, SS1, SS2, SS3.)

```bash
echo "Aaron Burr" > /dev/spidev1.0
```

Testing with an Arduino SPI slave using code from http://www.gammon.com.au/forum/?id=10892

With the slave code modified to send each character out the serial port immediately, rather than waiting for a newline character, it almost works. Characters are returned, but they're not the right ones. Probably the Rascal max speed of ~15 MHz is too fast for the Arduino. The Arduino can only transmit at 4 MHz. Receive speed might be even slower.

Tried to change driver to slower max speed. Seemed to compile fine.

```bash
.max_speed_hz   = 15 * 1000 * 10,
```

But, error in dmesg when driver was loaded.

```bash
atmel_spi atmel_spi.1: Atmel SPI Controller at 0xfffcc000 (irq 13)
atmel_spi atmel_spi.1: can't setup spi1.0, status -22
atmel_spi atmel_spi.1: can't setup spi1.1, status -22
atmel_spi atmel_spi.1: can't setup spi1.2, status -22
```

Also, could be problem with 5 V Arduino not registering 3.3 V signals.

(Later discovered it worked fine, but the chip select started way early and ended way late, so it appeared to be eternally low at first glance.)

### Changing SPI speed ###

Master clock is 132 MHZ; CPU is 396 MHZ (3x MCK); crystal is 18.432 MHZ (MCK/7.16?).

SPI clock is MCK divided by 8 bit value stored in the SCBR field, which is part of the SPI_CSR0-SPI_CSR3 registers.

132/9 = 14.67 MHz, appears to be what is produced by 15 * 1000 * 1000 in driver code.

Minimum speed is 132/255 = 518 kHz.

```c
// Written by Nick Gammon, slightly modified by Brandon
// February 2011

#include <SPI.h>

char buf [100];
volatile byte pos;
volatile boolean process_it;

void setup (void)
{
  Serial.begin (115200);   // debugging

  // have to send on master in, *slave out*
  pinMode(MISO, OUTPUT);
  
  // turn on SPI in slave mode
  SPCR |= _BV(SPE);
  
  // get ready for an interrupt 
  pos = 0;   // buffer empty
  process_it = false;

  // now turn on interrupts
  SPI.attachInterrupt();

}  // end of setup


// SPI interrupt routine
ISR (SPI_STC_vect)
{
byte c = SPDR;  // grab byte from SPI Data Register
  
  // add to buffer if room
  if (pos < sizeof buf)
    {
    buf [pos++] = c;

    if (c == '!') \\ This line is weird. Requires two !! to trigger. Whatever.
      process_it = true;
      
    }  // end of room available
}  // end of interrupt routine SPI_STC_vect

// main loop - wait for flag set in interrupt routine
void loop (void)
{
  if (process_it)
    {
    buf [pos] = 0;  
    Serial.println (buf);
    pos = 0;
    process_it = false;
    }  // end of flag set
    
}  // end of loop

```

New version of spiWrite in Python

```python
def spiWrite(val, cs='0'):
    with open('/dev/spidev1.' + str(cs), 'w') as f:
        f.write(val)
        f.close()
```

Also, from Bash

```bash
echo -n "Text goes here" > > /dev/spidev1.0
```

The -n option prevents the emission of a newline character.

### More SPI speed stuff ###

```c
#define SPI_IOC_RD_MAX_SPEED_HZ         _IOR(SPI_IOC_MAGIC, 4, __u32)
#define SPI_IOC_WR_MAX_SPEED_HZ         _IOW(SPI_IOC_MAGIC, 4, __u32)

#define _IOR(type,nr,size)      _IOC(_IOC_READ,(type),(nr),sizeof(size))

#define _IOC(dir,type,nr,size) \
         (((dir)  << _IOC_DIRSHIFT) | \
          ((type) << _IOC_TYPESHIFT) | \
          ((nr)   << _IOC_NRSHIFT) | \
          ((size) << _IOC_SIZESHIFT))
```

SPI_IOC_MAGIC is defined as 'k', which is 0x6B or 107.

From include/asm-generic/ioctl.h
```c
#define _IOC_NRBITS     8
#define _IOC_TYPEBITS   8
#define _IOC_SIZEBITS   14 // This can be overridden, but appears not to be for ARM arch
#define _IOC_READ       2U // U means unsigned int
#define _IOC_WRITE      1U // U means unsigned int
#define _IOC_NRSHIFT    0
#define _IOC_TYPESHIFT  (_IOC_NRSHIFT+_IOC_NRBITS)      // 8 for ARM
#define _IOC_SIZESHIFT  (_IOC_TYPESHIFT+_IOC_TYPEBITS)  // 16 for ARM
#define _IOC_DIRSHIFT   (_IOC_SIZESHIFT+_IOC_SIZEBITS)  // 30 for ARM
```


```c
SPI_IOC_RD_MAX_SPEED_HZ
_IOR(SPI_IOC_MAGIC, 4, __u32)
_IOR(0x6B, 4 __u32)
_IOR(type,nr,size)      _IOC(_IOC_READ,(type),(nr),sizeof(size))
_IOC(_IOC_READ, 0x6B, 4, sizeof(__u32))
(((dir)  << _IOC_DIRSHIFT) | ((type) << _IOC_TYPESHIFT) | ((nr)   << _IOC_NRSHIFT) | ((size) << _IOC_SIZESHIFT))
(((_IOC_READ)  << _IOC_DIRSHIFT) | ((0x6B) << _IOC_TYPESHIFT) | ((4)   << _IOC_NRSHIFT) | ((4) << _IOC_SIZESHIFT))
((2 << 30) | (0x6B << 8) | (4 << 0) | (4 << 16))
((2 << 30) | (0x6B << 8) | (0x04 << 0) | (0x04 << 16))
0x80046B04
```

Reading maximum speed

```python
>>> import array, fcntl
>>> speed = array.array('i', [0])
>>> fcntl.ioctl(f, 0x80046B04, speed)
0
>>> print speed[0]
528000
```

This is the result of setting the max speed to 528 kbps in arch/arm/mach-at91/board-rascal.c
```c
.max_speed_hz   = 528 * 1000
```

Get/set code
```python
def spiGetSpeed(channel='0'):
    import array, fcntl
    with open('/dev/spidev1.' + str(channel), 'w') as f:
        speed = array.array('i', [0])
        fcntl.ioctl(f, 0x80046B04, speed)
        return speed[0]

def spiSetSpeed(speed, channel='0'):
    import array, fcntl
    with open('/dev/spidev1.' + str(channel), 'w') as f:
        return fcntl.ioctl(f, 0x40046B04, array.array('i', [int(speed)]))
```
