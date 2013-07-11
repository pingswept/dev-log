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

Ah, the key appears to be to install the gst-plugins-XXX-meta package.

    opkg install gst-plugins-base-meta
    Installing gst-plugins-base-meta (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-base-meta_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugins-base-apps (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-base-apps_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstnetbuffer-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstnetbuffer-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstsdp-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstsdp-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstrtsp-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstrtsp-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstrtp-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstrtp-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstfft-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstfft-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstcdda-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstcdda-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing libgstriff-0.10-0 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgstriff-0.10-0_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-pango (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-pango_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-cdparanoia (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-cdparanoia_0.10.32-r11.1.6_armv5te.ipk.
    Installing libcdparanoia (10.2+svnr17714-r1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libcdparanoia_10.2+svnr17714-r1.6_armv5te.ipk.
    Installing gst-plugin-decodebin (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-decodebin_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-alsa (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-alsa_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-audiorate (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-audiorate_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-gio (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-gio_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-audioresample (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-audioresample_0.10.32-r11.1.6_armv5te.ipk.
    Installing liborc-test-0.4-0 (0.4.11-r0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/liborc-test-0.4-0_0.4.11-r0.6_armv5te.ipk.
    Upgrading liborc-0.4-0 on root from 0.4.10-r0.6 to 0.4.11-r0.6...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/liborc-0.4-0_0.4.11-r0.6_armv5te.ipk.
    Removing obsolete file /usr/lib/liborc-0.4.so.0.10.0.
    Installing gst-plugin-audiotestsrc (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-audiotestsrc_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-vorbis (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-vorbis_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-ffmpegcolorspace (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-ffmpegcolorspace_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-encodebin (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-encodebin_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-adder (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-adder_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-videorate (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-videorate_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-audioconvert (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-audioconvert_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-video4linux (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-video4linux_0.10.32-r11.1.6_armv5te.ipk.
    Installing libsm6 (1:1.2.0-r9.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libsm6_1.2.0-r9.0.6_armv5te.ipk.
    Installing libice6 (1:1.0.7-r9.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libice6_1.0.7-r9.0.6_armv5te.ipk.
    Installing libxv1 (1.0.6-r9.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libxv1_1.0.6-r9.0.6_armv5te.ipk.
    Installing libgudev-1.0-0 (151-r25.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libgudev-1.0-0_151-r25.6_armv5te.ipk.
    Installing gst-plugin-ogg (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-ogg_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-playbin (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-playbin_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-app (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-app_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-ximagesink (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-ximagesink_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-theora (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-theora_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-ivorbisdec (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-ivorbisdec_0.10.32-r11.1.6_armv5te.ipk.
    Installing libvorbisidec1 (20041119-r2.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libvorbisidec1_20041119-r2.6_armv5te.ipk.
    Installing gst-plugin-gdp (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-gdp_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-volume (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-volume_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-xvimagesink (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-xvimagesink_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-subparse (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-subparse_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-typefindfunctions (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-typefindfunctions_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-decodebin2 (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-decodebin2_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-videoscale (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-videoscale_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-videotestsrc (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-videotestsrc_0.10.32-r11.1.6_armv5te.ipk.
    Installing gst-plugin-libvisual (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-libvisual_0.10.32-r11.1.6_armv5te.ipk.
    Installing libvisual-0.4-0 (0.4.0-r1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libvisual-0.4-0_0.4.0-r1.6_armv5te.ipk.
    Installing gst-plugin-tcp (0.10.32-r11.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-tcp_0.10.32-r11.1.6_armv5te.ipk.
    Configuring liborc-0.4-0.
    Configuring gst-plugin-videoscale.
    Configuring libgstfft-0.10-0.
    Configuring gst-plugin-gdp.
    Configuring gst-plugin-app.
    Configuring libgstrtp-0.10-0.
    Configuring libice6.
    Configuring liborc-test-0.4-0.
    Configuring gst-plugins-base-apps.
    Configuring libxv1.
    Configuring libgstcdda-0.10-0.
    Configuring gst-plugin-playbin.
    Configuring gst-plugin-gio.
    Configuring libcdparanoia.
    Configuring gst-plugin-cdparanoia.
    Configuring libgstsdp-0.10-0.
    Configuring gst-plugin-adder.
    Configuring libvorbisidec1.
    Configuring libvisual-0.4-0.
    Configuring gst-plugin-libvisual.
    Configuring gst-plugin-audiotestsrc.
    Configuring libgstrtsp-0.10-0.
    Configuring gst-plugin-audioconvert.
    Configuring gst-plugin-theora.
    Configuring gst-plugin-decodebin2.
    Configuring libsm6.
    Configuring libgstnetbuffer-0.10-0.
    Configuring gst-plugin-volume.
    Configuring libgudev-1.0-0.
    Configuring gst-plugin-video4linux.
    Configuring gst-plugin-audiorate.
    Configuring gst-plugin-videorate.
    Configuring gst-plugin-subparse.
    Configuring gst-plugin-pango.
    Configuring libgstriff-0.10-0.
    Configuring gst-plugin-ogg.
    Configuring gst-plugin-decodebin.
    Configuring gst-plugin-alsa.
    Configuring gst-plugin-audioresample.
    Configuring gst-plugin-vorbis.
    Configuring gst-plugin-ffmpegcolorspace.
    Configuring gst-plugin-encodebin.
    Configuring gst-plugin-ximagesink.
    Configuring gst-plugin-ivorbisdec.
    Configuring gst-plugin-xvimagesink.
    Configuring gst-plugin-typefindfunctions.
    Configuring gst-plugin-videotestsrc.
    Configuring gst-plugin-tcp.
    Configuring gst-plugins-base-meta.

Problem installing gst-plugins-good-meta due to gconf-dbus wanting to install an existing conf file.

    opkg install gst-plugins-good-meta
    Installing gst-plugins-good-meta (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-good-meta_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugins-good-apps (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugins-good-apps_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-rtp (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-rtp_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-rtpmanager (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-rtpmanager_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-id3demux (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-id3demux_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-multipart (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-multipart_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-efence (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-efence_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-interleave (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-interleave_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-multifile (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-multifile_0.10.28-r11.0.6_armv5te.ipk.
    Installing gst-plugin-souphttpsrc (0.10.28-r11.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/gstreamer/gst-plugin-souphttpsrc_0.10.28-r11.0.6_armv5te.ipk.
    Installing libsoup-gnome-2.4-1 (2.32.2-r0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libsoup-gnome-2.4-1_2.32.2-r0.6_armv5te.ipk.
    Installing libsoup-2.4-1 (2.32.2-r0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libsoup-2.4-1_2.32.2-r0.6_armv5te.ipk.
    Installing gconf (2.28.0-r2.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/gconf_2.28.0-r2.6_armv5te.ipk.
    Installing dbus-x11 (1.2.24-r20.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/dbus-x11_1.2.24-r20.1.6_armv5te.ipk.
    Installing orbit2 (2.14.17-r0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/orbit2_2.14.17-r0.6_armv5te.ipk.
    Installing libidl-2-0 (0.8.13-r0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libidl-2-0_0.8.13-r0.6_armv5te.ipk.
    Installing libdbus-glib-1-2 (0.86-r2.1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libdbus-glib-1-2_0.86-r2.1.6_armv5te.ipk.
    Installing policykit (0.96-r3.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/policykit_0.96-r3.6_armv5te.ipk.
    Installing eggdbus (0.6-r0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/eggdbus_0.6-r0.6_armv5te.ipk.
    Installing libproxy (0.2.3-r1.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libproxy_0.2.3-r1.6_armv5te.ipk.
    Installing libxmu6 (1:1.1.0-r9.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libxmu6_1.1.0-r9.0.6_armv5te.ipk.
    Installing libxt6 (1:1.0.9-r9.0.6) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libxt6_1.0.9-r9.0.6_armv5te.ipk.
    Installing gconf-dbus (2.16.0+svnr641-r0.5) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/gconf-dbus_2.16.0+svnr641-r0.5_armv5te.ipk.
    Configuring dbus-x11.
    Configuring libidl-2-0.
    Configuring orbit2.
    Configuring libdbus-glib-1-2.
    Configuring eggdbus.
    Configuring policykit.
    Reloading system message bus config: done.
    Configuring gconf.
    Configuring libxt6.
    Configuring gst-plugin-multifile.
    Configuring gst-plugin-interleave.
    Configuring gst-plugin-rtp.
    Configuring libxmu6.
    Configuring libproxy.
    Configuring gst-plugin-efence.
    Configuring gst-plugin-id3demux.
    Configuring libsoup-2.4-1.
    Configuring gst-plugins-good-apps.
    Configuring libsoup-gnome-2.4-1.
    Configuring gst-plugin-souphttpsrc.
    Configuring gst-plugin-multipart.
    Configuring gst-plugin-rtpmanager.
    Collected errors:
     * check_data_file_clashes: Package gconf-dbus wants to install file /etc/gconf/2/path
        But that file is already provided by package  * gconf
     * check_data_file_clashes: Package gconf-dbus wants to install file /usr/libexec/gconfd-2
    	But that file is already provided by package  * gconf
     * check_data_file_clashes: Package gconf-dbus wants to install file /usr/lib/libgconf-2.so.4
    	But that file is already provided by package  * gconf
     * check_data_file_clashes: Package gconf-dbus wants to install file /usr/bin/gconf-merge-tree
    	But that file is already provided by package  * gconf
     * check_data_file_clashes: Package gconf-dbus wants to install file /usr/bin/gconftool-2
    	But that file is already provided by package  * gconf
     * opkg_install_cmd: Cannot install package gst-plugins-good-meta.

But mp3 decoding still doesn't seem to work right.

    gst-launch filesrc location=calm.mp3 ! decodebin ! audioconvert ! alsasink
    (gst-plugin-scanner:2417): GStreamer-WARNING **: Failed to load plugin '/usr/lib/gstreamer-0.10/libgstsouphttpsrc.so': libgnome-keyring.so.0: cannot open shared object file: No such file or directory
    Setting pipeline to PAUSED ...
    Pipeline is PREROLLING ...
    ERROR: from element /GstPipeline:pipeline0/GstFileSrc:filesrc0: Internal data flow error.
    Additional debug info:
    gstbasesrc.c(2574): gst_base_src_loop (): /GstPipeline:pipeline0/GstFileSrc:filesrc0:
    streaming task paused, reason not-linked (-1)
    ERROR: pipeline doesn't want to preroll.
    Setting pipeline to NULL ...
    Freeing pipeline ...
