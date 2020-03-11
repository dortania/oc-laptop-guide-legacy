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

{% endhint %}

### Intel Ivy Bridge

| iGPU | device-id | ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel HD Graphics 4000 | 01660001 | 01006601 | 4 | 96MB | 24MB | 1536MB | LVDS1 HDMI1 DP2 |
| Intel HD Graphics 4000 | 01660002 | 02006601 | 1 | 64MB | 24MB | 1536MB | LVDS1 |
| **Intel HD Graphics 4000<sup>1</sup>** * | 01660003 | 03006601 | 4 | 64MB | 16MB | 1536MB | LVDS1 DP3 |
| **Intel HD Graphics 4000<sup>2</sup>** | 01660004 | 04006601 | 1 | 32MB | 16MB | 1536MB | LVDS1 |
| Intel HD Graphics 4000 | 01660008 | 08006601 | 3 | 64MB | 16MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 4000<sup>3</sup>** | 01660009 | 09006601 | 3 | 64MB | 16MB | 1536MB | LVDS1 DP2 |

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

### Intel Haswell

| iGPU | device-id | ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel HD Graphics 4400 | 0a16000c | 0c00160a | 3 | 64MB | 34MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 5000<sup>1</sup>** | 0a260005 | 0500260a | 3 | 32MB | 19MB | 1536MB | LVDS1 DP2 |
| **Intel HD Graphics 5000<sup>2</sup>** | 0a260006 | 0600260a | 3 | 32MB | 19MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 5100 | 0a2e0008 | 08002e0a | 3 | 64MB | 34MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 5200 | 0d260007 | 0700260d | 4 | 64MB | 34MB | 1536MB | LVDS1 DP2 HDMI1 |
| Intel Iris Pro Graphics 5200 | 0d260009 | 0900260d | 1 | 64MB | 34MB | 1536MB | LVDS1 |
| Intel Iris Pro Graphics 5200 | 0d26000e | 0e00260d | 4 | 96MB | 34MB | 1536MB | LVDS1 DP2 HDMI1 |
| Intel Iris Pro Graphics 5200 | 0d26000f | 0f00260d | 1 | 96MB | 34MB | 1536MB | LVDS1 |

#### Special Notes:

* <sup>1</sup>: to be used usually with HD5000, HD5100 and HD5200
  * The device-id of these devices *should* be supported already by the native macOS drivers.
* <sup>2</sup>: to be used usually with HD4200, HD4400 and HD4600.
  * You **must** use `device-id` = `12040000`
* In some cases, just using these values directly would cause some glitches to show up, to mitigate them, we change the size of the cursor byte:
  * `framebuffer-patch-enable` = `1` (as a Number)
  * `framebuffer-cursor` = `00009000` (as Data)
    * We change the cursor byte from 6MB (00006000) to 9MB because of some glitches.


### Intel Broadwell

| iGPU | device-id | ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Unlisted iGPU | 16060002 | 02000616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Unlisted iGPU | 160e0001 | 01000e16 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 5600 | 16120003 | 03001216 | 4 | 34MB | 21MB | 1536MB | LVDS1 DP2 HDMI1 |
| Intel HD Graphics 5500 | 16160002 | 02001616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 5300 | 161e0001 | 01001e16 | 3 | 38MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 6200 | 16220002 | 02002216 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 6000 | 16260002 | 02002616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 6000 | 16260005 | 05002616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 6000\* | 16260006 | 06002616 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 6100 | 162b0002 | 02002b16 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |

### Intel Skylake

| iGPU | device-id | ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel HD Graphics 530 | 19120000 | 00001219 | 3 | 34MB | 21MB | 1536MB | DUMMY1 DP2 |
| Intel HD Graphics 520\* | 19160000 | 00001619 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 520 | 19160002 | 02001619 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 530 | 191b0000 | 00001b19 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 530 | 191b0006 | 06001b19 | 1 | 38MB | 0MB | 1536MB | LVDS1 |
| Intel HD Graphics 515 | 191e0000 | 00001e19 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 515 | 191e0003 | 03001e19 | 3 | 40MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 540 | 19260000 | 00002619 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 540 | 19260002 | 02002619 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 540 | 19260004 | 04002619 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 540 | 19260007 | 07002619 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 550 | 19270000 | 00002719 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Graphics 550 | 19270004 | 04002719 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 580 | 193b0000 | 00003b19 | 3 | 34MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel Iris Pro Graphics 580 | 193b0005 | 05003b19 | 4 | 34MB | 21MB | 1536MB | LVDS1 DP3 |

### Intel Kaby Lake, KBL-R, & Amber Lake

| iGPU | device-id | ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel HD Graphics 620 | 59160000 | 00001659 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 620 | 59160009 | 09001659 | 3 | 38MB | 0MB | 1536MB | LVDS1 DP2 |
| Unlisted iGPU | 59180002 | 02001859 | 0 | 0MB | 0MB | 1536MB | Connector: |
| Intel HD Graphics 630\* | 591b0000 | 00001b59 | 3 | 38MB | 21MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 630 | 591b0006 | 06001b59 | 1 | 38MB | 0MB | 1536MB | LVDS1 |
| Unlisted iGPU | 591c0005 | 05001c59 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 615 | 591e0000 | 00001e59 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel HD Graphics 615 | 591e0001 | 01001e59 | 3 | 38MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 640 | 59260002 | 02002659 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 650 | 59270004 | 04002759 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 650 | 59270009 | 09002759 | 3 | 38MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 617\*\* | 87C00000 | 0000C087 | 3 | 34MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 617 | 87C00005 | 0500C087 | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |

### Intel Coffee Lake

| iGPU | device-id | ig-platform-id | Port Count | Stolen Memory | Framebuffer Memory | Video RAM | Connectors |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Intel UHD Graphics 630 | 3E000000 | 0000003E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | 3E920000 | 0000923E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | 3E920009 | 0900923E | 3 | 57MB | 0MB | 1536MB | LVDS1 DUMMY2 |
| Intel UHD Graphics 630 | 3E9B0000 | 00009B3E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | 3E9B0006 | 06009B3E | 1 | 38MB | 0MB | 1536MB | LVDS1 DUMMY2 |
| Intel UHD Graphics 630 | 3E9B0009 | 09009B3E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 655 | 3EA50000 | 0000A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 655 | 3EA50004 | 0400A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel UHD Graphics 630 | 3EA50005 | 0500A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Intel Iris Plus Graphics 655\* | 3EA50009 | 0900A53E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |
| Unlisted iGPU | 3EA60005 | 0500A63E | 3 | 57MB | 0MB | 1536MB | LVDS1 DP2 |

## Panel Backlight

Whatevergreen will enable your panel backlight, but to do so you usually have to provide configuration. Because this is OpenCore, the only method we can use is adding the SSDT-PNLF.

We'll do that by compiling PNLF SSDT from the Whatevergreen source repository and placing it in OpenCore/ACPI/patched. First, save the dsl to your home directory.

[SSDT-PNLF.dsl @ Github](https://raw.githubusercontent.com/acidanthera/WhateverGreen/master/Manual/SSDT-PNLF.dsl)

Now that you've saved the file, you'll need to compile it. For that, we need to download maciASL if you are on macOS, or iASL if you are on linux or windows.

[maciASL Project @ Github](https://github.com/acidanthera/MaciASL)

Run maciASL, and open the SSDT-PNLF.dsl that you created previously. Save the file to OpenCore/ACPI/patched/SSDT-PNLF.aml. If you are using windows or linux, run `path/to/iasl SSDT-PNLF.dsl`.

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

