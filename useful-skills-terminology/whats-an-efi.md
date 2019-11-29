# What's an EFI?

An EFI partition is where a computer stores the bits needed for the system to boot. In the case of a Hackintosh, it's where CLOVER and all of the drivers and configurations that CLOVER needs to start your system are stored. As you are building your Hackintosh, you are going to become intimately familiar with this partition and its content. Older computers that don't support EFI boot CLOVER using a legacy stub. Here's how you mount it.

First, inspect your partitions and find your EFI. If you have two hard disks installed, be careful to determine which one contains the correct EFI as the IDs themselves can change on every startup.

```text
$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.1 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                 Apple_APFS Container disk1         499.9 GB   disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +499.9 GB   disk1
                                 Physical Store disk0s2
   1:                APFS Volume macOS                   199.9 GB   disk1s1
   2:                APFS Volume Preboot                 46.5 MB    disk1s2
   3:                APFS Volume Recovery                510.3 MB   disk1s3
   4:                APFS Volume VM                      20.5 KB    disk1s4
```

In the example above, you'll note that you have one internal disk and one synthesized disk. The synthesized disk is a container that holds all of your operating system bits. The source for this container can be found at /dev/disk0s2. You should also note that the EFI partition is located on the internal physical disk at /dev/disk0s1. This is the partition that you should mount for CLOVER. Here's how you would do it.

```text
$ sudo mkdir /Volumes/EFI
$ sudo diskutil mount -mountPoint /Volumes/EFI /dev/disk0s1
```

That's it, you're ready to work on your CLOVER installation! Simple! When you've finished editing your CLOVER folder, you can eject it in Finder or from Terminal like this.

```text
$ diskutil unmount /Volumes/EFI
```

## What are EFI drivers?

EFI drivers are used by CLOVER to bootstrap macOS and start the process of loading the kernel and services that get you to your desktop. They've moved around a bit recently, here's where they go.

### Before CLOVER r4985

Legacy EFI drivers should be installed to CLOVER/Drivers64

UEFI drivers should be installed to CLOVER/Drivers64UEFI

### CLOVER r4985 and Later

Legacy EFI drivers should be installed to CLOVER/drivers/BIOS

UEFI drivers should be installed to CLOVER/drivers/UEFI

## So what's LEGACY?

Another great question! For many years computers used a basic data structure to identify where your data was stored, and how to boot your operating system. This method added a small amount of data to the very start of your hard drive. Beginning in 2004 and adopted later by vendors, Intel established a new method of booting a computer called UEFI that was 32bit and provided significantly greater flexibility. If you have support for UEFI, you should use it. If you don't however, CLOVER will still accomodate you. You will also access your CLOVER directory the same way, the major difference is how the system recognizes and bootstraps the CLOVER bootloader.

As Legacy is well..Legacy..these types of installations usually require a lot of additional work which is not covered by this guide.

