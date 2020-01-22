# Setting up Input Devices

Input devices on laptops come in a few varietys, each with its own set of quirks. Fortunately, we've reached a clearing and you can rest a bit as this should be one of the simpler things to set up on your laptop. Let's start with keyboards, because they're easy.

## Keyboards

Most laptop keyboards are PS/2 devices. Yep, it's 2019 and PS/2 is still a thing. Crazy, huh? There's some good news here, you only need to install a single kext to get it working and then you can put the USB keyboard that you're probably using right now back in the closet where it belongs. That kext is VoodooPS2Controller, and you can find it over on Github.

[VoodooPS2 Project Page](https://github.com/acidanthera/VoodooPS2)

Download the latest release and add it to CLOVER placing the VoodooPS2Controller kext in your C/k/O folder. The path should match the tree below.

```text
EFI
└── CLOVER
    └── kexts
        └── Other
            └── VoodooPS2Controller.kext
```

## Touchpads

There are generally two types of touchpads on laptop computers, I2C devices and PS2 devices. If you have a PS2 touchpad, you're probably covered by VoodooPS2Controller, so reboot and see if it works. If you're still reading this, it's probably not working because it's I2C.

In case your touchpad is acting weird or lacks gestures, use the VoodooPS2Controller from Rehabman.

[VoodooPS2Controller Project - Rehabman @ BitBucket](https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/)

## Touch Screens

Good news! If you have one of those fancy 2-in-1 laptops with a touch screen, it is probably supported by VoodooI2C as well! Touch screens are I2C devices, so configuring your touchpad should also enable your touch display. You may find that it's not working right away though, and that's OK! It's probably just due to your USB ports not being fully configured yet.

## Prerequisites for VoodooI2C

I2C touchpads are interesting in that they fully integrate into macOS and support gestures and everything that a real Apple Trackpad supports, except force touch \(obviously\). Not all I2C touchpads are supported by I2C or without extra hotpatches to configure pinning. An unusual requirement for the trackpad to work is functional battery status reporting, so if you don't have that working yet skip to that section and then come back. Most though work fine with just two hotpatches that are pretty simple to install. Let's add them now. First, download SSDT-XOSI.dsl.

[Rehabman's SSDT-XOSI.dsl @ Github](https://raw.githubusercontent.com/RehabMan/OS-X-Clover-Laptop-Config/master/hotpatch/SSDT-XOSI.dsl)

Now that you've saved the file, you'll need to compile it. For that, we need to download maciASL.

[maciASL Project @ Github](https://github.com/acidanthera/MaciASL)

Run maciASL, and open the SSDT-XOSI.dsl that you created previously. Save the file as a binary to CLOVER/ACPI/patched/SSDT-XOSI.aml. Note: When saving as a binary, maciASL will automatically compile the SSDT.

### \_OSI to XOSI Rename

Now that you've added the XOSI SSDT patch, you need to add a patch to Clover. Open your config.plist with your plist editor of choice and add the following patch under ACPI/DSDT/Patches. If you're using the guide template this rename already exists so just make sure it's enabled.

| Key | Type | Value |
| :--- | :--- | :--- |
| Comment | String | \_OSI to XOSI |
| Disabled | Bool | False |
| Find | Data | 5f4f5349 |
| Replace | Data | 584f5349 |

Save config.plist and close the editor.

{% hint style="info" %}
You should have this already if you're using the config template from before. Make sure it's enabled.
{% endhint %}

### SSDT-GPI0

Next we'll create a stub function for GPI0 using another SSDT patch. Paste the following data into maciASL.

```text
DefinitionBlock ("", "SSDT", 2, "hack", "I2C", 0)
{
    External (_SB.PCI0.GPI0, DeviceObj)
    Scope (_SB.PCI0.GPI0)
    {
        Method (_STA, 0)
        {
            Return (0x0F)
        }
    }
}
```

Using maciASL, save this SSDT patch as a binary to CLOVER/ACPI/patched/SSDT-GPI0.aml

## Installing VoodooI2C

With the hotpatching out of the way, now it's time to install VoodooI2C. Generally you only want to install VoodooI2C.kext and VoodooI2CHID.kext to C/k/O as this is the proper configuration for most devices regardless of manufacturer. Start there, and then come back after rebooting for a description of the kexts and what their functions are.

[Download VoodooI2C @ Github](https://github.com/alexandred/VoodooI2C)

### Kexts and Satellites

When configuring VoodooI2C, it is important to only apply the kexts that you need. Generally, most users will need VoodooI2C.kext and VoodooI2CHID.kext. The remaining satellites are for a small number of specific devices. Even if you have an Elan or Synaptics touchpad, you should try the HID kext first. If you do learn that you need a different satallite kext, remove the HID kext before applying it.

* VoodooI2C - This is the primary device driver that implements Apple's Trackpad protocols.
* VoodooI2CHID - Implements the Microsoft HID device specification.
* VoodooI2CElan - Implements support for Elan proprietary devices. \(does not work on ELAN1200+, use the HID instead\)
* VoodooI2CSynaptics - Implements support for Synaptics proprietary devices.
* VoodooI2CFTE - Implements support for the FTE1001 touchpad.
* VoodooI2CUPDDEngine - Implements Touchbase driver support.

The VoodooI2C configuration should match the tree view below.

```text
EFI
└── CLOVER
    └── kexts
        └── Other
            ├── VoodooI2C.kext
            └── VoodooI2CHID.kext
```

I know that you've kind of breezed through this one, but we don't want to make it too easy, so here's some required reading.

[VoodooI2C Documentation](https://voodooi2c.github.io/#index)

The documentation covers troubleshooting, GPIO pinning, and other topics that may be important tools in troubleshooting any problems you may face while setting up your touchpad device.

{% hint style="info" %}
For touchscreens, you can also try VoodooI2C with VoodooI2CHID, if it's I2C it should work as long as your GPI0 and XOSI are properly fixed, if it's USB, there is a chance that it may attach. The VoodooI2C documentation doesn't guarantee that all USB touchscreens will work.
{% endhint %}

## Bluetooth Keyboard and Mice

One of the common questions asked is if users can use their bluetooth keyboard and mouse in the BIOS and CLOVER, and the answer to that is yes..but. In order for these devices to work in pre-boot environments like the BIOS, in CLOVER, or even in the FileVault Prebooter; the bluetooth adapter needs to support a profile called HID Proxy. This profile allows your devices to connect to the adapter before booting the OS and it passes them to the system as a USB keyboard and mouse. Without this profile, it is not possible to use these devices until you're in the operating system.

