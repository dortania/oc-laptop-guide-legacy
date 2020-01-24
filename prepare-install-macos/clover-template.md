# OpenCore Template

## The Config.plist in a Nutshell

Before continuing, you need a bit of knowledge about the config.plist which is a structured text document \(XML\) that provides OpenCore with the instructions that it needs to customize your laptop to boot and use macOS. It is a complex, and sometimes daunting dictionary of key value pairs. There are a variety of tools available to help configure it, here are a few to help get you started.

**ProperTree** - This is a cross platform plist editor written by CorpNewt that works on Windows, Linux, and macOS. It's a very simple, but very powerful tool that simplifies changing parameters while also helping ensure you don't break formatting rules.

[Download ProperTree from Github.](https://github.com/corpnewt/ProperTree)

**XCode** - You probably don't need or want to install an entire development suite for editing your plist, but it is an option so it's worth mentioning. If you're looking to take a tank to a knife fight, XCode can be installed through the App Store. It is also woth noting that Xcode has issues editing plists starting with Xcode 11.

**Clover Configurator** - While not generally recommended, sometimes it's nice to have a GUI with tips. This tool will give you that GUI, but it's been known to break things now and then so make sure you back your config.plist up before saving.

[Download Clover Configurator from the project website.](https://mackie100projects.altervista.org/download-clover-configurator/)

**Cloud Clover Editor** - This is exactly what it says, a cloud application for all of your OpenCore configuration needs.

[Visit the Clover Cloud Editor website](https://cloudclovereditor.altervista.org/cce/index.php).

**PlistBuddy** - This is a command line utility that's built into macOS. It's a great utility for making changes to plist files from scripts or the command line.

```text
$ /usr/libexec/PlistBuddy --help
```

## Making your own Config.plist

OpenCore config files \(and EFIs in general\) tend to be very similar to Desktop ones. Because of this, I will link you to Hackintosh Slav's guide _again_ to set up the Config.plist.

{% hint style="info" %}
When following the guides below:

* DO NOT insert SSDT-EC. On the next page, we'll go over how to rename the EC already existing within your ACPI tables.
* Instead of using the imac models provided in the guide, use one of the models provided by the table below each link \(Generally choose the one that matches your laptop closest\)
{% endhint %}

{% hint style="danger" %}
Remember that anytime you add a Kext, Driver, Tool, or SSDT, you need to add it to your Config.plist. The OC Snapshot tool from ProperTree can automatically add these for you.
{% endhint %}

#### [Ivy Bridge](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/ivy-bridge)

| Model | Graphics | Size | Year |
| :--- | :--- | :--- | :--- |
| MacBookPro9,1 | HD 4000/GeForce GT 650M | 15" | 2012 |
| MacBookPro9,2 | HD 4000 | 13" | 2012 |
| MacBookPro10,1 | HD 4000/GeForce T 650M | 15" | 2012/13 |
| MacBookPro10,2 | HD 4000 | 13" | 2012/13 |

#### [Haswell](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/haswell)

| Model | Graphics | Size | Year |
| :--- | :--- | :--- | :--- |
| MacBookPro11,1 | Irs 5100 | 13" | 2013/14 |
| MacBookPro11,2 | Irs Pro 5200 | 15" | 2013/14 |
| MacBookPro11,3 | Irs Pro 5200/GeForce GT 750M | 15" | 2013/14 |
| MacBookPro11,4 | Irs Pro M5200 | 15" | 2015 |
| MacBookPro11,5 | Irs Pro 5200/Radeon R9 M370x | 15" | 2015 |

#### [Broadwell \(Uses Haswell config\)](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/haswell)

| Model | Graphics | Size | Year |
| :--- | :--- | :--- | :--- |
| MacBookPro12,1 | Iris 6100 | 13" | 2015 |

#### [Skylake](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/skylake)

| Model | Graphics | Size | Year |
| :--- | :--- | :--- | :--- |
| MacBookPro13,1 | Iris 540 | 13" | 2016 |
| MacBookPro13,2 | Iris 550 | 13" | 2016 |
| MacBookPro13,3 | HD 530/Radeon Pro 450 | 15" | 2016 |

#### [Kaby Lake](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/kaby-lake)

| Model | Graphics | Size | Year |
| :--- | :--- | :--- | :--- |
| MacBookPro14,1 | Iris Plus 640 | 13" | 2017 |
| MacBookPro14,2 | Iris Plus 650 | 13" | 2017 |
| MacBookPro14,3 | HD 630/Radeon Pro 555 | 15" | 2017 |

#### [Coffee Lake](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/coffee-lake)

| Model | Graphics | Size | Year |
| :--- | :--- | :--- | :--- |
| MacBookPro15,1 | UHD 630/Radeon Pro 555X | 15" | 2018 |
| MacBookPro15,2 | Iris Plus 655 | 13" | 2018 |
| MacBookPro15,3 | UHD 630/Vega 16/20 | 15" | 2018 |
| MacBookPro15,4 | Iris Plus 645 | 13" | 2019 |

#### [Coffee Lake Refresh \(14nm +++++++++++++++++++\)](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/coffee-lake)

| Model | Graphics | Size | Year |
| :--- | :--- | :--- | :--- |
| MacBookPro16,1 | UHD 630/Radeon Pro 5300M | 16" | 2019 |

