
Download Jessie Lite with PiTFT kernel from Adafruit: https://adafruit-download.s3.amazonaws.com/2016-03-25_Jessie_Lite_PiTFT22.zip

    unzip 2016-03-25_Jessie_Lite_PiTFT22.zip
    df -h
    unmount /dev/disk2s1
    sudo dd bs=1m if=/Users/brandon/Downloads/2016-03-25-22-lite.img of=/dev/rdisk2
