# Configuring USB Ports

I agree..the need to configure USB ports does sound a little weird, but it is a necessity on a Hackintosh as Apple uses a very strict configuration for USB and the macOS kernel does not autodetect and configure ports for you. You may be thinking that you don't need it because your USB ports seem to work fine, but mapping ports can help resolve instant wake problems, and things like bluetooth and touch screens not working properly. One item of note, if you ever have to change your SMBIOS it will invalidate your USB map and you'll have to map your ports again.

## Understanding Port Limits

As mentioned previously, macOS has a very strict USB configuration by default, and that includes a hard limit of 15 USB ports. That doesn't sound like it's a big deal, but if your computer has USB 3.0 every 3.0 port uses 2 ports on the bus; a low speed port \(HS\) and a super speed port \(SS\). Your computer has both internal and external USB ports as well, so it's not as simple as just counting the ports you can see and doubling them. To correct this problem long enough to map the ports that you need, we need to add a few patches to the config.plist if they aren't already there. Once the port limit has been lifted and your ports have been mapped, disable the patches to prevent instability in macOS.

### Applying the Port Limit Patches

{% hint style="danger" %}
**DO NOT** use these as a permanent solution, they're only temporary fixes until you map all your USB ports.
{% endhint %}

Mount up your EFI partition if it isn't already and then we'll merge these patches using your favorite text editor \(remember, I prefer [Atom](https://atom.io)\).

Add the patches to an array called KextsToPatch under KernelAndKextPatches. If the array does not exist, go ahead and create it. Save and close the file. If you're using the guide template these patches already exist so just make sure they're enabled.

| Comment | Disabled | Find | Replace | InfoPlistPatch | Name |
| :--- | :--- | :--- | :--- | :--- | :--- |
| USB port limit patch \#1 10.14.x modify by DalianSky\(credit ydeng\) | False | 83FB0F0F | 83FB3F0F | False | com.apple.iokit.IOUSBHostFamily |
| USB port limit patch \#2 10.14.x modify by DalianSky\(credit PMHeart\) | False | 83E30FD3 | 83E33FD3 | False | com.apple.iokit.IOUSBHostFamily |
| USB Port limit patch \#3 10.14.x modify by DalianSky\(credits PMheart\) | False | 83FB0F0F | 83FB3F0F | False | com.apple.iokit.IOUSBHostFamily |
| USB Port limit patch \#4 10.14.x modify by DalianSky\(credits PMheart\) | False | 83FF0F0F | 83FF3F0F | False | com.apple.iokit.IOUSBHostFamily |
| USB Port limit patch \#5 10.15.x modify by DalianSky\(credits PMheart\) | False | 83F90F0F | 83F93F0F | False | com.apple.iokit.IOUSBHostFamily |

## Information About Port Types

There are only three port types that you should be concerned with. These types and their descriptions can be found in the table below.

| Port | Port description |
| :--- | :--- |
| Type 0 | This is a standard external USB 2.0 port. |
| Type 3 | This port is an external USB-A or USB-C 3.0 port. |
| Type 255 | This is a port on the motherboard for internal devices. |

## Are My Devices Internal or External?

Well, they're probably both actually. You would determine if it's an internal or external port by whether it is inside the computer, or outside as a port that you plug devices into. Some common examples can be found in the table below.

| Device | Location |
| :--- | :--- |
| Thumb Drives | External |
| Bluetooth inside my computer \(m.2 or Mini PCIe\) | Internal |
| Bluetooth that I plug in | External |
| Touch Screen | Internal |
| Webcam | Internal |
| WIFI Dongle | External |
| SDCard Reader that I plug in | External |

Remember, internal devices are of type 255 and external devices depend on the type of port. If you don't know what kind of external ports you have, consult your laptop service manual or user guide.

## Inject All The Ports!

A prerequisite to mapping your ports is injecting every port imagineable into your installation temporarily. That allows the mapping utility to see all of the ports available to your system. The kernel extension needed for this to work is called USBInjectAll, and it can be found in Rehabman's Bitbucket repository. Download the kext, and add it to C/k/O. You'll need to reboot before continuing.

[Download USBInjectAll @ Bitbucket](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)

## Mapping USB Ports

Now that you're armed with knowledge about your USB devices and you've fixed that pesky port limit, it's time to map your ports. So, what do we need to do to map the ports? Good question! For this, we need to use corpnewt's wonderful USBMap utility. Go ahead and download that now. It's important to read the utility's README for information on how to use the tool, and how to adjust your ports as described above.

[Visit USBMap @ Github](https://github.com/corpnewt/USBMap)

After saving your changes, the USBMap tool will create several files and open a finder window. Take the USBMap.kext that you've just created and copy it to C/k/O. Delete USBInjectAll, you won't need it any longer.

After verifying the ports you need are mapped and working properly, disable the port limit patches.

