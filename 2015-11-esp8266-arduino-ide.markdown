Install Arduino IDE 1.6.5.

Arduino > Preferences

Enter `http://arduino.esp8266.com/stable/package_esp8266com_index.json` under *Additional Board Manager URLs*.

Tools > Boards > Board Manager

Select `esp8266` and click `Install`.

After install, set `Tools > Board` to ESP 12E Module and `Tools > Port` to `/dev/######-usbserial`

Need FTDI adapter to talk to ESP8266.
