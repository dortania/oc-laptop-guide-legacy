# Embedded Controller \(EC\)

Starting in macOS 10.15 \(Catalina\), macOS now stalls when booting and attaching to an EC with the device id of "PNP0C09" and is not named EC within the ACPI tables. This normally manifests itself as messages similar to `apfs_module_start...` or `waiting for root device` when attempting to boot. With a desktop, we would normally just put in a fake embedded controller with the correct properties and call it a day - but for laptops we rename the existing embedded controller in order to get battery status working. The general gist is that:

1. You need to dump your DSDT
2. Decompile said DSDT
3. Figure out which EC is the main one in your DSDT
4. Put in the right rename into the patches section of your Config.plist

For more info, you can [read this guide,](https://khronokernel.github.io/EC-fix-guide/) also by Hackintosh Slav

## Dumping the DSDT

### [MaciASL](https://github.com/acidanthera/MaciASL/releases)

* When you open MaciASL, it should open directly to your system's DSDT. If it does not, you can go into the top left corner and hit "File" -&gt; "New from ACPI" -&gt; "DSDT". What you are seeing is the decompiled form of your DSDT, so you can skip ahead to figuring out which EC you need.
* This needs to be done on the target computer, and if you have any ACPI fixes or patches applied from Clover or OpenCore, those will be applied to the DSDT you see in MaciASL.

### [SSDTTime](https://github.com/corpnewt/SSDTTime)

* Can be ran on either Windows or Linux

### [acpidump.exe](https://acpica.org/sites/acpica/files/iasl-win-20180105.zip)

* In command prompt run `path/to/acpidump.exe -b -n DSDT -z`, this will dump your DSDT as a .dat file. Rename this to DSDT.aml

### Pressing F4 in Clover

* This dumps your DSDT in `EFI/CLOVER/ACPI/origin`. This folder must exist bfeore you dump though.

### [acpidump.efi](https://github.com/khronokernel/Opencore-Vanilla-Desktop-Guide/tree/master/extra-files/acpidump.efi.zip)

* Add this to `EFI/OC/Tools` and in your config under `Misc -> Tools` with the argument: `-b -n DSDT -z`. Select this option in OpenCore's picker, then rename DSDT.dat to DSDT.aml. 

