# Pre-Installation Review

Congratulations! If you've made it this far you should be ready to install macOS. Before we do, take one last look at your EFI configuration to make sure it matches the tree view below.

```text
EFI
├── BOOT
│   └── BOOTX64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-GPI0.aml
    │   ├── SSDT-PNLF.aml
    │   └── SSDT-XOSI.aml
    ├── OpenCore.efi
    ├── config.plist
    ├── Drivers
    │   ├── ApfsDriverLoader.efi
    │   ├── FwRuntimeServices.efi
    │   └── HFSPlus.efi
    ├── Kexts
    │   ├── Lilu.kext
    │   ├── NoTouchID.kext
    │   ├── SMCBatteryManager.kext
    │   ├── SMCLightSensor.kext // Only use if you have a light sensor.
    │   ├── SMCProcessor.kext
    │   ├── SMCSuperIO.kext // IO status reporting, causes panics on some systems.
    │   ├── USBInjectAll.kext
    │   ├── VirtualSMC.kext
    │   ├── VoodooI2C.kext
    │   ├── VoodooI2CHID.kext
    │   ├── VoodooInput.kext
    │   └── VoodooPS2Controller.kext
    └── Tools // These aren't needed to boot but can be useful
        ├── VerifyMSR2.efi
        └── acpidump.efi
```

Looks good? Great! Let's start the installation!

