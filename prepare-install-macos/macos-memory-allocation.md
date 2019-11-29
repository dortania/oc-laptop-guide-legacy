# macOS Memory Allocation

The firmware of laptops \(and desktops\) allocates memory in small chunks to all of the various devices in your computer including an area for the operating system kernel to load into. This is fine for Windows as it expects this to be the case and it maps itself \(the Windows kernel\) into memory accordingly. MacOS however expects things to operate a little differently and have a larger chunk of memory to load into, and since non-Mac computers also tend to have more built in devices \(and their memory allocations\) we need a helper to provide macOS with a chunk of memory to use for the kernel. To solve the problem on Hackintoshes there are a variety of helpers to choose from. No one helper is better than another, they each simply apply a correction a bit differently than the other.

## Memory Fix Indicators

When booting the macOS install USB or macOS you may see the following error message. This error message means there is not enough memory mapped to macOS to allow the OS to boot.

```text
ERROR!!! Load prelinked kernel with status 0x8000000000000009
```

## OcQuirks

Helpers are EFI drivers, that are used by CLOVER to bootstrap macOS. The best supported and most stable helper is OcQuirks. Currently this helper won't be found in the upstream version of CLOVER, but it can be found on Github and can be downloaded from the repository linked below.

[ReddestDream's OcQuirks @ Github](https://github.com/ReddestDream/OcQuirks)

Go ahead and download OcQuirks, as we'll need it as we prepare the installation media in the next step.

## Other Memory Fix Helpers

The table below lists helpers that you may have found on the web while you were searching for a fix. These helpers are older and were written to solve a specific problem that has been rolled up into AptioMemoryFix. If you find a reference to them, use AptioMemoryFix instead.

| Memory Fix |  |
| :--- | :--- |
| AptioMemoryFix | This helper is an alternative to OcQuirks, but is unsupported by the developer. |
| OsxAptioFix3Drv | This helper was deprecated by AptioMemoryFix. |
| OsxAptioFix2Drv | This helper was deprecated by AptioMemoryFix. |
| OsxAptioFixDrv | This helper was deprecated by AptioMemoryFix. |
| OsxAptioFix2Drv-**free2000** | This memory helper should never be used for any reason.  It was created to test a specific correction, and not to be used by end users.  It will corrupt your data, and may also cause hardware failure. |

## Inserting the Memory Fix into the EFI

Now that you have your memory fix, add it to your CLOVER EFI folder. The layout should match the tree view below.

```text
EFI
└── CLOVER
    └── drivers
        └──UEFI
           ├──FwRuntimeServices.efi
           └──OcQuirks.efi
```

