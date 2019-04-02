### Get MAC address ###

#include <ESP8266WiFi.h>
 
void setup(){
   Serial.begin(115200);
   delay(500);
 
   Serial.println();
   Serial.print("MAC: ");
   Serial.println(WiFi.macAddress());
}
 
void loop(){
    // Do nothing.
    // Go look at the serial monitor to see what MAC address was printed out.
    // Make sure you set the speed in the serial monitor's menu to "115200 baud".
}
