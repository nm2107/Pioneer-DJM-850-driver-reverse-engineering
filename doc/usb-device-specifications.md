# USB device specifications

*Table of contents :*

- [Vendor and device IDs](#vendor-and-device-ids)
- [Details from the user manual](#details-from-the-user-manual)
- [Complete USB device specification](#complete-usb-device-specification)
- [Interpretation](#interpretation)
    - [First Interface Descriptor](#first-interface-descriptor)
    - [Second Interface Descriptor](#second-interface-descriptor)
    - [Third Interface Descriptor](#third-interface-descriptor)
    - [Fourth Interface Descriptor](#fourth-interface-descriptor)

## Vendor and device IDs

- Vendor ID : `08e4`
- Product ID : `0163`

## Details from the user manual

The [user manual](https://www.pioneerdj.com/en/support/documents/archive/djm-850/)
specifies the following details :

> 24 bit/96 kHz STEREO 4-IN 4-OUT SOUND CARD
> 
> This unit is equipped with a 24 bit/96 kHz stereo 4-in 4-out compatible
> USB sound card.
> 
> This unit supports ASIO/Core Audio standards, so it can be used not
> only for DJ performances with DJ software but also with a wide variety of
> other software applications, including for software for creating music.
> 
> - Four sets of stereo sound from a single computer can be input to the
> respective channels and mixed.
> - Up to four sets of stereo sound can be output to the computer from
> the respective channels (channels 1 to 4, REC OUT, crossfader sides
> A and B and microphone).
> - The sampling rate can be switched between 96 kHz, 48 kHz and
44.1 kHz.

*EN user manual, p6.*

> Audio Section
> 
> Sampling rate .................. 96 kHz
> 
> MASTER D/A converter ........... 32 bits
> 
> Other A/D and D/A converters ... 24 bits

*EN user manual, p28.*

## Complete USB device specification

```bash
$ sudo lsusb -v -d 08e4:0163

Bus 001 Device 008: ID 08e4:0163 Pioneer Corp. DJM-850
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass            0 
  bDeviceSubClass         0 
  bDeviceProtocol         0 
  bMaxPacketSize0        64
  idVendor           0x08e4 Pioneer Corp.
  idProduct          0x0163 
  bcdDevice           10.00
  iManufacturer           1 PIONEER CORPORATION
  iProduct                2 DJM-850
  iSerial                 0 
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x006c
    bNumInterfaces          3
    bConfigurationValue     1
    iConfiguration          2 DJM-850
    bmAttributes         0xc0
      Self Powered
    MaxPower                0mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           0
      bInterfaceClass       255 Vendor Specific Class
      bInterfaceSubClass      0 
      bInterfaceProtocol      0 
      iInterface              0 
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       1
      bNumEndpoints           2
      bInterfaceClass       255 Vendor Specific Class
      bInterfaceSubClass      0 
      bInterfaceProtocol      0 
      iInterface              0 
      Endpoint Descriptor:
        bLength                 9
        bDescriptorType         5
        bEndpointAddress     0x05  EP 5 OUT
        bmAttributes            5
          Transfer Type            Isochronous
          Synch Type               Asynchronous
          Usage Type               Data
        wMaxPacketSize     0x0400  1x 1024 bytes
        bInterval               1
        bRefresh                0
        bSynchAddress           0
      Endpoint Descriptor:
        bLength                 9
        bDescriptorType         5
        bEndpointAddress     0x86  EP 6 IN
        bmAttributes            5
          Transfer Type            Isochronous
          Synch Type               Asynchronous
          Usage Type               Data
        wMaxPacketSize     0x0400  1x 1024 bytes
        bInterval               1
        bRefresh                0
        bSynchAddress           0
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       0
      bNumEndpoints           0
      bInterfaceClass         1 Audio
      bInterfaceSubClass      1 Control Device
      bInterfaceProtocol      0 
      iInterface              2 DJM-850
      AudioControl Interface Descriptor:
        bLength                 9
        bDescriptorType        36
        bDescriptorSubtype      1 (HEADER)
        bcdADC               1.00
        wTotalLength       0x0009
        bInCollection           1
        baInterfaceNr(0)        2
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        2
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         1 Audio
      bInterfaceSubClass      3 MIDI Streaming
      bInterfaceProtocol      0 
      iInterface              4 DJM-850 USB MIDI
      MIDIStreaming Interface Descriptor:
        bLength                 7
        bDescriptorType        36
        bDescriptorSubtype      1 (HEADER)
        bcdADC               1.00
        wTotalLength       0x0016
      MIDIStreaming Interface Descriptor:
        bLength                 6
        bDescriptorType        36
        bDescriptorSubtype      2 (MIDI_IN_JACK)
        bJackType               2 External
        bJackID                 1
        iJack                   4 DJM-850 USB MIDI
      MIDIStreaming Interface Descriptor:
        bLength                 9
        bDescriptorType        36
        bDescriptorSubtype      3 (MIDI_OUT_JACK)
        bJackType               1 Embedded
        bJackID                 2
        bNrInputPins            1
        baSourceID( 0)          1
        BaSourcePin( 0)         1
        iJack                   4 DJM-850 USB MIDI
      Endpoint Descriptor:
        bLength                 9
        bDescriptorType         5
        bEndpointAddress     0x87  EP 7 IN
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0200  1x 512 bytes
        bInterval               0
        bRefresh                0
        bSynchAddress           0
        MIDIStreaming Endpoint Descriptor:
          bLength                 5
          bDescriptorType        37
          bDescriptorSubtype      1 (GENERAL)
          bNumEmbMIDIJack         1
          baAssocJackID( 0)       2
Device Status:     0x0001
  Self Powered
```

## Interpretation

### First Interface Descriptor

- Interface Number : `0`
- Alternate Setting : `0`
- Interface Class : `Vendor Specific Class`
- Description : used to configure the USB output settings, via endpoint number
`0`. See the [`Mixer output` tab](windows-djm-850-setting-utility/README.md)
documentation for interface usage details.

### Second Interface Descriptor

- Interface Number : `0`
- Alternate Setting : `1`
- Interface Class : `Vendor Specific Class`
- Description : exposes endpoints number `5` and `6` for `ISOCHRONOUS` `URB in`
and `URB out` requests and responses. Used for DVS on the 24bits A/D and D/A
audio converter (ASIO and Core Audio compatible).

### Third Interface Descriptor

- Interface Number : `1`
- Alternate Setting : `0`
- Interface Class : `Audio Control Device`
- Description : may be the interface related to the 32bits A/D and D/A  audio
converter, which can be used for classic audio playback and recording hover USB
(i.e. not for DVS usage).

### Fourth Interface Descriptor

- Interface Number : `2`
- Alternate Setting : `0`
- Interface Class : `Audio MIDI Streaming`
- Description : seems to expose MIDI I/O on endpoint number `7`.
