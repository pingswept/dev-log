Install Arduino IDE 1.6.7.

Arduino > Preferences

Enter `http://arduino.esp8266.com/stable/package_esp8266com_index.json` under *Additional Board Manager URLs*.

(Tested with version 2.0.0.)

Under `Tools > Boards > Board Manager`, select `esp8266` and click `Install`.

After install, set `Tools > Board` to `NodeMCU 1.0 (ESP-12E Module)` and `Tools > Port` to `/dev/tty.usbserial-######`

Set `Tools > Programmer` to `esptool`. (Hmmm. That doesn't appear in the menu. Used AVRISP mkII)

Need 3.3 V FTDI adapter to talk to ESP8266.

Connect adapter to ESP8266 with TX -> RX, RX -> TX, and GND -> GND.

Power ESP8266 with 3.3 V; connect ground in common with FTDI adapter.

Connect `CH_PD` to 3.3 V.

Connect `GPIO_0` to ground to start bootloader mode.

After uploading, code will start executing.

If you try to upload again, i.e. when not in bootloader mode, the Arduino IDE will report:

    warning: espcomm_sync failed
    error: espcomm_open failed
    error: espcomm_upload_mem failed

To reenter bootloader mode, cycle power on ESP8266. Then you're ready to upload again.

### Blink example ###

    int led = 2; // At least for ESP-12E, pin 2 has an LED attached.

    void setup() {                
        pinMode(led, OUTPUT);
    }

    void loop() {
        digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
        delay(1000);               // wait for a second
        digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
        delay(1000);               // wait for a second
    }
