### Compiler ###

Already have avrdude and other avr tools as part of Arduino IDE, but it's not clear that just adding them to the path would make them all work outside of the IDE.

Instead, install CrossPack-AVR-20131216.dmg from http://www.obdev.at/products/crosspack/download.html

Build fails on OS X

    make
    make: *** No rule to make target `power.elf', needed by `all'.  Stop.

### Successful build on BBB (not OS X) ###

    root@rascal-yellow:~/PowerCape/avr# make
    avr-gcc -g -Wall -Os -mmcu=atmega328p -I. -Dpowercape -DF_CPU=1000000   -c -o main.o main.c
    avr-gcc -g -Wall -Os -mmcu=atmega328p -I. -Dpowercape -DF_CPU=1000000   -c -o board.o board.c
    board.c: In function ‘board_gpio_config’:
    board.c:172:5: warning: large integer implicitly truncated to unsigned type [-Woverflow]
    board.c:178:5: warning: large integer implicitly truncated to unsigned type [-Woverflow]
    avr-gcc -g -Wall -Os -mmcu=atmega328p -I. -Dpowercape -DF_CPU=1000000   -c -o twi_slave.o twi_slave.c
    avr-gcc -g -Wall -Os -mmcu=atmega328p -I. -Dpowercape -DF_CPU=1000000   -c -o registers.o registers.c
    registers.c: In function ‘registers_host_write’:
    registers.c:151:13: warning: dereferencing type-punned pointer will break strict-aliasing rules [-Wstrict-aliasing]
    avr-gcc -g -Wall -Os -mmcu=atmega328p -I. -Dpowercape -DF_CPU=1000000   -c -o eeprom.o eeprom.c
    avr-gcc -g -Wall -Os -mmcu=atmega328p -I. -Dpowercape -DF_CPU=1000000 -Wl,-Map,powercape.map -o powercape.elf main.o board.o twi_slave.o registers.o eeprom.o 
    avr-objdump -h -S powercape.elf > powercape.lst
    avr-size powercape.elf
       text	   data	    bss	    dec	    hex	filename
       2734	      0	     41	   2775	    ad7	powercape.elf
    avr-objcopy -j .text -j .data -O ihex powercape.elf powercape.hex
    avr-objcopy -j .text -j .data -O binary powercape.elf powercape.bin
    avr-objcopy -j .text -j .data -O srec powercape.elf powercape.srec
    rm registers.o board.o eeprom.o twi_slave.o main.o

### Examining setup on BBB ###

Headers appear in /usr/lib/avr/include/avr

### Working setup on OS X ###

Turns out that the latest commit in Github was a little screwy. Now branching off the commit that built on the BBB.

    git checkout bbfcfedc73b6f0b3d5f7276751c8ea42ad1410ea

Planning to fork the repo and branch there.
