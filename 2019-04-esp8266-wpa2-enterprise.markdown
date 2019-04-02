### Connecting an ESP8266 to the Tufts_Secure wifi network at Tufts University ###

### (DOES NOT WORK AS OF 2019-04-2) ###

(It might be easier to just make a wifi hotspot with your phone, and then connect both the ESP8266 and your laptop to that.)

Basic info as of April 2019

    SSID:	Tufts_Secure
    Protocol:	802.11ac
    Security type:	WPA2-Enterprise
    Type of sign-in info:	Microsoft: Protected EAP (PEAP)
    Network band:	5 GHz
    Network channel:	56
    Link-local IPv6 address:	fe80::6898:126d:b462:acc5%19
    IPv4 address:	10.244.140.62
    IPv4 DNS servers:	10.246.104.56
    10.250.104.56
    Primary DNS suffix:	tufts.edu
    Manufacturer:	Marvell Semiconductor, Inc.
    Description:	Marvell AVASTAR Wireless-AC Network Controller
    Driver version:	15.68.9125.57
    Physical address (MAC):	98-5F-D3-C6-5F-A7

Best guess of code:

    #include <ESP8266WiFi.h>

    extern "C" {
    #include "wpa2_enterprise.h"
    }


    // SSID to connect to
    static const char* ssid = "Tufts_Secure";
    // Username for authentification
    static const char* username = "Nolop_IOT";
    // Password for authentication
    static const char* password = "the password goes here";

    char buff[20];
    String ip;

    void setup() {
        Serial.begin(115200);
        wifi_set_opmode(STATION_MODE);
        struct station_config wifi_config;
        memset(&wifi_config, 0, sizeof(wifi_config));
        strcpy((char*)wifi_config.ssid, ssid);
        wifi_station_set_config(&wifi_config);
        wifi_station_set_wpa2_enterprise_auth(1);
        wifi_station_set_enterprise_username((uint8*)username, strlen(username));
        wifi_station_set_enterprise_password((uint8*)password, strlen(password));
        wifi_station_connect();
    }

    void loop() {
        Serial.println();
        Serial.println("Waiting for connection and IP Address from DHCP");
        while (WiFi.status() != WL_CONNECTED) {
            delay(2000);
            Serial.print(".");
        }
        Serial.println("");
        Serial.println("WiFi connected");
        Serial.println("IP address: ");
        Serial.println(WiFi.localIP());
        IPAddress myAddr = WiFi.localIP();
        sprintf(buff, "%d.%d.%d.%d", myAddr[0], myAddr[1], myAddr[2], myAddr[3]);
        ip = String(buff);
        Serial.println(ip);
    }

Mostly based on https://github.com/jtuttas/ESP8266-WPA2-Enterprise/blob/master/ino/webserver/webserver.ino
Also relevant to your interests: https://github.com/espressif/ESP8266_NONOS_SDK/blob/release/v3.0.0/examples/wpa2_enterprise/user/user_main.c
