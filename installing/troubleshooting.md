# Troubleshooting

Hackintosh Slav has a large page of errors and troubleshooting steps for Hackintoshes in general:

{% embed url="https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/troubleshooting/troubleshooting" %}

## Trackpad does not work but keyboard, buttons, and trackstick do!

Make sure that within your Config.plist under Kernal/Add, that VoodooInput is above/before any of the VoodooPS2 and VoodooI2C kexts. Voodooinput needs to be loaded before the VoodooPS2Trackpad kext specifically for it to attach and function.

