# CLOVER Template

## Using the Vanilla Laptop Guide Template

Linked below is a generic config.plist for laptops which is intended to help take some of the pain out of installing macOS on a laptop. Here are a few important things you'll need to know when using this template.

* The template is generic and must be edited to suit your configuration. Do not expect it to just work.
* Not all of the patches are enabled by default, so review the entire template. Only enable what you need and disable what you don't.
* You may need to expand upon this template after installation to enable some of the hardware in your system.

You can find this template in the Vanilla Laptop Guide artifacts repository.

[Vanilla Laptop Guide Artifacts Repository @ Github](https://github.com/fewtarius/laptop-guide-artifacts)

Once you have your config copy it to the CLOVER folder in your EFI, saving it as config.plist. Remember, you will need to make some changes to customize it for your system. The location of config.plist should match the tree view below.

```text
EFI
└── CLOVER
    └── config.plist
```

## Patch Overview

### ACPI Patches

| Patch | Default | Function |
| :--- | :--- | :--- |
| change APSS to APXX | Disabled | Can cause AppleIntelCPUPowerManagement to panic, no need to enable if APSS does not exist in ACPI |
| change \_DSM to XDSM | Disabled | Renames device specific methods.  May be necessary for VoodooI2C. |
| change EHC1 to EH01 | Disabled | Patch USB 2.0 methods for macOS. |
| change EHC2 to EH02 | Disabled | Patch USB 2.0 methods for macOS. |
| change \_OSI to XOSI | Enabled | Rename \_OSI.  Pair with SSDT-XOSI.aml found later in the guide. |
| change XHC1 to XHC | Enabled | Patch USB 3.0 methods for macOS. |
| change SAT0 to SATA | Disabled | Patch SATA for macOS. |
| change LPC to LPCB | Disabled | Patch low pin count bus for macOS. |
| change \_REG to XREG in EC0 | Disabled | May be necessary for battery status. |
| ALS: change Method\(RALS,0,S\) XALS | Disabled | May be necessary for ambient light sensor |

## Fix IRQ Conflicts

The table below contains some common fixes that help to eliminate any potential IRQ conflicts that may be preventing your touchpad and audio device from working properly. Enable them in config.plist under ACPI/DSDT/Fixes.

| Fix | Type | Parameter | Description |
| :--- | :--- | :--- | :--- |
| FixIPIC | Bool | YES | Deletes IRQ from IPIC device. |
| FixTMR | Bool | YES | Excludes IRQ from TMR device. |
| FixRTC | Bool | YES | Excludes IRQ from RTC device. |
| FixHPET | Bool | YES | Adds IRQ to HPET device. |

## Kernel Patches

| Patch | Default | Function |
| :--- | :--- | :--- |
| MSR 0xE2 \_xcpm\_idle instant reboot\(c\) Pike R. Alpha | Enabled | Prevents instant reboot on systems without the ability to disable the MSR parameter in your BIOS. |

## Kext Patches

| Patch | Default | Function |
| :--- | :--- | :--- |
| Enable TRIM for SSD | Enabled | Enables native trim support for SATA SSDs |
| Prevent Apple I2C kexts from attaching to I2C controllers, credit CoolStar | Enabled | Helper to allow VoodooI2C to start before Apple native I2C drivers. \(Two patches\) |

## The Config.plist in a Nutshell

Before continuing, you need a bit of knowledge about the config.plist which is a structured text document \(XML\) that provides CLOVER with the instructions that it needs to customize your laptop to boot and use macOS. It is a complex, and sometimes daunting dictionary of key value pairs. There are a variety of tools available to help configure it, here are a few to help get you started.

**ProperTree** - This is a cross platform plist editor written by CorpNewt that works on Windows, Linux, and macOS. It's a very simple, but very powerful tool that simplifies changing parameters while also helping ensure you don't break formatting rules.

[Download ProperTree from Github.](https://github.com/corpnewt/ProperTree)

**XCode** - You probably don't need or want to install an entire development suite for editing your plist, but it is an option so it's worth mentioning. If you're looking to take a tank to a knife fight, XCode can be installed through the App Store.

**Clover Configurator** - While not generally recommended, sometimes it's nice to have a GUI with tips. This tool will give you that GUI, but it's been known to break things now and then so make sure you back your config.plist up before saving.

[Download Clover Configurator from the project website.](https://mackie100projects.altervista.org/download-clover-configurator/)

**Cloud Clover Editor** - This is exactly what it says, a cloud application for all of your CLOVER configuration needs.

[Visit the Clover Cloud Editor website](https://cloudclovereditor.altervista.org/cce/index.php).

**PlistBuddy** - This is a command line utility that's built into macOS. It's a great utility for making changes to plist files from scripts or the command line.

```text
$ /usr/libexec/PlistBuddy --help
```

## OK OK, but What Do All of Those Keys and Values Mean?!

Well, there are so many CLOVER parameters that it's just too much to cover here, so instead you should have a look at their WIKI.

[Visit the CLOVER WIKI.](https://sourceforge.net/p/cloverefiboot/wiki/Home/)

Got it? Good!

