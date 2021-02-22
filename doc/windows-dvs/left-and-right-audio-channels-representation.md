# Left and right audio channels representation

*Table of contents :*

- [USB frame capture workflow](#usb-frame-capture-workflow)
- [Left and right audio channels on music signal](#left-and-right-audio-channels-on-music-signal)
    - [Left and right audio channels](#left-and-right-audio-channels)
    - [Left audio channel](#left-audio-channel)
    - [Right audio channel](#right-audio-channel)
- [Left and right audio channels on timecode signal](#left-and-right-audio-channels-on-timecode-signal)
- [Left and right audio channels map](#left-and-right-audio-channels-map)

## USB frame capture workflow

In order to determine how the left and right audio channels were represented in
both timecode and music streams, I did the following captures during DVS
playback :

*normal timecode and music signals usage :*

- [left and right RCA cables plugged into the mixer input, playing a stereo file in mixxx](captures/usb12_lrinput_lroutput_256samples_44100Hz.pcapng)

*timecode signal variations :*

- [left RCA cable only plugged into the mixer input, playing a stereo file in mixxx](captures/usb12_linput_lroutput_256samples_44100Hz.pcapng)
- [right RCA cable only plugged into the mixer input, playing a stereo file in mixxx](captures/usb12_rinput_lroutput_256samples_44100Hz.pcapng)
- [no RCA cables plugged into the mixer input, playing a stereo file in mixxx](captures/usb12_noinput_lroutput_256samples_44100Hz.pcapng)

*music signal variations :*

- [left and right RCA cables plugged into the mixer input, playing an audio file with muted right channel](captures/usb12_lrinput_loutput_256samples_44100Hz.pcapng)
- [left and right RCA cables plugged into the mixer input, playing an audio file with muted left channel](captures/usb12_lrinput_routput_256samples_44100Hz.pcapng)

Then, I just had to compare these captures to see the differences when an audio
channel was used or not.

All these captures were done on the `USB 1/2` channel.

## Left and right audio channels on music signal

### Left and right audio channels

On the capture file when both left and right audio channels are present for the
music signal, we have the following data chunk which is sent :

```
USB isodesc 0 [Cross-device link (-EXDEV)] (144 bytes)
    Status: Cross-device link (-EXDEV) (-18)
    Offset [bytes]: 0
    Length [bytes]: 144
    ISO Data: 4eaffed97aff00000000000000000000000000000000000016ddfe5ddeff000000000000…
    Padding: 0x00000000
```

The complete `ISO Data` of this chunk is the following :

```
4e af fe d9 7a ff 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  16 dd fe 5d de ff 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
23 bc fe 00 38 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  51 9a fe 35 4a 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
6c ce fe f7 88 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  01 1f ff 97 3c 01 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
```

We can see that the first six bytes of the data are filled
(`4e af fe d9 7a ff`), and then we have eighteen empty bytes, until we
encounter another block of six filled bytes (`16 dd fe 5d de ff`),
and so on ...

### Left audio channel

If we look at the capture file where only the left audio channel is present for
the music signal, we have the following :

```
6e 7c fc 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  07 5a fb 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
97 ca ff 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  cd fe 03 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
07 73 02 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
```

We can see that this time, we have only the first three bytes that are filled
(`6e 7c fc`), the next three bytes are silent (`00 00 00`).
Then, we have eighteen empty bytes, and we encounter again a six bytes block
with only the three first bytes filled, and so on ...

### Right audio channel

For the capture file where only the right audio channel is present for the music
signal, we have the following :

```
00 00 00 85 ac ff 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 6b e9 ff 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 e3 de fe 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 26 15 fe 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 08 7c fe 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 98 e6 fe 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
```

This time, the first three bytes are empty (`00 00 00`), then we have data on
the next three bytes (`85 ac ff`). Next, eighteen empty bytes until we encounter
a new six bytes block with the first three bytes being emtpy, and so on ...

## Left and right audio channels on timecode signal

The same representation is used for the timecode signal, however it is less
visible in the captures as there is still some residual electric signal detected
from the Analog/Digital converter, even when the RCA cables are not plugged :

```
f9 ff ff f9 ff ff 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  f4 ff ff e5 ff ff 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
f4 ff ff 09 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  05 00 00 ff ff ff 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
f1 ff ff eb ff ff 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
```

We can see that the values are close to `ff ff ff` or `00 00 00` when the RCA
cables aren't plugged. These values vary a bit due to recording conditions
(mixer electronic components health, moisture, residual electricity, etc...),
which are tricking the A/D converter.

## Left and right audio channels map

From what we observed, we can say that the left and right audio channels are
represented as following :

```
left audio channel
    ^
    |
--------
4e af fe d9 7a ff
         --------
             |
             v
        right audio channel
```

The stereo audio data is encoded on six bytes where the first three are for the
left channel, and the last three are for the right channel.

From the [USB device specifications](../usb-device-specifications.md), we saw
that the USB soundcard was a 24bits soundcard, so this is why the audio channels
are encoded on three bytes : 3bytes = 3 * 8bits = 24bits.
