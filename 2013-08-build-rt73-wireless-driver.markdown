Unzip driver archive into linux-2.6/drivers/net/wireless/rt73/

Because we are building for 2.6 kernel:

    cp Makefile.6 Makefile

Add this as linux-2.6/drivers/net/wireless/rt73/

    config RT73
    	tristate "Ralink RT73 USB WiFi"
    	depends on USB
    	---help---
    	  This driver implements basic IEEE802.11. Infrastructure and adhoc mode
              with open or shared or WPA-PSK or WPA2-PSK authentication method.
              NONE, WEP, TKIP and AES encryption.

Add to linux-2.6/drivers/net/wireless/Kconfig

    source "drivers/net/wireless/rt73/Kconfig"

Add to linux-2.6/drivers/net/wireless/Makefile

    obj-$(CONFIG_RT73) += rt73/
