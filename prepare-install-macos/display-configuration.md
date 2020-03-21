# Display Configuration

Configuring a laptop's integrated GPU is a lot like configuring a desktop's integrated GPU. You usually start by disabling the dedicated GPU if you have one. This section will talk about the methods that you would use for each component of GPU configuration and how to test that it's working, but there are better guides that already exist to help with the heavy lifting.

## Configuring your Display Adapter \(GPU\)

Depending on your laptop, you may have very little to do to configure your iGPU, or you could have to add an elaborate set of patches to configure stuff like DVMT. The most important thing you'll want here is the WhateverGreen kext and it's dependency, LILU. Lilu is a patching mechanism that's used by multiple kernel extensions, and WhateverGreen is responsible for patching your display adapter\(s\).

If you haven't already added WhateverGreen to your OpenCore EFI, better do that now before continuing. You can get it here. Remember to add it to your config.plist

[Download WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)

### First things first, disable that dGPU!

There are two ways to disable a dGPU in a laptop, the short way and the not so short way. We'll start with the short way because it is really simple. If you don't have a dGPU \(NVidia/ATI dedicated graphics\) you can skip this part and jump straight to configuring your display adapter instead.

To disable your dGPU the short way, add the following to the `NVRAM/Add/7C436110-AB2A-4BBB-A880-FE41995C9F82/boot-args` section of your config.plist.

```text
-wegnoegpu
```

This command instructs Whatevergreen to disable all internal and external dedicated GPUs.

Mission complete? Great! If it doesn't work out the way you would expect, come back and move on the the not so short method using Hackintosh Slav's wonderful guide.

