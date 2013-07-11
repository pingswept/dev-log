    opkg install alsa-utils-speakertest

    speaker-test -t sine -f440

/usr/share/sounds/alsa/Noise.wav works, but calm.wav does not.

Noise.wav: mono, sample rate 48000 Hz, sample format 32-bit float
calm.wav: mono, sample rate 11025 Hz, sample format 32-bit float

    opkg install alsa-utils-aplay

    aplay -t wav calm.wav 
    Playing WAVE 'calm.wav' : Unsigned 8 bit, Rate 11025 Hz, Mono

    aplay -t wav calm-16-bit-pcm.wav 
    Playing WAVE 'calm-16-bit-pcm.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono


BUT:

    aplay -t wav asshole.wav 
    aplay: test_wavefile:876: can't play WAVE-file format 0x0055 which is not PCM or FLOAT encoded

    opkg install alsa-utils # figure this package might be useful
    opkg install python-pyalsaaudio

### Background ###

OSS is original Linux sound system used in 2.4 and 2.5 kernels.

ALSA replaced OSS in 2.6 kernel around 2004.

Pulseaudio is a newer system that can coexist with OSS and ALSA. It's now the most common system in Ubuntu, Fedora, etc.

### Gstreamer ###

    opkg install gstreamer
    Package gstreamer (0.10.32-r0.6) installed in root is up to date.

    opkg install python-gst
    Installing python-gst (0.10.17-r0.5) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/python/python-gst_0.10.17-r0.5_armv5te.ipk.
    Installing libgstpbutils-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstpbutils-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstinterfaces-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstinterfaces-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstaudio-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstaudio-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgsttag-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgsttag-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Configuring libgstpbutils-0.10-0.
    Configuring libgstinterfaces-0.10-0.
    Configuring libgstaudio-0.10-0.
    Configuring libgsttag-0.10-0.
    Configuring python-gst.

Gst-inspect

    gst-inspect --version
    gst-inspect-0.10 version 0.10.32
    GStreamer 0.10.32
    Unknown package origin
    [root@rascal14$:~]: gst-inspect-0.10 --version
    gst-inspect-0.10 version 0.10.32
    GStreamer 0.10.32
    Unknown package origin
    
    gst-inspect
    coreindexers:  memindex: A index that stores entries in memory
    coreindexers:  fileindex: A index that stores entries in file
    coreelements:  capsfilter: CapsFilter
    coreelements:  fakesrc: Fake Source
    coreelements:  fakesink: Fake Sink
    coreelements:  fdsrc: Filedescriptor Source
    coreelements:  fdsink: Filedescriptor Sink
    coreelements:  filesrc: File Source
    coreelements:  identity: Identity
    coreelements:  input-selector: Input selector
    coreelements:  output-selector: Output selector
    coreelements:  queue: Queue
    coreelements:  queue2: Queue 2
    coreelements:  filesink: File Sink
    coreelements:  tee: Tee pipe fitting
    coreelements:  typefind: TypeFind
    coreelements:  multiqueue: MultiQueue
    coreelements:  valve: Valve element
    staticelements:  bin: Generic bin
    staticelements:  pipeline: Pipeline object
    
    opkg list-installed | grep gst
    gstreamer - 0.10.32-r0.6
    libgstapp-0.10-0 - 0.10.32-r11.1.6
    libgstaudio-0.10-0 - 0.10.32-r11.1.6
    libgstinterfaces-0.10-0 - 0.10.32-r11.1.6
    libgstpbutils-0.10-0 - 0.10.32-r11.1.6
    libgsttag-0.10-0 - 0.10.32-r11.1.6
    libgstvideo-0.10-0 - 0.10.32-r11.1.6
    python-gst - 0.10.17-r0.5
    
Note: tried installing gst-plugins-base. Appears to contain no files.

    opkg install gst-plugins-base
    Installing gst-plugins-base (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-base_0.10.32-r11.1.6_armv5te.ipk.
    Configuring gst-plugins-base.
    [root@rascal14$:~]: opkg files gst-plugins-base
    Package gst-plugins-base (0.10.32-r11.1.6) is installed on root and has the following files:

Gst-plugins-good has two files.

    opkg install gst-plugins-good
    Installing gst-plugins-good (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-good_0.10.28-r11.0.6_armv5te.ipk.
    Configuring gst-plugins-good.
    [root@rascal14$:~]: opkg files gst-plugins-good  
    Package gst-plugins-good (0.10.28-r11.0.6) is installed on root and has the following files:
    /usr/share/gstreamer-0.10/presets/GstIirEqualizer3Bands.prs
    /usr/share/gstreamer-0.10/presets/GstIirEqualizer10Bands.prs


Gst-plugins-bad has no files.

    opkg install gst-plugins-bad
    Installing gst-plugins-bad (0.10.21-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-bad_0.10.21-r11.0.6_armv5te.ipk.
    Configuring gst-plugins-bad.
    [root@rascal14$:~]: opkg files gst-plugins-bad  
    Package gst-plugins-bad (0.10.21-r11.0.6) is installed on root and has the following files:

Gst-plugins-ugly has 1 file.

    opkg install gst-plugins-ugly
    Installing gst-plugins-ugly (0.10.16-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-ugly_0.10.16-r11.1.6_armv5te.ipk.
    Configuring gst-plugins-ugly.
    [root@rascal14$:~]: opkg files gst-plugins-ugly  
    Package gst-plugins-ugly (0.10.16-r11.1.6) is installed on root and has the following files:
    /usr/share/gstreamer-0.10/presets/GstX264Enc.prs
