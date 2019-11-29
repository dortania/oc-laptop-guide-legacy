# Enabling FileVault

## What the heck is FileVault?

FileVault is Apple's solution to whole disk encryption. Once enabled, it will use your CPUs encryption extensions to encrypt and decrypt data as you're using it. The credentials to unlock the disk are tied to your user and iCloud accounts, and integrated into the system for a relatively seemless experience.

## Setting up FileVault

You should never just enable FileVault on a Hackintosh without first installing the prerequisites. Doing so will encrypt your disk, and CLOVER won't have the extensions that it needs to help you decrypt it, leaving you stuck. You can add the extensions to a USB drive later, but let's avoid that scenario altogether by setting it up now.

The first thing you're going to want to do is install VirtualSMC if you have not done so yet. You can learn more about that by clicking [here](../prepare-install-macos/smc-emulation.md).

### Installing AppleSupportPkg

Next you're going to need to download AppleSupportPkg. It contains all of the remaining items that you'll need to get FileVault up and running. Use version **2.0.9** as more recent versions have been merged into OpenCore.

[Download AppleSupportPkg @ Github](https://github.com/acidanthera/AppleSupportPkg)

Browse to CLOVER/drivers/UEFI and delete the following files if they exist.

* SMCHelper.efi
* UsbKbDxe.efi
* UsbMouseDxe.efi
* ApfsDriverLoader.efi

Now, copy the following files from AppleSupportPkg to CLOVER/drivers/UEFI.

* AppleGenericInput.efi
* ApfsDriverLoader.efi
* AppleUiSupport.efi
* VboxHFS \(If you don't already have HFSPlus.efi\)

Excluding the EFI drivers necessary for booting, your EFI folder should look like this.

```text
EFI
└── CLOVER
    └── drivers
        └── UEFI
            ├── ApfsDriverLoader.efi
            ├── AppleGenericInput.efi
            ├── AppleUiSupport.efi
            ├── VBoxHfs.efi
            └── VirtualSmc.efi
```

Finally, reboot. If everything comes up OK, go ahead and enable FileVault.

