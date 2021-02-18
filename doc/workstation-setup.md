# Workstation setup

The setup of your workstation for this reverse engineering is heavily inspired
by the great article of Mario Kicherer describing
[how to reverse engineer a USB soundcard driver for GNU/Linux](https://kicherer.org/joomla/index.php/de/blog/38-reverse-engineering-a-usb-sound-card-with-midi-interface-for-linux).

The RE process is done on a GNU/Linux host, and by using a Windows VM.
The DJM-850 is plugged into the host, and the USB traffic is transfered to the
VM.

*Table of contents :*

- [Host configuration](#host-configuration)
    - [VirtualBox](#virtualbox)
    - [Wireshark](#wireshark)
- [VM configuration](#vm-configuration)
    - [VM settings](#vm-settings)
    - [Windows packages](#windows-packages)
- [Capture USB packets](#capture-usb-packets)

## Host configuration

Your host machine should be a recent GNU/Linux distro, with a recent kernel, and
a USB 2.0 (or above) port that would only be used to plug the DJM-850 into.

### VirtualBox

Install `VirtualBox` using your package manager.

Then, add your user to the `vboxusers` group :

```bash
$ sudo usermod -a -G vboxusers <your_username>
```

and log out then log back in to consider this change.

Finally, install the `VirtualBox Extension Pack` from the
[VirtualBox website](https://www.virtualbox.org/wiki/Downloads) in order to be
able to share the USB 2.0 (or above) port of the host to the VM (only USB 1.1 is
supported without the extension pack).

### Wireshark

Install `wireshark` using your package manager.

Then, add your user to the `wireshark` group :

```bash
$ sudo usermod -a -G wireshark <your_username>
```

and log out then log back in to consider this change.

Finally, enable the `usbmon` kernel module in order to be able to capture the
USB traffic :

```bash
$ sudo modprobe usbmon
```

## VM configuration

### VM settings

Create a new VM and install Windows 10 64bits. You can grab an official ISO
[here](https://www.microsoft.com/en-us/software-download/windows10ISO). During
the installation, enter your serial if you have one, or click `Remind me later`
to enter it once you'll get one.

In VirtualBox, for this VM settings, make sure that the `Enable USB Controller`
box is ticked on the `USB` tab, and select the `USB 2.0 Controller` (or above)
in order to share your USB 2.0 port (or above) with the VM.

### Windows packages

Once the VM is installed and booted, you'll have to install the official
DJM-850 driver and setting utility in the VM.
Grab it from the [Pioneer website](https://www.pioneerdj.com/en-us/support/software/djm-850/)
(`DJM-850 driver for Windows`).

Also, install a DJ software with DVS support into the VM
(e.g. [Mixxx](https://mixxx.org/)) and configure it as you'd normaly do when
mixing on this supported platform.

## Capture USB packets

Once the host and the VM are configured, plug the DJM-850 into the host and
boot the VM.

Once the VM is up and running, share the DJM-850 device from the host to the VM
by checking the `Devices -> USB -> Pioneer DJM-850` checkbox of the VM menu.

The DJM-850 Setting Utility should now open on the VM as it should have
recognised the device.

On the host, you can start wireshark and capture packets from the `usbmonX`
interface, where `X` is the USB interface number where the device is plugged
into. You may have many `usbmon` interfaces depending on the amount of
USB ports you have on your machine, so take a look to the activity graph next
to the interface name to figure out which one to use.

Once wireshark has started the capture, you can make some changes on the device
(such as changing channel switches position), change the `Mixer output` settings
of the setting utility, or start DVS playback with your DJ software.
Then, analyse what you have captured.

It is better to only make a single change at once during captures to ease its
identification. Also, try to make short captures as the device and the computer
are talking to each other a lot.

To have help about how to analyse the captures, please read the remaining of
the documentation in this project.
