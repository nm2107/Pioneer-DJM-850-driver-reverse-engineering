# Framerate modelization

This document explains how the different framerates supported by the device
(44100Hz, 48000Hz and 96000Hz) are modelized on audio streams.

*Table of contents :*

- [USB frame capture workflow](#usb-frame-capture-workflow)
- [Variations on transferred data](#variations-on-transferred-data)

## USB frame capture workflow

I configured the mixxx DJ software to use DVS in the Windows VM, and I started
with a 44100Hz framerate. I started the DVS playback and I captured the USB
frames for about 3 seconds. Then, I closed mixxx and repeated these steps for
the 48000Hz and 96000Hz framerates.

This gave me 3 capture files :

- [44100Hz](../left-and-right-audio-channels-representation/captures/usb12_lrinput_lroutput_23.2msbuffer_44100Hz.pcapng)
- [48000Hz](captures/usb12_lrinput_lroutput_23.2msbuffer_48000Hz.pcapng)
- [96000Hz](captures/usb12_lrinput_lroutput_23.2msbuffer_96000Hz.pcapng)

## Variations on transferred data

We can see on the 44100Hz capture file that the USB frames containing the
[audio stream chunks](../timecode-and-music-signals-transfer/README.md)
are having a certain length :

```
URB length [bytes]: 1056
```

Additionally, we know that each audio frame is
[represented on 24bytes](../usb-channels-routing-representation/README.md).

So, to know the number of audio frames transferred in this USB frame, we can
divide the USB frame payload length by the length of a single audio frame :

1056 / 24 = 44

There are 44 audio frames in this transferred USB frame for the 44100Hz audio
framerate.

If we look to the other captures, we have :

| Audio framerate (in Hz) | USB frame payload length (in bytes) | Transferred audio frames in a USB frame |
| ----------------------- | ----------------------------------- | --------------------------------------- |
| 44100                   | 1056                                | 1056 / 24 = 44                          |
| 48000                   | 1152                                | 1152 / 24 = 48                          |
| 96000                   | 2304                                | 2304 / 24 = 96                          |

From this table, we can deduce that the amount of transferred audio frames per
USB frame is related to the audio framerate.

*Note :* For the 44100Hz framerate, there might have some USB frames having
less audio frames in order to transfer the audio for the remaining 100 frames.

We can also deduce that the stream chunk transfer is rate is one chunk per
millisecond (1000Hz).
