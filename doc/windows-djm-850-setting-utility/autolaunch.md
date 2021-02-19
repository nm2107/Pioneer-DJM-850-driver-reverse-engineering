# Setting Utility autolaunch

*Table of contents :*

- [When does the Setting Utility autolaunch ?](#when-does-the-setting-utility-autolaunch-)
- [How could the Setting Utility autolaunch ?](#how-could-the-setting-utility-autolaunch-)
- [Why does the Setting Utility autolaunch ?](#why-does-the-setting-utility-autolaunch-)

## When does the Setting Utility autolaunch ?

The Setting Utility autolaunches in the following cases :

- When plugging in the device into the computer, and the computer is already
started (i.e. Windows session opened).
- When the device is already plugged into the computer, and the computer starts
(i.e. booting the computer and opening a Windows session).

## How could the Setting Utility autolaunch ?

We can suppose that there is some kind of daemon or a kernel handler which is
aware of the plugged in USB devices on the computer (or gets notified when a
device gets plugged in), and which then launches the Setting Utility.

Note that when the Setting Utility is already launched but the device is not
plugged in yet, there is no additional launches of the Setting Utility when the
device gets plugged in (i.e. there is only one instance of the Setting Utility
running).

Also, when running, the Setting Utility is able to tell whether the DJM-850
device is plugged into the computer or not.

## Why does the Setting Utility autolaunch ?

By looking at what happens when
[plugging in the device into the computer](plugging-in-device/README.md),
we can see that the Setting Utility sends the latest configured options for the
USB output settings to the device.

From that, we can suppose that these options are never stored in the device.
They do are stored in the computer, and this initialization process may signify
that the device do not have persistent storage.

Also by looking at the polling behavior of the Setting Utility in order to get
the channel input switches position for the
[`Mixer input` tab](mixer-input-tab/README.md), we can see that the device
never sends info to the computer without being requested to (i.e. the device
does not send any info about an input switch position change).

We can describe it as a passive behavior from the device. The computer is
responsible of gathering informations about channel input switches position, and
is also responsible of initializing the device for USB output options
configuration.

This passive behavior could be explained as the device can also be used as a
standalone DJ mixer, so it does not have to inform any computer about any
changes, unless requested.
