# SMC Emulation

Apple computers use a chip called the System Management Controller \(SMC\) to manage many of the underlying services that you never really think about. Things such as battery charging, temperatures, and even powering your computer off. A System Management Controller is required for installing macOS so before proceeding to the installation step, let's set one up. This guide recommends VirtualSMC, but we'll also cover FakeSMC.

## About VirtualSMC

VirtualSMC is an advanced SMC emulator. It includes plugins to recognize your CPU, manage your battery status, or even enable a light sensor if you have one. Installing it is simple too, so let's do that now starting with downloading the package from Github. VirtualSMC provides the best support for the largest variety of systems.

[Download VirtualSMC @ Github](https://github.com/acidanthera/VirtualSMC)

VirtualSMC includes two main components and a few plugins for extra functionality. VirtualSMC.efi is a direct replacement for SMCHelper which is included with CLOVER by default. The EFI drivers help CLOVER load FileVault and resume from hibernation. The VirtualSMC kext is the main SMC emulator that helps with system management, and the plugins add support for battery status, light sensors to dim or brighten the display, as well as CPU and IO statistics.

Before installing VirtualSMC you will also need to download and install VirtualSMC's dependency kext, Lilu. Lilu is a patching system for macOS. Many of the kexts that you'll need to boot and operate your hackintosh require it.

[Download Lilu @ Github](https://github.com/acidanthera/Lilu)

### Installing VirtualSMC

Once you have the packages downloaded, mount your EFI partition and delete SMCHelper from CLOVER/drivers/UEFI or wherever the EFI drivers are stored for your installation. Copy the VirtualSMC EFI into the directory to replace it.

Once that's done, move over to C/k/O and make sure there are no instance of FakeSMC. That's a VirtualSMC competitor, we don't need that. Copy all of the VirtualSMC kexts and the Lilu kext into the directory. When you're done it should look something like this.

```text
EFI
└── CLOVER
    ├── drivers
    │       └──UEFI
    │           └── VirtualSmc.efi
    └── kexts
        └── Other
            ├── lilu.kext
            ├── SMCBatteryManager.kext
            ├── SMCLightSensor.kext    // if you have a light sensor
            ├── SMCProcessor.kext
            ├── SMCSuperIO.kext        // for various sensors, may not be needed
            └── VirtualSMC.kext
```

Simple, right? I thought so too. Download VirtualSMC, as we'll need it as we prepare the install media.

[VirtualSMC Documentation @ Github](https://github.com/acidanthera/VirtualSMC/tree/master/Docs)

## FakeSMC

Like VirtualSMC, FakeSMC is another System Management Controller emulator. It operates similar to VirtualSMC but it uses plugins to provide additional functionality to macOS. It is also older, and should only be used when VirtualSMC is found to be incompatible with your hardware.

Installing FakeSMC is easy, you can find it on the Clover ISO that we'll download on the next page. To use it, copy the FakeSMC kext and efi as described in the tree view below.

```text
.
├── drivers
│       └──UEFI
│           └── SMCHelper.efi
└── kexts
    └── Other
        └── FakeSMC.kext
```

## SMC Emulator Conflicts

VirtualSMC and FakeSMC conflict with each other, so you should never have both installed in your EFI at the same time. Chose one or the other, and if both are present remove the SMC emulator that you are not going to use.

