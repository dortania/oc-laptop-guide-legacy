# macOS Memory Allocation

{% hint style="info" %}
This page contains info about FwRuntimeServices.efi, which is already placed into your EFI. You should read this, but there is nothing to do on this page.
{% endhint %}

The firmware of laptops \(and desktops\) allocates memory in small chunks to all of the various devices in your computer including an area for the operating system kernel to load into. This is fine for Windows as it expects this to be the case and it maps itself \(the Windows kernel\) into memory accordingly. MacOS however expects things to operate a little differently and have a larger chunk of memory to load into, and since non-Mac computers also tend to have more built in devices \(and their memory allocations\) we need a helper to provide macOS with a chunk of memory to use for the kernel. To solve the problem on Hackintoshes there are a variety of helpers to choose from. With OpenCore, we generally use FwRuntimeServices - the successor to AptioMemoryFix which is developed primarily for OpenCore.

## Memory Fix Indicators

When booting the macOS install USB or macOS you may see the following error message. This error message means there is not enough memory mapped to macOS to allow the OS to boot.

```text
ERROR!!! Load prelinked kernel with status 0x8000000000000009
```

You may also see messages mentioning KASLR, or slide. If you get a message stating "couldn't allocate pages" or are seeing messages about slide values and can't boot, you will want to visit this page which talks about how to calculate slide and reduce memory usage:

[https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/kalsr-fix](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/kalsr-fix)

## Other Memory Fix Helpers

The table below lists helpers that you may have found on the web while you were searching for a fix. These helpers are older and were written to solve a specific problem that has been rolled up into FwRuntimeServices. You generally do not use these unless FwRuntimeServices is giving you issues booting.

| Memory Fix |  |
| :--- | :--- |
| AptioMemoryFix | This helper is an alternative to OcQuirks, but is unsupported by the developer. |
| OsxAptioFix3Drv | This helper was deprecated by AptioMemoryFix. |
| OsxAptioFix2Drv | This helper was deprecated by AptioMemoryFix. |
| OsxAptioFixDrv | This helper was deprecated by AptioMemoryFix. |
| OsxAptioFix2Drv-**free2000** | This memory helper should never be used for any reason.  It was created to test a specific correction, and not to be used by end users.  It will corrupt your data, and may also cause hardware failure. |

