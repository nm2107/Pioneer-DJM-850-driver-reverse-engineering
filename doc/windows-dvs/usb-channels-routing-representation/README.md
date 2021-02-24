# USB channels routing representation

*Table of contents :*

- [USB frame capture workflow](#usb-frame-capture-workflow)
- [USB channel routing for timecode signal](#usb-channel-routing-for-timecode-signal)
    - [`USB 1/2` channel routing](#usb-12-channel-routing)
    - [`USB 3/4` channel routing](#usb-34-channel-routing)
    - [`USB 5/6` channel routing](#usb-56-channel-routing)
    - [`USB 7/8` channel routing](#usb-78-channel-routing)
- [USB channel routing for music signal](#usb-channel-routing-for-music-signal)
- [USB channel routing map](#usb-channel-routing-map)
- [Implementation guide](#implementation-guide)
    - [Reading audio frames](#reading-audio-frames)
    - [Writing audio frames](#writing-audio-frames)

## USB frame capture workflow

In order to determine how the USB channels routing was represented in both
timecode and music audio streams, I did the following captures :

- [DVS usage on `USB 1/2` channel only](../left-and-right-audio-channels-representation/captures/usb12_lrinput_lroutput_23.2msbuffer_44100Hz.pcapng)
- [DVS usage on `USB 3/4` channel only](captures/usb34_lrinput_lroutput_23.2msbuffer_44100Hz.pcapng)
- [DVS usage on `USB 5/6` channel only](captures/usb56_lrinput_lroutput_23.2msbuffer_44100Hz.pcapng)
- [DVS usage on `USB 7/8` channel only](captures/usb78_lrinput_lroutput_23.2msbuffer_44100Hz.pcapng)

Then, I just had to compare these captures to see the differences when a
specific USB channel was used or not.

## USB channel routing for timecode signal

### `USB 1/2` channel routing

On the capture file where only `USB 1/2` channel is used, we have the following
data chunk which is sent from the device :

```
USB isodesc 0 [Success] (144 bytes)
    Status: Success (0)
    Offset [bytes]: 0
    Length [bytes]: 144
    ISO Data: 61defbf693150000000000000000000000000000000000002efcfed3f015000000000000…
    Padding: 0x00000000
```

where the complete `ISO Data` value is :

```
61 de fb f6 93 15 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  2e fc fe d3 f0 15 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
25 1f 02 33 dd 15 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  03 37 05 76 57 15 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
6e 34 08 ce 62 14 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  09 07 0b da 04 13 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
```

We can see a six bytes block, followed by eighteen empty bytes until we
encounter another six bytes block, and so on ...

The six bytes block contains the [left and right audio channels data](left-and-right-audio-channels-representation.md)
transmitted on this `USB 1/2` channel.

### `USB 3/4` channel routing

When only the `USB 3/4` channel is used, we have the following data chunk :

```
00 00 00 00 00 00 a2 08  f1 85 1c 10 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 5d 7c
f3 8d 10 12 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 6b 30  f6 57 a6 13 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 00 00 4c 17
f9 a4 d5 14 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 00 00 3d 22  fc 99 99 15 00 00 00 00
00 00 00 00 00 00 00 00
```

We can see that this time, the first six bytes are empty, but the next six
bytes are containing data. Then, eighteen more empty bytes before encountering
an other six bytes block of data.

### `USB 5/6` channel routing

When only the `USB 5/6` channel is used, we have the following data chunk :

```
00 00 00 00 00 00 00 00  00 00 00 00 81 d1 06 f5
e7 14 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 18 bb 09 a3  ba 13 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 2c 71 0c e3
26 12 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 f3 e7 0e ec  34 10 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 0b 11 11 9b
ef 0d 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 00 00 d3 e0 12 8d  60 0b 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 00 00 cd 4f 14 76
96 08 00 00 00 00 00 00
```

This time, the first twelve bytes are empty, then we have a six bytes block of
data. Then, eighteen more empty bytes before encountering an other six bytes
block of data.

### `USB 7/8` channel routing

When only the `USB 7/8` channel is used, we have the following data chunk :

```
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 fa 92 fc 81 8a 15  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 7b ad ff b8 ce 15
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 3d ca 02 87 a1 15  00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00  00 00 24 d8 05 67 03 15
00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
00 00 6b c7 08 ef f8 13
```

We start with eighteen empty bytes, and then we have a six bytes block of data.
Then, eighteen more empty bytes before encountering an other six bytes block of
data.

## USB channel routing for music signal

The same model is used to represent the USB channel routing on the music signal.

## USB channel routing map

From what we saw, we can say that each USB channel are requiring six bytes to
transfer audio data ([three bytes for the left audio channel, and three bytes
for the right audio channel](left-and-right-audio-channels-representation.md)).

Additionnaly, we saw that for each USB channel, these six bytes were not
written at the same position in the data chunk. We can so affirm that the USB
channel routing is determined from the bytes position.

We have four USB channels, each of them requires six bytes : 

- the first six bytes : used for `USB 1/2` channel
- the second six bytes : used for `USB 3/4` channel
- the third six bytes : used for `USB 5/6` channel
- the fourth six bytes : used for `USB 7/8` channel

4 * 6bytes = 24bytes = one audio frame containing audio data for the four USB
channels.

Each audio frame is represented on 24bytes. The transferred data chunk is a
concatenation of audio frames.

Moreover, the data chunk length is a multiple of 24 : 

```
Length [bytes]: 120
```

120 / 24 = 5 . In this chunk, there are 5 audio frames.

We can say that the frame format is `s24_le` (a.k.a. `S24_3LE` for ALSA), as
we're transmitting numbers representing a polarized wave, hence they should be
signed (`s`) ; the number length is on 24bits (`24`) ; and the first transmitted
byte represents the smallest value of the number (little endian, `le`).

## Implementation guide

### Reading audio frames

In order to read the audio frame from a data chunk, we should read it by blocks
of 24 bytes.

Then, in this 24 bytes block, there are 4 blocks of 6 bytes, one for each USB
channel, respectively.

In this 6 bytes block, the first 3 bytes are for the left audio channel, and the
last 3 bytes are for the right audio channel.

### Writing audio frames

This is the same principle than for reading the frames, but we're writing
instead of reading.
