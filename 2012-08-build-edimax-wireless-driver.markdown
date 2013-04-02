### Building Realtek driver for Edimax wireless adapter ###

* Prepare kernel build as in http://rascalmicro.com/docs/build-guide.html
* Copy driver source to linux-2.6/drivers/net/wireless/rtl8192cu
* Enable driver with make menuconfig, Device Drivers > Network device support > Realtek 8192C USB Wifi (NEW)

```bash
make menuconfig
make ARCH=arm modules
```
### Testing driver ###

* Copy driver to Rascal

```bash
insmod 8192cu
ifconfig wlan0 up # Brings up the wireless interface
wpa_supplicant -B -c/etc/wpa_supplicant.conf -iwlan0 # This step fails
udhcpc -i wlan0
```

Starting wpa_supplicant results in failed authentication with the following error:
```bash
ioctl[SIOCSIWAP]: Operation not permitted
```

SIOCSIWAP corresponds to "set access point MAC addresses", according to: http://lxr.free-electrons.com/source/include/linux/wireless.h#L263

/etc/wpa_supplicant.conf is still all the default values. Tried altering ap_scan to 0.

```bash
ctrl_interface=/var/run/wpa_supplicant
eapol_version=1
ap_scan=1
fast_reauth=1

network={
    ssid="Ixion"
    psk="pw-removed"
}
```

Here's my full debug log:

```bash
[root@rascal14:~]: wpa_supplicant -d -B -c/etc/wpa_supplicant.conf -iwlan0
Initializing interface 'wlan0' conf '/etc/wpa_supplicant.conf' driver 'default' ctrl_interface 'N/A' bridge 'N/A'
Configuration file '/etc/wpa_supplicant.conf' -> '/etc/wpa_supplicant.conf'
Reading configuration file '/etc/wpa_supplicant.conf'
ctrl_interface='/var/run/wpa_supplicant'
eapol_version=1
ap_scan=1
fast_reauth=1
Priority group 0
   id=0 ssid='Ixion'
SIOCGIWRANGE: WE(compiled)=22 WE(source)=16 enc_capa=0xf
  capabilities: key_mgmt 0xf enc 0xf flags 0x0
ioctl[SIOCSIWAP]: Operation not permitted
WEXT: Failed to set bogus BSSID/SSID to disconnect
netlink: Operstate: linkmode=1, operstate=5
Own MAC address: 80:1f:02:68:67:3f
wpa_driver_wext_set_key: alg=0 key_idx=0 set_tx=0 seq_len=0 key_len=0
wpa_driver_wext_set_key: alg=0 key_idx=1 set_tx=0 seq_len=0 key_len=0
wpa_driver_wext_set_key: alg=0 key_idx=2 set_tx=0 seq_len=0 key_len=0
wpa_driver_wext_set_key: alg=0 key_idx=3 set_tx=0 seq_len=0 key_len=0
wpa_driver_wext_set_countermeasures
RSN: flushing PMKID list in the driver
Setting scan request: 0 sec 100000 usec
EAPOL: SUPP_PAE entering state DISCONNECTED
EAPOL: Supplicant port status: Unauthorized
EAPOL: KEY_RX entering state NO_KEY_RECEIVE
EAPOL: SUPP_BE entering state INITIALIZE
EAP: EAP entering state DISABLED
EAPOL: Supplicant port status: Unauthorized
EAPOL: Supplicant port status: Unauthorized
Using existing control interface directory.
ctrl_iface bind(PF_UNIX) failed: Address already in use
ctrl_iface exists, but does not allow connections - assuming it was leftover from forced program termination
Successfully replaced leftover ctrl_iface socket '/var/run/wpa_supplicant/wlan0'
Added interface wlan0
Daemonize..
```
