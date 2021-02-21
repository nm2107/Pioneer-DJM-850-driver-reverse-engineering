# Plugged in idle behavior

*Table of contents :*

- [USB frame capture workflow](#usb-frame-capture-workflow)
- [Frame details](#frame-details)
    - [Request](#request)
    - [Response](#response)

## USB frame capture workflow

I plugged the device into the computer, and the Setting Utility showed up in the
Windows VM. I closed it and then started to capture USB frames with wireshark
from my GNU/Linux host.

By looking at [the capture](captures/no_setting_utility_nor_dj_software_opened.pcapng),
we can see that the computer and the device are discussing, even if the Setting
Utility is not opened, nor any DJ software.

The discussion is targeting endpoint `7`, which is for MIDI i/o.

## Frame details

The computer is repetitively sending `URB_BULK in` requests to the device, which
responds with `USB-MIDI` packets using `USBAUDIO` protocol.

### Request

```
Frame 100: 64 bytes on wire (512 bits), 64 bytes captured (512 bits) on interface usbmon1, id 0
    Interface id: 0 (usbmon1)
        Interface name: usbmon1
    Encapsulation type: USB packets with Linux header and padding (115)
    Arrival Time: Feb 20, 2021 21:36:05.652516000 CET
    [Time shift for this packet: 0.000000000 seconds]
    Epoch Time: 1613853365.652516000 seconds
    [Time delta from previous captured frame: 0.000420000 seconds]
    [Time delta from previous displayed frame: 0.000420000 seconds]
    [Time since reference or first frame: 0.380811000 seconds]
    Frame Number: 100
    Frame Length: 64 bytes (512 bits)
    Capture Length: 64 bytes (512 bits)
    [Frame is marked: False]
    [Frame is ignored: False]
    [Protocols in frame: usb]
USB URB
    [Source: host]
    [Destination: 1.8.7]
    URB id: 0xffff95309c66f900
    URB type: URB_SUBMIT ('S')
    URB transfer type: URB_BULK (0x03)
    Endpoint: 0x87, Direction: IN
        1... .... = Direction: IN (1)
        .... 0111 = Endpoint number: 7
    Device: 8
    URB bus id: 1
    Device setup request: not relevant ('-')
    Data: not present ('<')
    URB sec: 1613853365
    URB usec: 652516
    URB status: Operation now in progress (-EINPROGRESS) (-115)
    URB length [bytes]: 512
    Data length [bytes]: 0
    [Response in: 109]
    [bInterfaceClass: Audio (0x01)]
    Unused Setup Header
    Interval: 0
    Start frame: 0
    Copy of Transfer Flags: 0x00000200, Dir IN
        .... .... .... .... .... .... .... ...0 = Short not OK: False
        .... .... .... .... .... .... .... ..0. = ISO ASAP: False
        .... .... .... .... .... .... .... .0.. = No transfer DMA map: False
        .... .... .... .... .... .... ..0. .... = No FSBR: False
        .... .... .... .... .... .... .0.. .... = Zero Packet: False
        .... .... .... .... .... .... 0... .... = No Interrupt: False
        .... .... .... .... .... ...0 .... .... = Free Buffer: False
        .... .... .... .... .... ..1. .... .... = Dir IN: True
        .... .... .... ...0 .... .... .... .... = DMA Map Single: False
        .... .... .... ..0. .... .... .... .... = DMA Map Page: False
        .... .... .... .0.. .... .... .... .... = DMA Map SG: False
        .... .... .... 0... .... .... .... .... = Map Local: False
        .... .... ...0 .... .... .... .... .... = Setup Map Single: False
        .... .... ..0. .... .... .... .... .... = Setup Map Local: False
        .... .... .0.. .... .... .... .... .... = DMA S-G Combined: False
        .... .... 0... .... .... .... .... .... = Aligned Temp Buffer: False
    Number of ISO descriptors: 0
```

### Response

```
Frame 109: 68 bytes on wire (544 bits), 68 bytes captured (544 bits) on interface usbmon1, id 0
    Interface id: 0 (usbmon1)
        Interface name: usbmon1
    Encapsulation type: USB packets with Linux header and padding (115)
    Arrival Time: Feb 20, 2021 21:36:05.756121000 CET
    [Time shift for this packet: 0.000000000 seconds]
    Epoch Time: 1613853365.756121000 seconds
    [Time delta from previous captured frame: 0.020648000 seconds]
    [Time delta from previous displayed frame: 0.020648000 seconds]
    [Time since reference or first frame: 0.484416000 seconds]
    Frame Number: 109
    Frame Length: 68 bytes (544 bits)
    Capture Length: 68 bytes (544 bits)
    [Frame is marked: False]
    [Frame is ignored: False]
    [Protocols in frame: usb:usbaudio]
USB URB
    [Source: 1.8.7]
    [Destination: host]
    URB id: 0xffff95309c66f900
    URB type: URB_COMPLETE ('C')
    URB transfer type: URB_BULK (0x03)
    Endpoint: 0x87, Direction: IN
        1... .... = Direction: IN (1)
        .... 0111 = Endpoint number: 7
    Device: 8
    URB bus id: 1
    Device setup request: not relevant ('-')
    Data: present (0)
    URB sec: 1613853365
    URB usec: 756121
    URB status: Success (0)
    URB length [bytes]: 4
    Data length [bytes]: 4
    [Request in: 100]
    [Time from request: 0.103605000 seconds]
    [bInterfaceClass: Audio (0x01)]
    Unused Setup Header
    Interval: 0
    Start frame: 0
    Copy of Transfer Flags: 0x00000200, Dir IN
        .... .... .... .... .... .... .... ...0 = Short not OK: False
        .... .... .... .... .... .... .... ..0. = ISO ASAP: False
        .... .... .... .... .... .... .... .0.. = No transfer DMA map: False
        .... .... .... .... .... .... ..0. .... = No FSBR: False
        .... .... .... .... .... .... .0.. .... = Zero Packet: False
        .... .... .... .... .... .... 0... .... = No Interrupt: False
        .... .... .... .... .... ...0 .... .... = Free Buffer: False
        .... .... .... .... .... ..1. .... .... = Dir IN: True
        .... .... .... ...0 .... .... .... .... = DMA Map Single: False
        .... .... .... ..0. .... .... .... .... = DMA Map Page: False
        .... .... .... .0.. .... .... .... .... = DMA Map SG: False
        .... .... .... 0... .... .... .... .... = Map Local: False
        .... .... ...0 .... .... .... .... .... = Setup Map Single: False
        .... .... ..0. .... .... .... .... .... = Setup Map Local: False
        .... .... .0.. .... .... .... .... .... = DMA S-G Combined: False
        .... .... 0... .... .... .... .... .... = Aligned Temp Buffer: False
    Number of ISO descriptors: 0
USB Midi Event Packet: Single Byte
    0000 .... = Cable Number: 0x0
    .... 1111 = Code Index: Single Byte (0xf)
    MIDI Event: f8
    Padding: 0000
```
