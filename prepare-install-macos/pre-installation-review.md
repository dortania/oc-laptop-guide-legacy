# Pre-Installation Review

Congratulations! If you've made it this far you should be ready to install macOS. Before we do, take one last look at your EFI configuration to make sure it matches the tree view below.

```text
EFI
├── BOOT
│   └── BOOTX64.efi
└── CLOVER
    ├── ACPI
    │   ├── origin
    │   └── patched
    │       ├── SSDT-GPI0.aml
    │       ├── SSDT-PNLF.aml
    │       └── SSDT-XOSI.aml
    ├── CLOVERX64.efi
    ├── config.plist
    ├── drivers
    │   └── UEFI
    │       ├── ApfsDriverLoader.efi
    │       ├── FwRuntimeServices.efi
    │       ├── FSInject.efi
    │       ├── OcQuirks.efi
    │       ├── HFSPlus.efi
    │       └── VirtualSmc.efi
    ├── kexts
    │   └── Other
    │       ├── Lilu.kext
    │       ├── NoTouchID.kext
    │       ├── SMCBatteryManager.kext
    │       ├── SMCLightSensor.kext (if applicable)
    │       ├── SMCProcessor.kext
    │       ├── SMCSuperIO.kext
    │       ├── USBInjectAll.kext
    │       ├── VirtualSMC.kext
    │       ├── VoodooI2C.kext
    │       ├── VoodooI2CHID.kext
    │       └── VoodooPS2Controller.kext
    ├── themes
    └── tools (optional)
        ├── Shell32.efi
        ├── Shell64.efi
        ├── Shell64U.efi
        └── bdmesg.efi
```

Looks good? Great! Let's start the installation!

