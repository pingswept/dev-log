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

but when building with make arch=ARM modules:

    drivers/net/wireless/rt73/rtmp_main.c: In function 'usb_rtusb_probe':
    drivers/net/wireless/rt73/rtmp_main.c:1153: error: 'struct net_device' has no member named 'open'
    drivers/net/wireless/rt73/rtmp_main.c:1154: error: 'struct net_device' has no member named 'stop'
    drivers/net/wireless/rt73/rtmp_main.c:1156: error: 'struct net_device' has no member named 'hard_start_xmit'
    drivers/net/wireless/rt73/rtmp_main.c:1157: error: 'struct net_device' has no member named 'get_stats'
    drivers/net/wireless/rt73/rtmp_main.c:1165: error: 'struct net_device' has no member named 'do_ioctl'
    make[4]: *** [drivers/net/wireless/rt73/rtmp_main.o] Error 1
    make[3]: *** [drivers/net/wireless/rt73] Error 2
    make[2]: *** [drivers/net/wireless] Error 2
    make[1]: *** [drivers/net] Error 2
    make: *** [drivers] Error 2

This appears to be because this driver was targeting kernel 2.6.27, but we're building against 2.6.36. As of 2.6.31, "[the] kernel removes backwards compatibility for the old network driver api. Only the new net_driver_ops structure api is supported," according to https://www.virtualbox.org/ticket/4264

The good news is that, according to https://bbs.archlinux.org/viewtopic.php?id=95784, "You don't need to install any drivers to get rt73 (ralink) working, as the open rt2x00 drivers are included into the kernel since 2.6.24. You just have to install the firmware, which is included in the package rt2x00-rt71w-fw"

That's the next step. Maybe also double-check that Ralink hasn't released a newer version of the rt73 drivers that would work with 2.6.36.
