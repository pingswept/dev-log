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

### Register the MAC address with the Tufts_Wireless access points ###

Go to http://hostreg.net.tufts.edu/, log in with your UTLN and password, and add a device with the MAC address you got out of your ESP8266.

### Wait more than 5 minutes, but less than 1 year. ###

It takes a few minutes for the list of MACs allowed on the network to update across all the access points. After 1 year, your device will get erased from the list, and you'll need to reregister it.

### Test connection to Tuft_Wireless ###

    /*
        This sketch establishes a TCP connection to a "quote of the day" service.
        It sends a "hello" message, and then prints received data.
    */

    #include <ESP8266WiFi.h>

    #ifndef STASSID
    #define STASSID "Tufts_Wireless"
    #define STAPSK  ""
    #endif

    const char* ssid     = STASSID;
    const char* password = STAPSK;

    const char* host = "djxmmx.net";
    const uint16_t port = 17;

    void setup() {
      Serial.begin(115200);

      // We start by connecting to a WiFi network

      Serial.println();
      Serial.println();
      Serial.print("Connecting to ");
      Serial.println(ssid);

      /* Explicitly set the ESP8266 to be a WiFi-client, otherwise, it by default,
         would try to act as both a client and an access-point and could cause
         network-issues with your other WiFi-devices on your WiFi-network. */
      WiFi.mode(WIFI_STA);
      WiFi.begin(ssid, password);

      while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
      }

      Serial.println("");
      Serial.println("WiFi connected");
      Serial.println("IP address: ");
      Serial.println(WiFi.localIP());
    }

    void loop() {
      Serial.print("connecting to ");
      Serial.print(host);
      Serial.print(':');
      Serial.println(port);

      // Use WiFiClient class to create TCP connections
      WiFiClient client;
      if (!client.connect(host, port)) {
        Serial.println("connection failed");
        delay(5000);
        return;
      }

      // This will send a string to the server
      Serial.println("sending data to server");
      if (client.connected()) {
        client.println("hello from ESP8266");
      }

      // wait for data to be available
      unsigned long timeout = millis();
      while (client.available() == 0) {
        if (millis() - timeout > 5000) {
          Serial.println(">>> Client Timeout !");
          client.stop();
          delay(60000);
          return;
        }
      }

      // Read all the lines of the reply from server and print them to Serial
      Serial.println("receiving from remote server");
      // not testing 'client.connected()' since we do not need to send data here
      while (client.available()) {
        char ch = static_cast<char>(client.read());
        Serial.print(ch);
      }

      // Close the connection
      Serial.println();
      Serial.println("closing connection");
      client.stop();

      delay(300000); // execute once every 5 minutes, don't flood remote service
    }
