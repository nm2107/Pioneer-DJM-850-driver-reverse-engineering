# Audio buffer usage

*Table of contents :*

- [USB frame capture workflow](#usb-frame-capture-workflow)

## USB frame capture workflow

I configured the mixxx DJ software to use DVS in the Windows VM, and I started
with a 23.2ms buffer size. I started the DVS playback and I captured the USB
frames for about 3 seconds. Then, I closed mixxx and repeated these steps for
the 5.8ms buffer size.

This gave me 2 capture files :

- [23.2ms buffer size](../common-captures/usb12_lrinput_lroutput_23.2msbuffer_44100Hz.pcapng)
- [5.8ms buffer size](captures/usb12_lrinput_lroutput_5.8msbuffer_44100Hz.pcapng)

However, by inspecting the capture files, I did not find any difference on the
length of the transmitted data, nor on the frequency of the requests.

I suppose that this setting only affects the computer software, not the
transmitted audio streams.
