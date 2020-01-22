# BIOS Configuration

In order to boot macOS you have to make a few adjustments to your laptop's BIOS configuration for compatibility. Some of these settings may not appear in your BIOS, that's OK as many of them can also be corrected in CLOVER. If the parameter isn't found in your laptop's BIOS, just ignore it.

For information on how to access your BIOS configuration, refer to your laptop's users manual.

## Turn off Secure Boot

Disable it, or you won't be able to access CLOVER to boot macOS or the macOS installation media.

## Turn the TPM off

If your computer has a TPM chip, you'll want to turn it off. macOS can't use it anyway.

## Disable VT-D

VT-D is Intel's hardware based IO and device offload technology. It's incompatible with macOS and can cause boot issues, and kernel panics. If you have the option, you should turn it off.

## Graphics DVMT-Prealloc

This is the initial memory used for your Intel GPU. By default it's usually 32MB, but for macOS it should be set to 64MB. If you don't have this option in your BIOS it can be patched later.

## SATA/AHCI/RAID

You always want your Hard Disks and SSDs to operate in AHCI mode. In any of the other modes, macOS won't see them for installation.

## Disable that dGPU!

If you can, if not you can disable it with a patch later. You may need to boot the installation media with an extra argument \(-x\) to use safe mode though.

## Enable Legacy USB support

This is sometimes needed for your keyboard to work in CLOVER. It's always safe to set if it's available.

## Enable XHCI Handoff

This parameter tells the computer to hand control of the XHCI \(USB 3\) bus to macOS, if you have it make sure it's enabled.

## Disable Fast Boot

Fast Boot establishes a cache to boot into Windows more quickly, but that can be problematic when booting into macOS. If you have the parameter, you'll want to turn it off.

## Disable Wake on Lan

Wake on Lan is one of the leading causes of sleep wakeups, if you have the option in your BIOS to disable it, it's recommended that you do.

## Disable Unsupported Devices

That fingerprint and some SDCard reader won't work anyway, so turn off the ports if you can and save power! \(if possible\)

