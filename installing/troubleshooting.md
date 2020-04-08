# Troubleshooting

Hackintosh Slav has a [large page of errors and troubleshooting steps](https://desktop.dortania.ml/troubleshooting/troubleshooting) for Hackintoshes in general:

### Scrambled Login Screen

![Scrambled Screen](https://cdn.discordapp.com/attachments/302485086060937219/694780782564212757/image0.jpg)

Enable CSM in your UEFI settings. This may appear as "Boot Legacy Roms" or some other legacy setting.

### Trackpad does not work but keyboard, buttons, and trackstick do!

Make sure that within your Config.plist under Kernal/Add, that VoodooInput is above/before any of the VoodooPS2 and VoodooI2C kexts. Voodooinput needs to be loaded before the VoodooPS2Trackpad kext specifically for it to attach and function.

