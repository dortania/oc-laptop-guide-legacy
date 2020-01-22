# What about KEXTs?

Glad you asked! A kext is a kernel extension that adds functionality and hardware support to macOS. There are some rules around kexts that you need to know, and here they are.

## KEXT Paths

**/System/Library/Extensions \(/S/L/E\)** - This is where Apple stores the kexts that macOS uses to boot and operate. If something is telling you to put a kext here, hard stop because it's probably wrong.

**/Library/Extensions** **\(/L/E\)** - This is a place for 3rd party developers to place signed kexts for hardware that you buy and connect to your computer. These developers have been granted a kernel signing certificate that is used to sign the kexts which tells macOS they're safe to use. If something tells you to put a kext here, hard stop as the same rule applies.

**CLOVER/kexts/Other \(C/k/O\)** - This is the location that CLOVER uses to inject functionality into the operating system for your Hackintosh to boot and operate. This is where all of your Hackintosh kexts go. Did you notice the lack of a slash before CLOVER? That's because you need to reference where you've mounted your EFI partition before adding C/k/O. Remember that, you'll need to add the mount path anywhere in the guide that it's mentioned.

{% hint style="danger" %}
Do **NOT** use S/L/E \(or SLE for short\) to install kexts to on 10.11 and later, it's usually secured by SIP for a reason.

Learn more about System Integration Protection [here](https://support.apple.com/en-us/HT204899).
{% endhint %}

Following this policy keeps your system clean and allows you to fully enable System Integrity Protection increasing the security of your laptop.

