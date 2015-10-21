Install some Iceweasel extensions

* Lastpass
* Adblock Plus
* Ghostery

Update repositories

    sudo apt-get update
    sudo apt-get install pianobar

Audio doesn't work yet. Might be a mechanical problem with headphone plug.

Moving windows is kind of laggy, probably because UseGPU option is set to false in `/usr/share/X11/xorg.conf.d/60-novena.conf`

Increase font size from Sans 10 to Sans 12 in Applications Menu > Settings > Appearance > Fonts

Install Chromium based off http://www.kosagi.com/forums/viewtopic.php?id=299. Each tab is a different process, so we can use all 4 cores, which makes it faster than Firefox.

    wget https://launchpad.net/ubuntu/+source/chromium-browser/43.0.2357.130-0ubuntu2/+build/7719027/+files/chromium-browser_43.0.2357.130-0ubuntu2_armhf.deb
    wget https://launchpad.net/ubuntu/+source/chromium-browser/43.0.2357.130-0ubuntu2/+build/7719027/+files/chromium-codecs-ffmpeg_43.0.2357.130-0ubuntu2_armhf.deb
    wget https://launchpad.net/ubuntu/+source/chromium-browser/43.0.2357.130-0ubuntu2/+build/7719027/+files/chromium-codecs-ffmpeg-extra_43.0.2357.130-0ubuntu2_armhf.deb
    sudo dpkg -i chromium-*.deb
