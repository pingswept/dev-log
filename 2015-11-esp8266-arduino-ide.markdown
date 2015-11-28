Install Arduino IDE 1.6.5.

Arduino > Preferences

Enter `http://arduino.esp8266.com/stable/package_esp8266com_index.json` under *Additional Board Manager URLs*.

Under `Tools > Boards > Board Manager`, select `esp8266` and click `Install`.

After install, set `Tools > Board` to ESP 12E Module and `Tools > Port` to `/dev/tty.usbserial-######`

Set `Tools > Programmer` to `esptool`. (Hmmm. That doesn't appear in the menu.)

Need FTDI adapter to talk to ESP8266.
