# OpenCore Template

## The Config.plist in a Nutshell

Before continuing, you need a bit of knowledge about the config.plist which is a structured text document \(XML\) that provides OpenCore with the instructions that it needs to customize your laptop to boot and use macOS. It is a complex, and sometimes daunting dictionary of key value pairs. There are a variety of tools available to help configure it, here are a few to help get you started.

**ProperTree** - This is a cross platform plist editor written by CorpNewt that works on Windows, Linux, and macOS. It's a very simple, but very powerful tool that simplifies changing parameters while also helping ensure you don't break formatting rules.

[Download ProperTree from Github.](https://github.com/corpnewt/ProperTree)

**XCode** - You probably don't need or want to install an entire development suite for editing your plist, but it is an option so it's worth mentioning. If you're looking to take a tank to a knife fight, XCode can be installed through the App Store.

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
When following the guides below  
 DO NOT insert SSDT-EC. For laptops, we rename the EC within the ACPI tables.

* DO NOT insert SSDT-EC. Further down this page, we'll go over how to rename the EC already exisitng within your ACPI tables.
* Instead of using the models provided in the guide, use one of the models provided by the table below each link \(Generally what matches your config closest\)
{% endhint %}

#### [Ivy Bridge](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-config.plist/ivy-bridge)

| Model | Graphics | Size |
| :--- | :--- | :--- |
| MacBookPro9,1 | HD 4000/GeForce GT 650M | 15" |
| MacBookPro9,2 | HD 4000 | 13" |
| MacBookPro10,1 | HD 4000/GeForce T 650M | 15" |
| MacBookPro10,2 | HD 4000 | 13" |

#### Haswell

| Model | Graphics |
| :--- | :--- |
| MacBookPro11,1 |  |
| MacBookPro11,2 |  |
| MacBookPro11,3 |  |
| MacBookPro11,4 |  |
| MacBookPro11,5 |  |