[How to disable your unsupported GPU for MacOS](https://khronokernel-4.gitbook.io/disable-unsupported-gpus/)

Got it all fixed up? Excellent! Now that this is behind us, it's time to configure your iGPU.

### iGPU Configuration

Apple uses Intel graphics cards that have features other GPUs in Intel's line up don't have. For example, with a Hackintosh laptop it is generally not possible to use DRM as FairPlay 2.0 is not supported. Unfortunately it also means that macOS requires that you patch macOS to believe you have a different GPU than you really do. Since these GPUs fall within the same family, we can use the data provided within the macOS Intel driver to build a patch that enables your GPU with full acceleration.

A prerequisite to configuring your iGPU is knowing which GPU you actually have. If you don't already know, look up your CPU using Intel's Ark utility. Once you find your CPU pay attention to the code name, and the graphics adapter. This information will be useful as you configure your GPU patches.

### iGPU Patching

We can instruct Whatevergreen to patch your GPU by passing specific parameters to macOS in your config.plist. The table below describes the patches that we will be utilizing. These parameters can be found or added in your config.plist under DeviceProperties/Add/PciRoot\(0x0\)/Pci\(0x02,0x00\).

| Key | Function |
| :--- | :--- |
| AAPL,ig-platform-id | This is the platform identifier of the GPU you are spoofing.  \(required\) |
| device-id | This is the device identifier of the GPU you are spoofing. \(required in cases) |
| framebuffer-patch-enable | This switch enables framebuffer patching.  It is required when setting framebuffer patches such as fbmem and stolenmem. |
| framebuffer-fbmem | This patches framebuffer memory, and is used when you cannot configure DVMT to 64MB in the BIOS.  _Do not use if the DVMT BIOS option is available._ |
| framebuffer-stolenmem | This patches framebuffer stolen memory, and is used when you cannot configure DVMT to 64MB in the BIOS.  _Do not use if the DVMT BIOS option is available._ |

These parameters contain the basics to get you started. If you need more advanced patching to enable your framebuffer, review the [Whatevergreen Intel GPU FAQ](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md). The Framebuffer patches are included in the config.plist provided with this guide, however they are disabled. To enable them remove the \# symbol from the beginning of the key.

### iGPU Patches

Now you are ready to configure your patches. Use the table below to select the patch that most closely resembles the configuration from Ark.

{% hint style="info" %}

`*` Denotes the default configured by WhateverGreen.

**thicc text** Denotes recommended values (taken from WhateverGreen and Rehabman laptop configs)

`Intel <Gen> (10.x.x)` Denotes the Generation of the Processor and the supported macOS versions.

{% endhint %}

### Intel Ivy Bridge (10.8+)

| iGPU | device-id | AAPL,ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel HD Graphics 4000 | 66010001 | 01006601 | 4 | 96MB | 24MB | 1536MB | LVDS1 HDMI1 DP2 |
| Intel HD Graphics 4000 | 66010002 | 02006601 | 1 | 64MB | 24MB | 1536MB | LVDS1 |
| **Intel HD Graphics 4000 <sup>1</sup>** * | 66010003 | 03006601 | 4 | 64MB | 16MB | 1536MB | LVDS1 DP3 |
| **Intel HD Graphics 4000 <sup>2</sup>** | 66010004 | 04006601 | 1 | 32MB | 16MB | 1536MB | LVDS1 |
| Intel HD Graphics 4000 | 66010008 | 08006601 | 3 | 64MB | 16MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 4000 <sup>3</sup>** | 66010009 | 09006601 | 3 | 64MB | 16MB | 1536MB | LVDS1 DP2 |

#### Special Notes:

* For these cards, no `device-id` property is required.

* <sup>1</sup> : to be used with 1366 by 768 displays or lower (main)

* <sup>2</sup> : to be used with 1600 by 900 displays or higher (main)

* <sup>3</sup> : to be used with some devices that have `eDP` connected monitor (contrary to classical LVDS), must be tested with <sup>1</sup> and <sup>2</sup> first then try this.

* VGA is *not* supported (unless it's running through a DP to VGA internal adapter, which apparently only rare devices will see it as DP and not VGA, it's all about luck.)

* For `04006601` platform, as you can tell, it has only one output, which is not enough for external connectors (HDMI/DP), you may need to add these extra parameters (credit to Rehabman)

  | Key | Type | Value | Explanation |
  | :--- | :--- | :--- | :--- |
  | `framebuffer-patch-enable` | Number | `1`                                                          | *enabling the semantic patches in principle* (from WEG manual) |
  | `framebuffer-memorycount`  | Number | `2`                                                          | Matching FBMemoryCount to the one on `03006601` (1 on `04` vs 2 on `03`) |
  | `framebuffer-pipecount`    | Number | `2`                                                          | Matching PipeCount to the one on `03006601` (3 on `04` vs 2 on `03`) |
  | `framebuffer-portcount`    | Number | `4`                                                          | Matching PortCount to the one on `03006601` (1 on `04` vs 4 on `03`) |
  | `framebuffer-stolenmem`    | Data   | `00000004`                                                   | Matching STOLEN memory to 64MB (0x04000000 from hex to base 10 in Bytes) to the one on `03006601`<br />Check [here](https://www.tonymacx86.com/threads/guide-alternative-to-the-minstolensize-patch-with-32mb-dvmt-prealloc.221506/) for more information. |
  | `framebuffer-con1-enable`  | Number | `1`                                                          | This will enable patching on *connector1* of the driver. (Which is the second connector after con0, which is the eDP/LVDS one) |
  | `framebuffer-con1-alldata` | Data   | `02050000 00040000 07040000 03040000 00040000 81000000 04060000 00040000 81000000` | When using `all data` with a connector, either you give all information of that connector (port-bused-type-flag) or that port and the ones following it, like in this case.<br />In this case, the ports in `04` are limited to `1`:<br />`05030000 02000000 30020000` (which corresponds to port 5, which is LVDS)<br />However on `03` there are 3 extra ports:<br />`05030000 02000000 30000000` (LVDS, con0, like `04`)<br/>`02050000 00040000 07040000` (DP, con1)<br/>`03040000 00040000 81000000` (DP, con2)<br/>`04060000 00040000 81000000` (DP, con3)<br />Since we changed the number of PortCount to `4` in a platform that has only 1, that means we need to define the 3 others (and we that starting with con1 to the end).<br /> |

* Some laptops from this era came with a mixed chipset setup, using Ivy Bridge CPUs with Sandy Bridge chipsets which creates issues with macOS since it expects a certain IMEI ID that it doesn't find and would get stuck at boot, to fix this we need to fake the IMEI's IDs in these models

  * To know if you're affected check if your CPU is an intel Core ix-3xxx and your chipset is Hx6x (for example a laptop with HM65 or HM67 with a Core i3-3110M)
  * In your config add a new PciRoot device named `PciRoot(0x0)/Pci(0x16,0x0)`
    * Key: `device-id`
    * Type: Data
    * Value: `3A1E0000`

### Intel Haswell (10.9+)

| iGPU | device-id | AAPL,ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel HD Graphics 4400 | 160a000c | 0c00160a | 3 | 64MB | 34MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 5000 <sup>1</sup>** | 260a0005 | 0500260a | 3 | 32MB | 19MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 5000 <sup>2</sup>** | 260a0006 | 0600260a | 3 | 32MB | 19MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 5100 | 2e0a0008 | 08002e0a | 3 | 64MB | 34MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 5200 | 260d0007 | 0700260d | 4 | 64MB | 34MB | 1536MB | LVDS1 DP2 HDMI1 |
| Intel Iris Pro Graphics 5200 | 260d0009 | 0900260d | 1 | 64MB | 34MB | 1536MB | LVDS1 |
| Intel Iris Pro Graphics 5200 | 260d000e | 0e00260d | 4 | 96MB | 34MB | 1536MB | LVDS1 DP2 HDMI1 |
| Intel Iris Pro Graphics 5200 | 260d000f | 0f00260d | 1 | 96MB | 34MB | 1536MB | LVDS1 |

#### Special Notes:

* <sup>1</sup>: to be used usually with HD5000, HD5100 and HD5200
  * The device-id of these devices *should* be supported already by the native macOS drivers.
* <sup>2</sup>: to be used usually with HD4200, HD4400 and HD4600.
  * You **must** use `device-id` = `12040000`
* In some cases, just using these values directly would cause some glitches to show up, to mitigate them, we change the size of the cursor byte:
  * `framebuffer-patch-enable` = `1` (as a Number)
  * `framebuffer-cursor` = `00009000` (as Data)
    * We change the cursor byte from 6MB (00006000) to 9MB because of some glitches.


### Intel Broadwell (10.10.2+)

| iGPU | device-id | AAPL,ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Unlisted iGPU | 06160002 | 02000616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Unlisted iGPU | 0e160001 | 01000e16 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 5600 | 12160003 | 03001216 | 4 | 34MB | 21MB | 1536MB | LVDS1 DP2 HDMI1 |
| Intel HD Graphics 5500 | 16160002 | 02001616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 5300 | 1e160001 | 01001e16 | 3 | 38MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 6200 | 22160002 | 02002216 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 6000 | 26160002 | 02002616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 6000 | 26160005 | 05002616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 6000** * | 26160006 | 06002616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 6100 | 2b160002 | 02002b16 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |

#### Sepcial Notes:

* For HD5300, HD5500 and HD6000, you do not have to specify any `device-id`

* For HD5600 you need `device-id` faked to `26160000`

* In some cases where you cannot set the DVMT-prealloc of these cards to 96MB higher in your UEFI Setup, you may get a kernel panic. Usually they're configured for 32MB of DVMT-prealloc, in that case these values are added to your iGPU Properties
 
  | Key | Type | Value |
  | :--- | :--- | :--- |
  | `framebuffer-patch-enable` | Number | `1` |
  | `framebuffer-stolenmem` | Data | `00003001` |
  | `framebuffer-fbmem` | Data | `00009000` |

### Intel Skylake 

| iGPU | device-id | AAPL,ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel HD Graphics 530 | 12190000 | 00001219 | 3 | 34MB | 21MB | 1536MB | DUMMY1 DP2 |
| **Intel HD Graphics 520** | 16190000 | 00001619 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 520 | 16190002 | 02001619 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 530** * | 1b190000 | 00001b19 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 530 | 1b190006 | 06001b19 | 1 | 38MB | 0MB | 1536MB | LVDS1 |
| Intel HD Graphics 515 | 1e190000 | 00001e19 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 515 | 1e190003 | 03001e19 | 3 | 40MB | 0MB | 1536MB | LVDS1 DP2 |
| **Intel Iris Graphics 540** | 26190000 | 00002619 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 540 | 26190002 | 02002619 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 540 | 26190004 | 04002619 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 540 | 26190007 | 07002619 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 550 | 27190000 | 00002719 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 550 | 27190004 | 04002719 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 580 | 3b190000 | 00003b19 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 580 | 3b190005 | 05003b19 | 4 | 34MB | 21MB | 1536MB | LVDS1 DP3 |

#### Special Notes:

* For HD515, HD520, HD530 and HD540, you do not need to use `device-id` faking, they're natively recognised.
  * I would recommend you keep the `AAPL,ig-platform-id` automatically recognised for each device-id by commenting/removing its entry in the config, otherwise it is recommended to choose `00001619`.
* For HD510 you may need to use `device-id`=`02190000` to fake its device-id.
  * You would need also to use `AAPL,ig-platform-id`=`00001B19` or `00001619`
* For HD550 and P530 (and potentially all HD P-series iGPUs), you may need to use `device-id`=`16190000`(recommended) or `12190000` or `26190000` or `1b190000`
  * The choice of device-id may help with usable screen on boot up and on wake.
    * For example Lenovo ThinkPad P50 with Xeon CPU will only properly work with `1619`.
    * For example Dell Precision 7710 with i7 CPU has issues when set to `1619`, using `1b19` or something else may help.
    * It is also recommended using `2619` with Xeon iGPUs.
  * You may also pair it with a proper `AAPL,ig-platform-id`=`00001619`(recommended) or `00001219` or `00002619` or `00001b19`

* In some cases where you cannot set the DVMT-prealloc of these cards to 64MB higher in your UEFI Setup, you may get a kernel panic. Usually they're configured for 32MB of DVMT-prealloc, in that case these values are added to your iGPU Properties
 
  | Key | Type | Value |
  | :--- | :--- | :--- |
  | `framebuffer-patch-enable` | Number | `1` |
  | `framebuffer-stolenmem` | Data | `00003001` |
  | `framebuffer-fbmem` | Data | `00009000` |


### Intel Kaby Lake, KBL-R, & Amber Lake

| iGPU | device-id | AAPL,ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Intel HD Graphics 620** | 16590000 | 00001659 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 620 | 16590009 | 09001659 | 3 | 38MB | 0MB | 1536MB | LVDS1 DP2 |
| Unlisted iGPU | 18590002 | 02001859 | 0 | 0MB | 0MB | 1536MB | Connector: |
| **Intel HD Graphics 630** | 1b590000 | 00001b59 | 3 | 38MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 630 | 1b590006 | 06001b59 | 1 | 38MB | 0MB | 1536MB | LVDS1 |
| Unlisted iGPU | 1c590005 | 05001c59 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 615 | 1e590000 | 00001e59 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 615 | 1e590001 | 01001e59 | 3 | 38MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 640 | 26590002 | 02002659 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 650 | 27590004 | 04002759 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 650 | 27590009 | 09002759 | 3 | 38MB | 0MB | 1536MB | LVDS1 DP2 |
| **Intel UHD Graphics 617** | C0870000 | 0000C087 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 617 | C0870005 | 0500C087 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |

#### Special Notes:

* For `HD615`, `HD620`, `HD630`, `HD640` and `HD650` it is not needed to use a `device-id`, however due to many issues with different setups it is recommended to use:
  * `device-id`=`1b590000` or `16590000`
  * `AAPL,ig-platform-id`=`00001659` or `00001b59` (you can try whichever works the best, some even try to cross the device-id and the ig-platform-id)
    * For HD620 users, they can skip the part above (unless you get issues)
* For `UHD620` users, you **must** use:
  * `device-id`=`87C00000`
  * `AAPL,ig-platform-id`=`0000C087`
    * **Note:** `UHD630` ***IS NOT*** KabyLake, it's CoffeeLake (check next section).
* For all HD6\*\* (`UHD` users are not concerned), there are some small issues with output where plugging anything would cause a lock up (kernel panic), here are some patches to mitigate that (credit Rehabman):
  * 0306 to 0105 (will probably explain what it does one day)

  | Key | Type | Value |
  | :--- | :--- | :--- |
  | `framebuffer-con1-enable` | Number | `1` |
  | `framebuffer-con1-alldata` | Data | `01050A00 00080000 87010000 02040A00 00080000 87010000 FF000000 01000000 20000000` |

  * 0204 to 0105 (will probably explain what it does one day)

  | Key | Type | Value |
  | :--- | :--- | :--- |
  | `framebuffer-con1-enable` | Number | `1` |
  | `framebuffer-con1-alldata` | Data | `01050A00 00080000 87010000 03060A00 00040000 87010000 FF000000 01000000 20000000` |

* In some cases where you cannot set the DVMT-prealloc of these cards to 64MB higher in your UEFI Setup, you may get a kernel panic. Usually they're configured for 32MB of DVMT-prealloc, in that case these values are added to your iGPU Properties
 
  | Key | Type | Value |
  | :--- | :--- | :--- |
  | `framebuffer-patch-enable` | Number | `1` |
  | `framebuffer-stolenmem` | Data | `00003001` |
  | `framebuffer-fbmem` | Data | `00009000` |

### Intel Coffee Lake, Comet Lake (14nm++++++)

| iGPU | device-id | AAPL,ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel UHD Graphics 630 | 003E0000 | 0000003E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | 923E0000 | 0000923E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | 923E0009 | 0900923E | 3 | 57MB | 0MB | 1536MB | LVDS1 DUMMY2 |
| **Intel UHD Graphics 630** | 9B3E0000 | 00009B3E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | 9B3E0006 | 06009B3E | 1 | 38MB | 0MB | 1536MB | LVDS1 DUMMY2 |
| Intel UHD Graphics 630 | 9B3E0009 | 09009B3E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 655 | A53E0000 | 0000A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 655 | A53E0004 | 0400A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | A53E0005 | 0500A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 655 | A53E0009 | 0900A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Unlisted iGPU | A63E0005 | 0500A63E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |

#### Special Notes:

* For `UHD630` you may not need to fake the `device-id`  as long as it's `8086:9B3E`, if it's anything else, you may use `device-id`=`9B3E0000`
* For `UHD620` in a Comet Lake CPU **requires**:
  * `device-id`=`9B3E0000`
  * `AAPL,ig-platform-id`=`00009B3E`

### Intel IceLake (*soon™*)

*to be filled*

## Panel Backlight

Whatevergreen will enable your panel backlight, but to do so you usually have to provide configuration. Because this is OpenCore, the only method we can use is adding the SSDT-PNLF.

We'll do that by compiling PNLF SSDT from the Whatevergreen source repository and placing it in OpenCore/ACPI/patched. First, save the dsl to your home directory.

[SSDT-PNLF.dsl @ Github](https://raw.githubusercontent.com/acidanthera/WhateverGreen/master/Manual/SSDT-PNLF.dsl)

[SSDT-PNLFCFL.dsl @ AppleLife.ru](https://cdn.discordapp.com/attachments/573338411503714324/690377007040561162/SSDT-PNLFCFL.dsl)

{% hint style="info" %}
Use the SSDT-PNLF.dsl with any iGPU Generation. Use SSDT-PNLFCFL.dsl with CoffeeLake (and potentially later releases). DO NOT USE BOTH AT THE SAME TIME.
{% endhint %}

Now that you've saved the file, you'll need to compile it. For that, we need to download maciASL if you are on macOS, or iASL if you are on linux or windows.

[maciASL Project @ Github](https://github.com/acidanthera/MaciASL)

Run maciASL, and open the SSDT-PNLF.dsl that you created previously. Save the file to OpenCore/ACPI/patched/SSDT-PNLF.aml. If you are using windows or linux, run `path/to/iasl SSDT-PNLF.dsl`.

#### Special note for CoffeeLake and later:

You might also want to try adding these boot arguments / device properties (recommended) to your configuration:

* `igfxcflbklt=1` or `enable-cfl-backlight-fix`=`1`(Number) to enable CFL backlight patch
* `-igfxmlr` or `enable-dpcd-max-link-rate-fix`=`1`(Number) to apply the maximum link rate fix (for some high resolution laptop screens like on XPS15 and others)

## Verifying Metal Support

This one's easy, just click Apple &gt; About This Mac &gt; System Report, and click Graphics/Displays. You should see a line that looks like this.

```text
  Metal:    Supported, feature set macOS GPUFamily2 v1
```

If you don't see it, you may have some additional work to do.

## Verifying HEVC Encoding

If you're using a system configured to act like a 2015 or earlier Macbook/Macbook Pro, stop here because it's not supported. Otherwise you can verify that it's working after macOS is installed by installing VideoProc \(Free version is fine\). VideoProc can be used to test H264 encoding and decoding as it should be supported even if HEVC isn't.

[Download VideoProc](https://www.videoproc.com)

Once you are in the application, click Settings \(Bottom right\) followed by the Options button next to Hardware Acceleration engine. If everything is working properly all of the boxes should be checked.

![](../.gitbook/assets/screen-shot-2019-08-23-at-8.15.58-pm.png)

All set? Great!

