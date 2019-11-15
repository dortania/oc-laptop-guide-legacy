# Display Configuration

Configuring a laptop's integrated GPU is a lot like configuring a desktop's integrated GPU.  You usually start by disabling the dedicated GPU if you have one.  This section will talk about the methods that you would use for each component of GPU configuration and how to test that it's working, but there are better guides that already exist to help with the heavy lifting.

### First things first, disable that dGPU!

There are two ways to disable a dGPU in a laptop, the short way and the not so short way.  We'll start with the short way because it is really simple.  If you don't have a dGPU \(NVidia/ATI dedicated graphics\) you can skip this part and jump straight to configuring your display adapter instead.

To disable your dGPU the short way, add the following to the Boot/Arguments section of your config.plist.

```text
-wegnoegpu
```

This command instructs Whatevergreen to disable all internal and external dedicated GPUs.  Don't reboot yet, because we still need to get Whatevergreen!

Mission complete?  Great!  If it doesn't work out the way you would expect, come back and move on the the not so short method using Hackintosh Slav's wonderful guide.

[How to disable your unsupported GPU for MacOS](https://khronokernel-4.gitbook.io/disable-unsupported-gpus/)

Got it all fixed up?  Excellent!  Now that this is behind us, it's time to configure your iGPU.

### Configuring your Display Adapter \(GPU\)

Depending on your laptop, you could have very little to do to configure your iGPU, or you could have to add an elaborate set of patches to configure stuff like DVMT that we talked about earlier.  The most important thing you'll want here is the WhateverGreen kext and it's dependency, LILU.  Lilu is a patching mechanism that's used by multiple kernel extensions, and WhateverGreen is responsible for patching your display adapter\(s\).

If you haven't already added WhateverGreen to your CLOVER config, better do that now before continuing.  You can get it here.

[Download WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)

By the way, this will also take care of your backlight control.  Yep, you even have to think about that.

Now to complete your quest to find the great working GPU, adventurer! It's dangerous to go alone, take this!  Headkaze's excellent guide to Intel Framebuffer patching!  If you're using a Rehabman template from the previous page, this guide will still be helpful if you have to update any of the GPU properties of the template.

[\[Guide\] Intel Framebuffer patching using WhateverGreen](https://www.tonymacx86.com/threads/guide-intel-framebuffer-patching-using-whatevergreen.256490/)

Back so soon?  I know, short read.. Now that you think you have everything working, here's how you can verify that you have Metal support as well as the ability to encode h264 in hardware.

#### Panel Backlight

Whatevergreen will enable your panel backlight, but to do so you usually have to provide configuration.  There are two methods supported by Whatevergreen.  Try one and if it doesn't work or your brightness keys are not working, disable it and try the other.

#### Method 1 - AddPNLF

This method is the simplest, and only requires a plist editor.  Use the table below to add/enable AddPNLF, SetIntelBacklight, and SetIntelMaxBacklight to your config.plist.

| Path | Property | Type | Value |
| :--- | :--- | :--- | :--- |
| ACPI/DSDT/Fixes/ | AddPNLF | Bool | True |
| Devices/ | SetIntelBacklight | Bool | True |
| Devices/ | SetIntelMaxBacklight | Bool | True |

#### Method 2 - SSDT-PNLF

We'll do that by compiling PNLF SSDT from the Whatevergreen source repository and placing it in CLOVER/ACPI/patched.  First, save the dsl to your home directory.

\*\*\*\*[SSDT-PNLF.dsl @ Github](https://raw.githubusercontent.com/acidanthera/WhateverGreen/master/Manual/SSDT-PNLF.dsl)

Now that you've saved the file, you'll need to compile it.  For that, we need to download maciASL.

[maciASL Project @ Github](https://github.com/acidanthera/MaciASL)

Run maciASL, and open the SSDT-PNLF.dsl that you created previously.  Save the file to CLOVER/ACPI/patched/SSDT-PNLF.aml

Before rebooting, review your config.plist and make sure patch GFX0 to IGPU and AddPNLF are disabled if they exist.

#### Verifying Metal Support

This one's easy, just click Apple &gt; About This Mac &gt; System Report, and click Graphics/Displays.  You should see a line that looks like this.

```text
  Metal:	Supported, feature set macOS GPUFamily2 v1
```

If you don't see it, you may have some additional work to do.

#### Verifying HEVC Encoding

If you're using a system configured to act like a 2015 or earlier Macbook/Macbook Pro, stop here because it's not supported.  Otherwise you can verify that it's working after macOS is installed by installing VideoProc \(Free version is fine\).  VideoProc can be used to test H264 encoding and decoding as it should be supported even if HEVC isn't.

[Download VideoProc](https://www.videoproc.com)

Once you are in the application, click Settings \(Bottom right\) followed by the Options button next to Hardware Acceleration engine.   If everything is working properly all of the boxes should be checked.

![](.gitbook/assets/screen-shot-2019-08-23-at-8.15.58-pm.png)

All set?  Great!

