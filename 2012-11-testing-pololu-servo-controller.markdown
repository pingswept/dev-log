
    usb 1-1: new full speed USB device using at91_ohci and address 2
    usb 1-1: USB disconnect, address 2
    usb 1-1: new full speed USB device using at91_ohci and address 3
    [root@rascal14:~]: lsusb -v
    
    Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            9 Hub
      bDeviceSubClass         0 Unused
      bDeviceProtocol         0 Full speed (or root) hub
      bMaxPacketSize0        64
      idVendor           0x1d6b Linux Foundation
      idProduct          0x0001 1.1 root hub
      bcdDevice            2.06
      iManufacturer           3 Linux 2.6.36+ ohci_hcd
      iProduct                2 AT91 OHCI
      iSerial                 1 at91
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           25
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0xe0
          Self Powered
          Remote Wakeup
        MaxPower                0mA
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         9 Hub
          bInterfaceSubClass      0 Unused
          bInterfaceProtocol      0 Full speed (or root) hub
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0002  1x 2 bytes
            bInterval             255
    Hub Descriptor:
      bLength               9
      bDescriptorType      41
      nNbrPorts             2
      wHubCharacteristic 0x0012
        No power switching (usb 1.0)
        No overcurrent protection
      bPwrOn2PwrGood        2 * 2 milli seconds
      bHubContrCurrent      0 milli Ampere
      DeviceRemovable    0x00
      PortPwrCtrlMask    0xff
     Hub Port Status:
       Port 1: 0000.0103 power enable connect
       Port 2: 0000.0100 power
    Device Status:     0x0003
      Self Powered
      Remote Wakeup Enabled
    
    Bus 001 Device 003: ID 1ffb:0089  
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               2.00
      bDeviceClass          239 Miscellaneous Device
      bDeviceSubClass         2 ?
      bDeviceProtocol         1 Interface Association
      bMaxPacketSize0         8
      idVendor           0x1ffb 
      idProduct          0x0089 
      bcdDevice            1.01
      iManufacturer           1 Pololu Corporation
      iProduct                2 Pololu Micro Maestro 6-Servo Controller
      iSerial                 5 00039612
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength          150
        bNumInterfaces          5
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0xc0
          Self Powered
        MaxPower              100mA
        Interface Association:
          bLength                 8
          bDescriptorType        11
          bFirstInterface         0
          bInterfaceCount         2
          bFunctionClass          2 Communications
          bFunctionSubClass       2 Abstract (modem)
          bFunctionProtocol       1 AT-commands (v.25ter)
          iFunction               3 Pololu Micro Maestro 6-Servo Controller Command Port
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         2 Communications
          bInterfaceSubClass      2 Abstract (modem)
          bInterfaceProtocol      1 AT-commands (v.25ter)
          iInterface              0 
          CDC Header:
            bcdCDC               1.20
          CDC ACM:
            bmCapabilities       0x02
              line coding and serial state
          CDC Union:
            bMasterInterface        0
            bSlaveInterface         1 
          CDC Call Management:
            bmCapabilities       0x00
            bDataInterface          1
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x000a  1x 10 bytes
            bInterval               1
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        1
          bAlternateSetting       0
          bNumEndpoints           2
          bInterfaceClass        10 CDC Data
          bInterfaceSubClass      0 Unused
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x02  EP 2 OUT
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x82  EP 2 IN
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
        Interface Association:
          bLength                 8
          bDescriptorType        11
          bFirstInterface         2
          bInterfaceCount         2
          bFunctionClass          2 Communications
          bFunctionSubClass       2 Abstract (modem)
          bFunctionProtocol       1 AT-commands (v.25ter)
          iFunction               4 Pololu Micro Maestro 6-Servo Controller TTL Port
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        2
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         2 Communications
          bInterfaceSubClass      2 Abstract (modem)
          bInterfaceProtocol      1 AT-commands (v.25ter)
          iInterface              0 
          CDC Header:
            bcdCDC               1.20
          CDC ACM:
            bmCapabilities       0x02
              line coding and serial state
          CDC Union:
            bMasterInterface        2
            bSlaveInterface         3 
          CDC Call Management:
            bmCapabilities       0x00
            bDataInterface          3
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x83  EP 3 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x000a  1x 10 bytes
            bInterval               1
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        3
          bAlternateSetting       0
          bNumEndpoints           2
          bInterfaceClass        10 CDC Data
          bInterfaceSubClass      0 Unused
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x04  EP 4 OUT
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x84  EP 4 IN
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        4
          bAlternateSetting       0
          bNumEndpoints           0
          bInterfaceClass       255 Vendor Specific Class
          bInterfaceSubClass      4 
          bInterfaceProtocol      1 
          iInterface              2 Pololu Micro Maestro 6-Servo Controller
    Device Status:     0x0000
      (Bus Powered)
    [root@rascal14:~]: lsusb
    Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 001 Device 003: ID 1ffb:0089
