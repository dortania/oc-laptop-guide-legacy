# BIOS Configuration

In order to boot macOS you have to make a few adjustments to your laptop's BIOS configuration for compatibility. Some of these settings may not appear in your BIOS, that's OK as many of them can also be corrected in OpenCore. If the parameter isn't found in your laptop's BIOS, it can be helpfull to make a note. Lack of BIOS settings can in some cases be taken care of by settings in your config.plist and related files.

For information on how to access your BIOS configuration, refer to your laptop's users manual.

## Turn off Secure Boot

Disable it, or you won't be able to access OpenCore to boot macOS or the macOS installation media.

Note: Secure Boot comes in different depending on you BIOS. Examples are Secure Boot, Secure Boot Control, Secure Boot Mode and the like. Read the manual in your laptop - in many you can find it online.

## Turn the TPM off

If your computer has a TPM chip, you'll want to turn it off. MacOS can't use it anyway.

Note: TPM = Trusted Platform Management. In some BIOS's also seen as TPM Security (not to be mistaken for Secure Boot), TPM State, TPM Support, TCG Security Device(!) and the like. Again we highly recommend reading the manual for your laptop. 


## Disable VT-D

VT-D is Intel's hardware based IO and device offload technology. It's incompatible with macOS and can cause boot issues, and kernel panics. If you have the option, you should turn it off.

Note: VT-D can be shown in many ways in your BIOS, such as Intel VT for Directed I/O, IO Virtualization and the like. With the risk of being reptitive: Read the manual to your laptop.

## Graphics DVMT-Preallocation

This is the initial memory used for your Intel GPU. By default it's usually 32MB, but for macOS it should be set to 64MB. If you don't have this option in your BIOS it can be patched later.

Note: In BIOS can be stated as IGD - DVMT, Graphics Memory, DVMT/FIXED and similar. Check your manual if in doubt.

## SATA/AHCI/RAID

You always want your Hard Disks and SSDs to operate in AHCI mode. In any of the other modes, macOS won't see them for installation.

## Disable that dGPU!

If you can, if not you can disable it with a patch later. You may need to boot the installation media with an extra argument \(-x\) to use safe mode though. This setting can also be left on if you want to use the dGPU within Windows or other operating systems.

## Enable Legacy USB support

This is sometimes needed for your keyboard to work in OpenCore. It's always safe to set if it's available.

## Enable XHCI Handoff

This parameter tells the computer to hand control of the XHCI \(USB 3\) bus to macOS, if you have it make sure it's enabled.

## Disable Fast Boot

Fast Boot establishes a cache to boot into Windows more quickly, but that can be problematic when booting into macOS. If you have the parameter, you'll want to turn it off.

## Disable Wake on Lan

Wake on Lan is one of the leading causes of sleep wakeups, if you have the option in your BIOS to disable it, it's recommended that you do.

## Disable Unsupported Devices

That fingerprint and some SDCard reader won't work anyway, so turn off the ports if you can and save power! \(if possible\)

