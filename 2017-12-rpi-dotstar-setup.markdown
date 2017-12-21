Add enable_uart=1 in config.txt on boot partition of SD card.

Connect via UART at 115200 bps.

Use raspi-config to set up wifi and SSH.

Log in via SSH.

sudo apt-get update
sudo apt-get install git
git clone https://github.com/adafruit/Adafruit_DotStar_Pi.git
