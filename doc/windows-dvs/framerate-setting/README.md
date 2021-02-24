# Framerate setting

This document explains how the computer and the device agree on which framerate
to use.

*Table of contents :*

- [USB frame capture workflow](#usb-frame-capture-workflow)
- [Framerate setting map](#framerate-setting-map)
- [Frame details](#frame-details)
    - [Request](#request)
    - [Response](#response)
- [Implementation guide](#implementation-guide)
    - [Crafting request](#crafting-request)
    - [Reading response](#reading-response)

## USB frame capture workflow

Mixxx was configured to use a 44100Hz framerate. I started the capture before
launching mixxx, and stopped it when mixxx was running and the device and the
computer were exchanging audio streams.

Then, I changed the framerate in mixxx, and closed it and reproduced all these
steps in order to capture the framerate agreement for the remaining framerates.

This gave me the following captures :

- [44100Hz framerate setting](captures/mixxx_startup_23.2msbuffer_44100Hz.pcapng)
- [48000Hz framerate setting](captures/mixxx_startup_23.2msbuffer_48000Hz.pcapng)
- [96000Hz framerate setting](captures/mixxx_startup_23.2msbuffer_96000Hz.pcapng)

The captures are starting with some MIDI messages while the
[mixer is idle](../../plugged-in-idle-behavior/README.md).
Then, when mixxx stats, the audio streams for timecode and music signals are
sent.

The information we're looking for is right before the audio streams transmission
start. You can use the following wireshark filter to see it easily :

```
usb.transfer_type == URB_CONTROL && usb.endpoint_address.direction == OUT
```

## Framerate setting map

As the framerate can be set via the DJ software on the computer, it's the
computer who tells the device which framerate to use.

By comparing the `URB_CONTROL out` frames of the different captures, I saw that
there was variations in the `Data Fragment` field of the request :

| Framerate to use | `Data Fragment` value |
| ---------------- | --------------------- |
| 44100Hz          | `44ac00`              |
| 48000Hz          | `80bb00`              |
| 96000Hz          | `007701`              |

The framerate information is stored on three bytes.

Once the framerate is set, the computer can initiate the audio streams.

## Frame details

### Request

Frame sent by the computer to the device to set the framerate to 44100Hz :

```
Frame 271: 67 bytes on wire (536 bits), 67 bytes captured (536 bits) on interface usbmon1, id 0
    Interface id: 0 (usbmon1)
        Interface name: usbmon1
    Encapsulation type: USB packets with Linux header and padding (115)
    Arrival Time: Feb 24, 2021 17:20:49.101078000 CET
    [Time shift for this packet: 0.000000000 seconds]
    Epoch Time: 1614183649.101078000 seconds
    [Time delta from previous captured frame: 0.000397000 seconds]
    [Time delta from previous displayed frame: 0.000397000 seconds]
    [Time since reference or first frame: 2.469670000 seconds]
    Frame Number: 271
    Frame Length: 67 bytes (536 bits)
    Capture Length: 67 bytes (536 bits)
    [Frame is marked: False]
    [Frame is ignored: False]
    [Protocols in frame: usb]
USB URB
    [Source: host]
    [Destination: 1.8.0]
    URB id: 0xffff8b9c1e2ae3c0
    URB type: URB_SUBMIT ('S')
    URB transfer type: URB_CONTROL (0x02)
    Endpoint: 0x00, Direction: OUT
        0... .... = Direction: OUT (0)
        .... 0000 = Endpoint number: 0
    Device: 8
    URB bus id: 1
    Device setup request: relevant (0)
    Data: present (0)
    URB sec: 1614183649
    URB usec: 101078
    URB status: Operation now in progress (-EINPROGRESS) (-115)
    URB length [bytes]: 3
    Data length [bytes]: 3
    [Response in: 272]
    Interval: 0
    Start frame: 0
    Copy of Transfer Flags: 0x00000000
        .... .... .... .... .... .... .... ...0 = Short not OK: False
        .... .... .... .... .... .... .... ..0. = ISO ASAP: False
        .... .... .... .... .... .... .... .0.. = No transfer DMA map: False
        .... .... .... .... .... .... ..0. .... = No FSBR: False
        .... .... .... .... .... .... .0.. .... = Zero Packet: False
        .... .... .... .... .... .... 0... .... = No Interrupt: False
        .... .... .... .... .... ...0 .... .... = Free Buffer: False
        .... .... .... .... .... ..0. .... .... = Dir IN: False
        .... .... .... ...0 .... .... .... .... = DMA Map Single: False
        .... .... .... ..0. .... .... .... .... = DMA Map Page: False
        .... .... .... .0.. .... .... .... .... = DMA Map SG: False
        .... .... .... 0... .... .... .... .... = Map Local: False
        .... .... ...0 .... .... .... .... .... = Setup Map Single: False
        .... .... ..0. .... .... .... .... .... = Setup Map Local: False
        .... .... .0.. .... .... .... .... .... = DMA S-G Combined: False
        .... .... 0... .... .... .... .... .... = Aligned Temp Buffer: False
    Number of ISO descriptors: 0
    [bInterfaceClass: Vendor Specific (0xff)]
Setup Data
    bmRequestType: 0x22
        0... .... = Direction: Host-to-device
        .01. .... = Type: Class (0x1)
        ...0 0010 = Recipient: Endpoint (0x02)
    bRequest: 1
    wValue: 0x0100
    wIndex: 134 (0x0086)
    wLength: 3
    Data Fragment: 44ac00
```

### Response

Frame sent by the device to the computer to aknowledge framerate setting :

```
Frame 272: 64 bytes on wire (512 bits), 64 bytes captured (512 bits) on interface usbmon1, id 0
    Interface id: 0 (usbmon1)
        Interface name: usbmon1
    Encapsulation type: USB packets with Linux header and padding (115)
    Arrival Time: Feb 24, 2021 17:20:49.101307000 CET
    [Time shift for this packet: 0.000000000 seconds]
    Epoch Time: 1614183649.101307000 seconds
    [Time delta from previous captured frame: 0.000229000 seconds]
    [Time delta from previous displayed frame: 0.000229000 seconds]
    [Time since reference or first frame: 2.469899000 seconds]
    Frame Number: 272
    Frame Length: 64 bytes (512 bits)
    Capture Length: 64 bytes (512 bits)
    [Frame is marked: False]
    [Frame is ignored: False]
    [Protocols in frame: usb]
USB URB
    [Source: 1.8.0]
    [Destination: host]
    URB id: 0xffff8b9c1e2ae3c0
    URB type: URB_COMPLETE ('C')
    URB transfer type: URB_CONTROL (0x02)
    Endpoint: 0x00, Direction: OUT
        0... .... = Direction: OUT (0)
        .... 0000 = Endpoint number: 0
    Device: 8
    URB bus id: 1
    Device setup request: not relevant ('-')
    Data: not present ('>')
    URB sec: 1614183649
    URB usec: 101307
    URB status: Success (0)
    URB length [bytes]: 3
    Data length [bytes]: 0
    [Request in: 271]
    [Time from request: 0.000229000 seconds]
    Unused Setup Header
    Interval: 0
    Start frame: 0
    Copy of Transfer Flags: 0x00000000
        .... .... .... .... .... .... .... ...0 = Short not OK: False
        .... .... .... .... .... .... .... ..0. = ISO ASAP: False
        .... .... .... .... .... .... .... .0.. = No transfer DMA map: False
        .... .... .... .... .... .... ..0. .... = No FSBR: False
        .... .... .... .... .... .... .0.. .... = Zero Packet: False
        .... .... .... .... .... .... 0... .... = No Interrupt: False
        .... .... .... .... .... ...0 .... .... = Free Buffer: False
        .... .... .... .... .... ..0. .... .... = Dir IN: False
        .... .... .... ...0 .... .... .... .... = DMA Map Single: False
        .... .... .... ..0. .... .... .... .... = DMA Map Page: False
        .... .... .... .0.. .... .... .... .... = DMA Map SG: False
        .... .... .... 0... .... .... .... .... = Map Local: False
        .... .... ...0 .... .... .... .... .... = Setup Map Single: False
        .... .... ..0. .... .... .... .... .... = Setup Map Local: False
        .... .... .0.. .... .... .... .... .... = DMA S-G Combined: False
        .... .... 0... .... .... .... .... .... = Aligned Temp Buffer: False
    Number of ISO descriptors: 0
    [bInterfaceClass: Vendor Specific (0xff)]
```

## Implementation guide

### Crafting request

In order to set the framerate to use, the computer should send a request to the
device having the following attributes :

```
USB URB
    URB type: URB_SUBMIT ('S')
    URB transfer type: URB_CONTROL (0x02)
    Endpoint: 0x00, Direction: OUT
        0... .... = Direction: OUT (0)
        .... 0000 = Endpoint number: 0
    URB length [bytes]: 3
    Data length [bytes]: 3
Setup Data
    bRequest: 1
    wValue: 0x0100
    wIndex: 134 (0x0086)
    wLength: 3
    Data Fragment: 44ac00
```

Refer to the [framerate setting map](#framerate-setting-map) to know the value
of the `Data Fragment` field.

### Reading response

The device should send a response having the following attributes when the
framerate setting has been aknowledged :

```
USB URB
    URB type: URB_COMPLETE ('C')
    URB transfer type: URB_CONTROL (0x02)
    Endpoint: 0x00, Direction: OUT
        0... .... = Direction: OUT (0)
        .... 0000 = Endpoint number: 0
    URB status: Success (0)
    URB length [bytes]: 3
    Data length [bytes]: 0
```
