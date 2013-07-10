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

