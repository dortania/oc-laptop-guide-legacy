# Embedded Controller \(EC\)

## Embedded Controller \(EC\)

Starting in macOS 10.15 \(Catalina\), macOS now stalls when booting and attaching to an EC with the device id of "PNP0C09" and is not named EC within the ACPI tables. This normally manifests itself as messages similar to `apfs_module_start...` or `waiting for root device` when attempting to boot. With a desktop, we would normally just put in a fake embedded controller with the correct properties and call it a day - but for laptops we rename the existing embedded controller in order to get battery status working. The general gist is that:

1. You need to dump your DSDT
2. Decompile said DSDT
3. Figure out which EC is the main one in your DSDT
4. Put in the right rename into the patches section of your Config.plist

For more info, you can [read this guide,](https://khronokernel.github.io/EC-fix-guide/) also by Hackintosh Slav. This page is more or less a copy and paste from the laptop page specifically.

## Dumping the DSDT

#### [MaciASL](https://github.com/acidanthera/MaciASL/releases)

* When you open MaciASL, it should open directly to your system's DSDT. If it does not, you can go into the top left corner and hit "File" -&gt; "New from ACPI" -&gt; "DSDT". What you are seeing is the decompiled form of your DSDT, so you can skip ahead to figuring out which EC you need.
* This needs to be done on the target computer, and if you have any ACPI fixes or patches applied from Clover or OpenCore, those will be applied to the DSDT you see in MaciASL.

#### [SSDTTime](https://github.com/corpnewt/SSDTTime)

* Can be ran on either Windows or Linux
* If you don't have an internet connection on that device, or are using Python 2 (or iasl otherwise fails to download) you may need to download `iASL` and put it in the `scripts` folder.
  * [macOS](https://bitbucket.org/RehabMan/acpica/downloads/iasl.zip)
  * [Windows](https://acpica.org/sites/acpica/files/iasl-win-20180105.zip)
    * You also want `acpidump.exe` in the `sripts` folder as well.
  * [Linooox](http://amdosx.kellynet.nl/iasl.zip)

#### [acpidump.exe](https://acpica.org/sites/acpica/files/iasl-win-20180105.zip)

* In command prompt run `path/to/acpidump.exe -b -n DSDT -z`, this will dump your DSDT as a .dat file. Rename this to DSDT.aml

#### Pressing F4 in Clover

* This dumps your DSDT in `EFI/CLOVER/ACPI/origin`. This folder must exist bfeore you dump though.

#### [acpidump.efi](https://github.com/khronokernel/Opencore-Vanilla-Desktop-Guide/tree/master/extra-files/acpidump.efi.zip)

* Add this to `EFI/OC/Tools` and in your config under `Misc -> Tools` with the argument: `-b -n DSDT -z`. Select this option in OpenCore's picker, then rename DSDT.dat to DSDT.aml. 
* If OpenCore is having issues running acpidump, you can call it from the shell with [OpenCoreShell](https://github.com/acidanthera/OpenCoreShell/releases)\(reminder to add to both `EFI/OC/Tools` and in your config under `Misc -> Tools` \):

```text
shell> fs0: // replace with proper drive

fs0:\> dir // to verify this is the right directory

Directory of fs0:\

01/01/01 3:30p EFI

fs0:\> cd EFI\OC\Tools // note that it's with forward slashes

fs0:\EFI\OC\Tools> acpidump.efi -b -n DSDT
```

## Decompiling the DSDT

So once we have our DSDT we still got a bit of work left, we'll first want to decompile it so we can view it easier. Couple options:

### macOS

So decompiling DSDTs is quite easy with macOS, all you need is [MaciASL](https://github.com/acidanthera/MaciASL). To decompile, just open the file!

### Windows

Decompiling on windows is fairly simple as well, you will need [iasl.exe](https://acpica.org/sites/acpica/files/iasl-win-20180105.zip) and Command Prompt:

```text
path/to/iasl.exe path/to/DSDT.aml
```

![](https://cdn.discordapp.com/attachments/456913818467958789/668211002340409404/unknown.png)

### Linux

Compiling and decompiling with Linux is just as simple, you will need a special copy of [iasl](http://amdosx.kellynet.nl/iasl.zip) and terminal:

```text
path/to/iasl path/to/DSDT.aml
```

## Finding the right EC patch

Now that our DSDT is readable, next search for `PNP0C09`. Should give you something similar to this:

![](https://i.imgur.com/lQ4kpb9.png)

As you can see our `PNP0C09` is found within the `Device (EC0)` meaning this is the device we want to rename.

> What happens if multiple `PNP0C09` show up

When this happens you need to figure out which is the main and which is not, it's fairly easy to figure out. Check each controller for the following properties:

* `_HID`
* `_CRS`
* `_GPE`

Note that only the main EC needs renaming, if you only have one `PNP0C09` then it is automatically your main regardless of properties.

## Applying your EC patch

As you can see from the table below, we'll be renaming our EC listed in the DSDT. Do note you cannot just throw random renames without checking first, as this can cause actual damage to your laptop.

| Comment | Find\*\[HEX\] | Replace\[HEX\] |
| :--- | :--- | :--- |
| change EC0 to EC | 4543305f | 45435f5f |
| change H\_EC to EC | 485f4543 | 45435f5f |
| change ECDV to EC | 45434456 | 45435f5f |
| change PGEC to EC | 50474543 | 45435f5f |

Add the rename as shown below:

| Comment | Type | Change XXXX to EC |
| :--- | :--- | :--- |
| Enabled | Boolean | YES |
| Count | Number | 0 |
| Limit | Nuber | 0 |
| Find | Data | xxxxxxxx |
| Replace | Data | xxxxxxxx |

![](https://cdn.discordapp.com/attachments/456913818467958789/668667268254793728/Screen_Shot_2020-01-19_at_9.04.50_PM.png)

