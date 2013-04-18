From Mike:

To enable interrupts there are two steps:

1.  Download and install the gpio-event kernel module like this:

        wget http://goelzer.com/gpio-event/gpio-event-drv.ko
        insmod gpio-event.ko
        echo "in" > /sys/class/gpio/gpio70/direction    # <-- i'm not positive this line is necessary any more but it won't hurt

2.  Restart uwsgi.  You should now *not* see any message about "Experimental interrupt support disabled" because the driver is now loaded.

After completing the above steps, put something like this in server.py:

@pin_rising(70)
def on_pin70_rising(v):
        print "Saw a rising edge on pin 70, toggling pin 11 in response"
        pytronics.toggle(11)

The function on_pin70_rising() should now run whenever gpio 70 (pin #3 in the Arduino numbering scheme) is pulled high.  There is another possible decorator called @pin_falling that can be used in an analogous way to see the falling edge.
