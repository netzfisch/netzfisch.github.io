---
layout:    post
category:  linux
tags:      android tablet flash cyanogenmod
title:     Samsung refuses to update - so switch to CyanogenMod
---
In general I like the Samsung device, in this case a older **Galaxy Tab 2 10.1 GSM (p5100)** is affected, which just refuses to work with the **Google+** app! And **Samsung refuses to fix** this, which is really annoying. So I had to switch to a custom binary like [CyanogenMod][1], that I used already for years on other devices.

To flash that device, first download the appropiate **ingredients**:

* the [CynogenMod Image][2] and [Google_Apps][3] directly to the **SD-Card** of your tablet and
* the custom [recovery][4] to your **Linux-Box**

Than follow these steps:

1. Power off the tablet.
2. Connect the USB adapter to the computer but not to the tablet.
3. Boot the Tablet into **download mode** by holding **Power + Volume-Up**.
4. Then, insert the USB-cable into the device,

and **flash** the recovery!

    $ sudo heimdall flash --RECOVERY recovery.img --no-reboot

A blue transfer bar will appear on the tablet confirming the recovery being transferred.

Unplug the USB cable and manually reboot the tablet into **ClockworkMod Recovery Mode** by holding **Power + Volume-Down**. Once the device boots into the ClockworkMod Recovery, use the physical power / volume buttons to

1. **Backup** your the current installation,
2. to **install** the custom image,
3. as well as the **Google_Apps** from your SD-Card.

**Afterwards** - still in the recovery mode - select **wipe data/factory reset** and clear the **cache/dalivk-cache**

And that's it, you are done - best done one hour before holiday starts.

**More Infos:** In the [CynogenMod Install Guide][5] for the Samsung GalaxyTab 10.2 you find detailed help and troubleshooting.

[1]: http://www.cyanogenmod.org/
[2]: http://download.cyanogenmod.org/?device=p5100
[3]: http://wiki.cyanogenmod.org/w/Google_Apps
[4]: http://download2.clockworkmod.com/recoveries/recovery-clockwork-6.0.2.3-p5100.img
[5]: http://wiki.cyanogenmod.org/w/Install_CM_for_p5100
