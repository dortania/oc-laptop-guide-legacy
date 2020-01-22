# Locate that Missing Battery

First thing I have to ask is this. Is it actually missing? Often just installing VirtualSMC is all that it takes for your battery status to appear. If it's there, there's nothing to see here, and it's safe to move onto the next section!

Oh, you're still here? Well...unfortunately that means this is going to be one of our more painful endeavors as it requires DSDT patches. Fortunately, you've already studied applying hotpatches, so there's no need to cover it again. Let's jump right into the basics to get you started.

## DSDT Battery Status Patching

First of all, you're going to need to dust off maciASL. Remember that? You used it to compile the SSDT patches for your touchpad. This is going to be a little different. Here's the 101.

### Dumping your DSDT

The first step in getting ready to patch your DSDT is to get a copy of it from your firmware. You can do this by rebooting your computer and pressing F4 at the CLOVER menu, before launching macOS. That instructs CLOVER to drop a copy of it in your EFI partition under CLOVER/ACPI/origin.

### Patching the DSDT

This is a little more advanced, so after getting you started you're going to have to study some documentation and really understand what it is that you're doing as the DSDT is quite fragile. Open maciASL, then open your DSDT file. You should be presented with quite an array of data about your system. If you get an error right away, that means you'll want to go into preferences and change your iASL and then close and re-open the DSDT. To do that, open Preferences of maciASL, click iASL and select Legacy. If Legacy is already selected change it to Stable. You'll also want to click the Update iASL button before closing.

Alright, now that you're in maciASL and you have your DSDT open comes the hard part. Finding patches that work for your system. In maciASL you'll notice that at the top center of the main window are two options; Compile, and Patch.

Click Patch and in the left pane of the window that opens you should find a whole bunch of patch repositories already configured for you. Find Rehabman in the tree. Under this section you will find a large selection of patches for various laptops. After making a backup of your DSDT, try applying the patches for systems that are most similar to yours. You might get lucky and have no more work to do.

If those don't work, you're going to have to get personal with Rehabman's battery patching guide and your DSDT.

[\[Guide\] How to patch DSDT for working battery status](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/)

This one could take a while, but don't worry..the next section will be here waiting for your click!

