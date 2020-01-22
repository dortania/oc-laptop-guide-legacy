# SMC Emulation

{% hint style="info" %}
This page contains info about VirtualSMC.kext and Lilu.kext, which have already been placed into your EFI. You should read this, but there is nothing to do on this page.
{% endhint %}

Apple computers use a chip called the System Management Controller \(SMC\) to manage many of the underlying services that you never really think about. Things such as battery charging, temperatures, and even powering your computer off. A System Management Controller is required for installing macOS so before proceeding to the installation step, let's set one up. This guide recommends VirtualSMC, but we'll also cover FakeSMC.

## About VirtualSMC

VirtualSMC is an advanced SMC emulator. It includes plugins to recognize your CPU, manage your battery status, or even enable a light sensor if you have one. VirtualSMC provides the best support for the largest variety of systems.

VirtualSMC includes two main components and a few plugins for extra functionality. VirtualSMC.efi is a direct replacement for SMCHelper and is now part of OpenCore itself. The EFI driver help OpenCore load FileVault and resume from hibernation. The VirtualSMC kext is the main SMC emulator that helps with system management, and the plugins add support for battery status, light sensors to dim or brighten the display, as well as CPU and IO statistics.

VirtualSMC, along with WhateverGreen and other kexts, depend on Lilu - another kext by the Acidanthera team. Lilu is a patching system for macOS.

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

