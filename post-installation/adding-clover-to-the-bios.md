# Adding CLOVER to Your Boot Drive

## Add CLOVER to Your Boot Drive

At this point, you should be booting into macOS using your USB stick. So, how do we put CLOVER on your hard drive/ssd? Simple, we just need to copy it. This process is relatively painless, but we're going to need to use the terminal so open that now.

First, we need to mount your USB EFI partition, and the EFI partition on your hard drive or SSD. Remember how we learned about using Disk Utility from the command line in the "[What's an EFI?](../useful-skills-terminology/whats-an-efi.md)" section? Good, because we're doing it twice.

#### Mount the USB EFI

Let's find out which device is our USB stick. We'll use the following command for this:

```text
$ diskutil list
```

Thumb drives are classified as "external" devices, so they're easy to locate. Find the device that matches your USB stick. Our example device is /dev/disk4 and our EFI is the second partition.

```text
/dev/disk4 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *16.0 GB    disk4
   1:                        EFI EFI                     209.7 MB   disk4s1
   2:                  Apple_HFS macOS Install           15.7 GB    disk4s2
```

Now that we know where our USB's EFI is, let's mount it. First we'll create a folder, and then we'll mount it. We'll call the folder 'USBEFI'.

```text
$ sudo mkdir /Volumes/USBEFI
$ sudo diskutil mount -mountPoint /Volumes/USBEFI /dev/disk4s1
```

Alright, now we'll repeat the process for the internal EFI. This time we'll look for an internal physical disk that contains the internal drive's EFI.

```text
$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.1 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                 Apple_APFS Container disk1         499.9 GB   disk0s2
```

Our example is disk0 partition 1, so let's mount it on 'SYSEFI'.

```text
$ sudo mkdir /Volumes/SYSEFI
$ sudo diskutil mount -mountPoint /Volumes/SYSEFI /dev/disk0s1
```

Now that we have both EFIs mounted we just need to copy the files.

```text
$ cd /Volumes/USBEFI
$ cp -rf * ../SYSEFI
```

Lastly, let's unmount those devices so we can reboot.

```text
$ diskutil unmount /Volumes/USBEFI
Volume USBEFI on disk4s1 unmounted
$ diskutil unmount /Volumes/SYSEFI
Volume SYSEFI on disk0s1 unmounted
```

That's it! Unmount your USB stick in Finder, and reboot without it. You may need to go into your BIOS and set Clover first in your boot order. If you're able to boot into CLOVER, you can skip the next section and move onto [Display Configuration](../prepare-install-macos/display-configuration.md).

## Adding CLOVER to your BIOS

Most newer computers will pick up Clover automatically, but if it doesn't appear as a bootable selection in your BIOS here's how to correct it.

### **Quick Reference Guide**

* _map_ - Lists devices that you can boot from.
* _drive:_ - Change to the drive you select. Ex. _FS0:_
* _ls_ or _dir_ - List the content of the selected drive.
* _cd_ - Change directories.
* _bcfg_ - Boot configuration, used to read and write BIOS boot data.

### Add CLOVER to your BIOS

* First, use _map_ to find your devices.
* Once you have an idea of your device, select it by typing _DEVICE:_ replacing device with the actual device. Ex. FS0:
* Use _ls_ to determine the content of the device. It should contain an EFI folder. Use caution to make sure this device is not your USB stick.
* Use `bcfg boot dump` to view your currently configured boot devices \(you may see your USB in this list for validation\).
* Use `bcfg boot add 00 FS0:\EFI\BOOT\BOOTX64.EFI Clover`to add an entry to your boot map.
  * 00 is the boot order ranking, 00 being the very first one, and it increments by one, 01 being the second, 02 being the third and so on.
* Rerun the boot dump command to verify.
* Reboot.

{% hint style="warning" %}
Note that on some laptops with weird BIOS configurations \(like InysideH2O on ACER or HP, and especially VAIO laptops\), this may have no effect on boot priority and would still boot windows if it's still installed. You can check [Fix Clovy Clovy in r/Hackintosh Multiboot guide](https://hackintosh-multiboot.gitbook.io/hackintosh-multiboot/uefi/fix-clovy-clovy).
{% endhint %}

