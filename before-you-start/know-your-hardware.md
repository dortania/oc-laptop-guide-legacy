# Know Your Hardware

## Know your Hardware!

Knowing your hardware is the single most imporant component of your Hackintosh journey. Your laptop has a lot of components ranging from the CPU and memory to WIFI and USB ports. Learning everything you can down to the physical IDs and locations of components on the bus can make the difference between a successful and relatively painless Hackintosh experience, and failure.

Remember, the only person responsible for the success or failure of your build is you! With that in mind, it's important to note that you are also responsible should anything go wrong or if your computer breaks by following any of the advice or guidance provided by this guide.

### Vendor Manuals

Your laptop vendor publishes information that documents the general makeup of your system. You want to start here. Download the users manual, and read it from cover to cover. If your vendor also publishes a service manual, that will be very helpful to read in advance. Examples of where this documentation will come in handy would be locating BIOS settings, or identifying part numbers of hardware for deeper inspection.

### Native OS Tools

Your system probably ships with Windows, use the built-in tools such as **System Information** and **Device Manager** to collect general information about your hardware.

### Linux Tools

Keep a bootable live USB of your favorite Linux distribution close by as it will include a variety of tools for deep inspection of devices. Tools such as:

* `lspci` \(Display detailed information about your PCI/PCIe devices\)
* `lsusb` \(Same for USB devices\)
* `dmidecode` \(Detailed dump of DMI configuration data including memory slots, firmware, and more\)

A Linux USB will also be helpful in the event you need to collect information about your devices, for example it would be used to collect detailed information about your audio codec for troubleshooting purposes.

## Incompatible Hardware

There are a lot of devices that are difficult or impossible to make work with macOS. These devices simply aren't supported, or may be partially supported but you could have serious issues.

### AMD CPUs

AMD CPUs and their associated iGPUs are unsupported in macOS. While it's possible to work around these issues with CPU patches and adding a dGPU on a desktop, you cannot add a dGPU to a laptop. AMD systems with dGPUs often use software mux switches which are also incompatible with macOS.

### Intel CPUs

macOS Mojave doesn't support any Intel processors earlier than generation 3 core CPUs due to a lack of Metal support in the integrated GPU. It is possible to get these processors working with various patches and hacks but you are better off staying with earlier versions of macOS instead.

The following CPUs won't work:

* Pentiums
* Celerons
* Atoms

### WIFI cards

Do you have an Intel, Atheros, Realtek, or Broadcom device that isn't a 

* BCM94352Z
* BCM943602BAED, or
* BCM94352HMB

Replace it with an m.2 or Mini PCIe device based one of those three cards. This will make WiFi possible. Do note, that installation of MacOS requires internet connection. You can boot without internet, but when you want to install MacOS then you'll need internet. Easiest is to use a wired connection to get the laptop going. If you do not have a wired ethernet connector in your laptop you need to get your WiFi working.

The Dell DW1560, DW1830, Lenovo 04X6020, and Azurewave BCM94352HMB are good choices.

The DW1820A \_\*\*\_works on some desktop Hackintoshes but it is incompatible with laptop Hacks.

If you are installing an earlier version of macOS than Mojave, Atheros 94XX and 95XX Mini PCIe cards are supported. These cards can be used on Mojave by adding support back to macOS, but this configuration is not supported by this guide as this method could stop working at any time or could cause stability issues.

Before purchasing make sure your Laptop doesn't have a whitelist that prevents the card from working. Yes, that's actually a thing. You will also need to make sure that the WIFI device is socketed and not soldered to the motherboard which has become a common theme among laptops over the last few years. I almost all cases you either need to consult the manual, take the laptop apart (recommended) or use the internet to get the information you need.

Note: The internet can be very good resource to learn how to take your laptop apart with minimum risk of making damage.

### GPUs

Does your laptop have an AMD/ATI or Nvidia GPU? You'll need to disable it and use the onboard Intel iGPU instead. 99% of laptop dGPUs use software mux switches which are not supported in macOS due to Apple's use of a hardware mux switch. Only on some RARE configurations, especially Workstation-grade laptops. <- FreedomDK: Sentence does not make quite sense.  But that is still experimental and very unstable for some devices.

### Built in SDCard Readers

If you have an SDCard reader that's connected through an internal USB hub, it's probably not going to work. Do yourself a favor if you need SDCard support and buy an inexpensive reader. If your SDCard reader is PCI based there is a small chance that it will work in macOS, but you're probably going to want to fake the ID so it appears as a native reader to the operating system.

### Fingerprint Readers

Apple only supports their implementation of fingerprint readers through Touch ID. You may as well turn it off in your BIOS and save a little bit of power. While you're in there, turn off that USB SDCard reader as well!

### SmartCard Readers

While some can be supported somehow, they're mostly not, and thankfully they're USB connected and can be disabled by either the BIOS/UEFI firmware Setup screen \(if possible\) or by remapping your USB ports and disabling it. It can potentially save some juice.

