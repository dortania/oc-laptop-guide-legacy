# Setting up Input Devices

Input devices on laptops come in a few variants, each with its own set of quirks. Most laptops will want VoodooPS2 for the keyboard. For your trackpad, it's a good idea to check device manager in windows if possible to see what trackpad you have.

## VoodooPS2 \(Keyboard and Trackpad\)

Most laptop keyboard and trackpads are PS/2 devices. You should only need the two listed below. These should cover most Synaptics touchpads used in recent laptops.

* [VoodooInput Project Page](https://github.com/acidanthera/VoodooInput)
  * Used by VoodooPS2 to emulate a Magic Trackpad
  * Should be listed first in your Config.plist as it is required for VoodooPS2Trackpad to load
* [VoodooPS2Controller Project Page](https://github.com/acidanthera/VoodooPS2)
  * Actually is 4 kexts bundled into one kext package.
    * VoodooPS2Controller
    * VoodooPS2Keyboard
    * VoodooPS2Trackpad
    * VoodooPS2Mouse
  * Make sure when adding this to your config.plist that all 4 of these appear

Download the two kexts and place them in your EFI as shown below.

```text
EFI
└── OpenCore
    └── Kexts
        ├── VoodooInput.kext
        └── VoodooPS2Controller.kext
```

{% hint style="warning" %}
#### The Kexts below do not need VoodooInput
{% endhint %}

If you have issues running the above version of VoodooPS2 \(the case of a hard surface trackpad\), you may want to try Rehabman's version of VoodooPS2Controller:

* [VoodooPS2Controller Project - Rehabman @ BitBucket](https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/)

If you have an Alps trackpad, you will want this version here:

* [VoodooPS2-Alps - 1Revenger1](https://github.com/1Revenger1/VoodooPS2-Alps)

## VoodooI2C \(Touchscreens and I2C Trackpads\)

I2C touchpads are interesting in that they fully integrate into macOS and support gestures and everything that a real Apple Trackpad supports, except force touch \(obviously\). Not all I2C touchpads are supported by I2C or without extra hotpatches to configure pinning. An unusual requirement for the trackpad to work is functional battery status reporting, so if you don't have that working yet skip to that section and then come back. Most though work fine with just two hotpatches that are pretty simple to install. Let's add them now. First, download SSDT-XOSI.dsl.

[Rehabman's SSDT-XOSI.dsl @ Github](https://raw.githubusercontent.com/RehabMan/OS-X-Clover-Laptop-Config/master/hotpatch/SSDT-XOSI.dsl)

Now that you've saved the file, you'll need to compile it. For that, we need to download maciASL.

[maciASL Project @ Github](https://github.com/acidanthera/MaciASL)

Run maciASL, and open the SSDT-XOSI.dsl that you created previously. Save the file as a binary to OC/ACPI/SSDT-XOSI.aml. Note: When saving as a binary, maciASL will automatically compile the SSDT.

### \_OSI to XOSI Rename

Now that you've added the XOSI SSDT patch, you need to add a patch to OpenCore. Open your config.plist with your plist editor of choice and add the following patch under ACPI/Patch. If you're using the guide template this rename already exists so just make sure it's enabled.

| Key | Type | Value |
| :--- | :--- | :--- |
| Comment | String | \_OSI to XOSI |
| Enabled | Bool | True |
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

Using MaciASL, save this SSDT patch as a binary to OC/ACPI/SSDT-GPI0.aml

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
└── OpenCore
    └── Kexts
        ├── VoodooI2C.kext
        └── VoodooI2CHID.kext
            └── Contents (already inside, you do not need to add them)
                └── Plugins
                    ├── VoodooGPIO.kext
                    └── VoodooI2CServices.kext
```

I know that you've kind of breezed through this one, but we don't want to make it too easy, so here's some required reading.

[VoodooI2C Documentation](https://voodooi2c.github.io/#index)

The documentation covers troubleshooting, GPIO pinning, and other topics that may be important tools in troubleshooting any problems you may face while setting up your touchpad device.

For this kext to properly load on your OpenCore setup, you must make sure that the loading order is as follows:

1. VoodooI2CServices
2. VoodooGPIO
3. VoodooI2C
4. 
Example: Using VoodooI2C + VoodooI2CHID

```markup
    <dict>
        <key>BundlePath</key>
        <string>VoodooI2C.kext/Contents/PlugIns/VoodooI2CServices.kext</string>
        <key>Comment</key>
        <string></string>
        <key>Enabled</key>
        <true/>
        <key>ExecutablePath</key>
        <string>Contents/MacOS/VoodooI2CServices</string>
        <key>MaxKernel</key>
        <string></string>
        <key>MinKernel</key>
        <string></string>
        <key>PlistPath</key>
        <string>Contents/Info.plist</string>
    </dict>
    <dict>
        <key>BundlePath</key>
        <string>VoodooI2C.kext/Contents/PlugIns/VoodooGPIO.kext</string>
        <key>Comment</key>
        <string></string>
        <key>Enabled</key>
        <true/>
        <key>ExecutablePath</key>
        <string>Contents/MacOS/VoodooGPIO</string>
        <key>MaxKernel</key>
        <string></string>
        <key>MinKernel</key>
        <string></string>
        <key>PlistPath</key>
        <string>Contents/Info.plist</string>
    </dict>
    <dict>
        <key>BundlePath</key>
        <string>VoodooI2C.kext</string>
        <key>Comment</key>
        <string></string>
        <key>Enabled</key>
        <true/>
        <key>ExecutablePath</key>
        <string>Contents/MacOS/VoodooI2C</string>
        <key>MaxKernel</key>
        <string></string>
        <key>MinKernel</key>
        <string></string>
        <key>PlistPath</key>
        <string>Contents/Info.plist</string>
    </dict>
    <dict>
        <key>BundlePath</key>
        <string>VoodooI2CHID.kext</string>
        <key>Comment</key>
        <string></string>
        <key>Enabled</key>
        <true/>
        <key>ExecutablePath</key>
        <string>Contents/MacOS/VoodooI2CHID</string>
        <key>MaxKernel</key>
        <string></string>
        <key>MinKernel</key>
        <string></string>
        <key>PlistPath</key>
        <string>Contents/Info.plist</string>
    </dict>
```

{% hint style="info" %}
### A note on Touchscreens:

If you have one of those fancy 2-in-1 laptops with a touch screen, it may be supported by VoodooI2C as well! Touchscreens are either I2C or USB devices, so configuring your touchpad should also enable your touch display when supported. Some things to keep in mind:

* You'll want to use VoodooI2C with VoodooI2CHID. 
* If it's I2C, it should work as long as your GPI0 and XOSI are properly fixed, and it's USB, there is a chance that it may attach.
* The VoodooI2C documentation doesn't guarantee that all USB touchscreens will work.
* USB Touchscreens may work as a single pointer device while in the installer phase, I2C touchscreens may not work until the full macOS installation boots up.
{% endhint %}

## Bluetooth Keyboard and Mice

One of the common questions asked is if users can use their bluetooth keyboard and mouse in the BIOS and OpenCore, and the answer to that is yes... but! In order for these devices to work in pre-boot environments like the BIOS, in OpenCore, or even in the FileVault Prebooter; the bluetooth adapter needs to support a profile called **HID Proxy**. This profile allows your devices to connect to the adapter before booting the OS and it passes them to the system as a USB keyboard and mouse. Without this profile, it is not possible to use these devices until you're in the operating system.

