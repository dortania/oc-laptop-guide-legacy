# A Guide to Build Guides

Welcome to the Internet! Here we have guides of all shapes and sizes, simple guides, complex guides, and even word salads tagged "guide" such as the one you're reading right now! Using guides to configure your system after installing can be OK but here is some general guidance to follow before tripping and falling down that rabbit hole.

## Is the Guide Complete?

In my opinion a complete guide contains the following information:

### Hardware details

The more information the better, there are so many variations of laptops out there that there are sub model identifiers within models. A good guide will identify things like:

* **Model** - The full model of the device including the variation designator.
* **CPU** - The precise model.
* **RAM** - Better if it includes the slot configuration!
* **GPU** - Is that guide for the 7th gen HP Omen laptop with the RX 580 or the GTX 1060?
* **DISKS** - Size, and configuration \(m.2 or SATA, NVMe or no?\)
* **AUDIO CODEC** - ALC/Conexant?  Best if it includes the layout ID.
* **WIFI/BLUETOOTH** - What did it have, and what was it replaced with?
* **NETWORK -** Does it have a built in ethernet device, does it match yours?
* **INPUT DEVICES -** Is the keyboard PS/2, is it backlit?  Is the touchpad I2C?  Is there a touch display?

Other helpful information to note are ancillary devices like the webcam, and devices that don't work.

### Does it Include Firmware Configuration?

We covered the basic BIOS configuration changes, but many devices have additional changes to be made. A good guide will document these settings.

### Are there Installation Steps?

This is only really important if there is some deviation from the norm. It's a nice to have though.

### Does the guide require you to disable SIP and/or Gatekeeper?!

A lot of guides out there sacrifice security for convenience, and they often neglect to inform the users of these guides why they're doing it. Whether it's intentional or not is unimportant, but you need to look for a few red flags, and if you find them you should stop using that guide. The table below describes what to look for, and why it's important to leave enabled.

| Property / Command | Parameter | Function |
| :--- | :--- | :--- |


| CsrActiveConfig \(config.plist\) | Any value other than 0x0 | This parameter enables System Integrity Protection. If it's set to any other value SIP is partially or fully disabled. SIP prevents unsigned packages from being injected into the kernel, and prevents applications from overwriting nvram values among other things. When injecting hack kexts via C/k/O it is not necessary to disable. |
| :--- | :--- | :--- |


<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>spctl</p>
        <p>(Terminal)</p>
      </th>
      <th style="text-align:left">--master-disable</th>
      <th style="text-align:left">This command disables Gatekeeper and allows any binary downloaded from
        the internet to be executed without being audited. Instead simply control-click
        or right-click and open your binary package leaving Gatekeeper enabled.</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>|  |  |  |
| :--- | :--- | :--- |


You'll come across this often in guides, there are a few things to ask yourself before aborting.

* Does the configuration include the user's DSDT and serial numbers?  This is an indicator that the writer either didn't take the time to clean up before publishing, or they didn't know enough about their Hackintosh to know they should.  Abort.
* Does the configuration include everything you need for a working hackintosh?  If it does, ask yourself when was the last time it was updated?  If there's a large gap between the publish date and the current date, abort.  If it's maintained, it might be OK.  Check the sources before continuing.
* Does the configuration include only the deviations from standard?  Maybe it has a clean config.plist, a usb map, and a few SSDTs or even a kext for something that's hard to find.  In this case, it's probably OK.  Still want to check that source though!

### Does the guide link to sources?

If not, does it at least provide enough information to help you find them yourself? If not, it's probably best to avoid unless you're up for more work which you may be since you're still here!

