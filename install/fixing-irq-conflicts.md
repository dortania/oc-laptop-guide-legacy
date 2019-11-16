# Fixing IRQ Conflicts

The table below contains some common fixes that help to eliminate any potential IRQ conflicts that may be preventing your touchpad and audio device from working properly. Enable them in config.plist under ACPI/DSDT/Fixes.

| Fix | Type | Parameter | Description |
| :--- | :--- | :--- | :--- |
| FixIPIC | Bool | YES | Deletes IRQ from IPIC device. |
| FixTMR | Bool | YES | Excludes IRQ from TMR device. |
| FixRTC | Bool | YES | Excludes IRQ from RTC device. |
| FixHPET | Bool | YES | Adds IRQ to HPET device. |

To learn more about these fixes, review the [Clover WIKI](https://sourceforge.net/p/cloverefiboot/wiki/ACPI/).

