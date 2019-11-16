# Dual booting​ with Bluetooth Devices

## Pairing with Windows and macOS

Hackintosh users often dual boot with Windows and want to use their keyboards, mice, and other Bluetooth devices regardless of the OS they've booted. While it seems like an impossible task, it's actually relatively simple by sharing the macOS paring keys with Windows the same way Bootcamp does on a real Mac.

First things first, you have to pair the device in Windows. I know that seems a little weird, but this is required so Windows has the registry keys that we need to update later. After that, boot into macOS and pair it there.

Once it's paired, you will need to get the link keys from the bluetooth plist in macOS. If you're unsure which link key is which, you can find the MAC address of your device by option-clicking the Bluetooth applet in your menu bar and expanding the connected device you want to share. To get the keys, run the following command in terminal:

```text
$ sudo defaults read /private/var/root/Library/Preferences/com.apple.Bluetoothd.plist LinkKeys
```

To find the link key for the device, match the devices MAC address in the output. The value assigned to the MAC address is the link key. It may look something like this:

```text
{
    “00-11-22-33-44-55” =     {
        “aa-bb-cc-dd-ee-ff“ = <01122334 45566778 89900112 23344556>;
    };
}
```

Once you have the link key, convert the endianness \(big endian to little endian\) so you can use it in Windows. Using the link key in the block above, the conversion would look like this:

```text
01122334 45566778 89900112 23344556
56453423 12019089 78675645 34231201
```

Save the MAC address and the converted key in a location where you can retrieve it in Windows, and reboot into Windows with the device turned off. Open regedit and browse to the Bluetooth keys.

```text
HKLM\SYSTEM\CurrentControlSet\Services\BTHPORT\Parameters\Keys
```

Replace the existing key matching the device's MAC address with the key that you converted in the earlier step. Restart the machine with the device turned on.

_Note: If you're using an Apple device such as a Keyboard or Trackpad, do not plug the device into your computer for charging. Doing so will cause the link keys to change requiring you to repeat this process again._

