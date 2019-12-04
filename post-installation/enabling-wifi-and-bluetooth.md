# Enabling WIFI & Bluetooth

I get it, you want your Hackintosh to be able to access the internet and connect to your Bluetooth headphones. That's a noble goal, so let's get to work! Enabling WIFI & Bluetooth is pretty straight forward, but if you haven't swapped the card out for one of the supported devices that we talked about in the Know Your Hardware section, you're in for a bad time. Stop now and do that first. Remember, there are only a few compatible devices, and you really don't want to try to force the use of an unsupported device because it's cheaper, you'll just be throwing your money away. Before purchasing make sure the adapter matches the socket you'll be installing it into.

## Supported WIFI/Bluetooth Combo Cards

### **Mini PCIe Adapters**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Chipset</th>
      <th style="text-align:left">Antenna</th>
      <th style="text-align:left">Models</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">BCM94352HMB</td>
      <td style="text-align:left">U.FL</td>
      <td style="text-align:left">
        <p><a href="https://web.archive.org/web/20191003003404/https://wikidevi.com/wiki/AzureWave_AW-CE123H">AzureWave AW-CE123H</a>
        </p>
        <p><a href="https://web.archive.org/web/20191003014022/https://wikidevi.com/wiki/Dell_Wireless_1550_(DW1550)">Dell DW1550</a>
        </p>
        <p><a href="https://web.archive.org/web/20191002211005/https://wikidevi.com/wiki/HP_TPC-Q013">HP TPC-Q013</a>
        </p>
        <p><a href="https://web.archive.org/web/20191003063651/https://wikidevi.com/wiki/Lite-On_WCBN606BH_(Lenovo)">Lenovo WCBN606BH</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BCM94360HMB</td>
      <td style="text-align:left">MHF4</td>
      <td style="text-align:left"><a href="https://web.archive.org/web/20191020204026/https://wikidevi.com/wiki/AzureWave_AW-CB160H">AzureWave AW-CB160H</a>
      </td>
    </tr>
  </tbody>
</table>### PCIe m.2 Adapters

<table>
  <thead>
    <tr>
      <th style="text-align:left">Chipset</th>
      <th style="text-align:left">Antenna</th>
      <th style="text-align:left">Models</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">BCM94352Z</td>
      <td style="text-align:left">MHF4</td>
      <td style="text-align:left">
        <p><a href="https://web.archive.org/web/20191003014024/https://wikidevi.com/wiki/Dell_Wireless_1560_(DW1560)">Dell DW1560</a> (A/E
          key)</p>
        <p><a href="https://web.archive.org/web/20191003030111/https://wikidevi.com/wiki/Broadcom_BCM94352Z">Lenovo 04X6020</a> (E
          key)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BCM943602BAED</td>
      <td style="text-align:left">MHF4</td>
      <td style="text-align:left"><a href="https://web.archive.org/web/20191020204443/https://wikidevi.com/wiki/Dell_Wireless_1830_(DW1830)">Dell DW1830</a> (A/E
        key)</td>
    </tr>
  </tbody>
</table>_Note: The while the DW1820A is compatible with many Hackintosh desktops, they are generally incompatible with Hackintosh laptops._

### Apple Native WIFI Cards

Wireless cards manufactured by Apple are natively supported, however they use a proprietary 12+6 connector which requires an adapter to function in an mPCIe or m.2 socket. Before purchasing ensure your laptop has sufficient space for the card + adapter, and make sure to purchase the appropriate 12+6 to mPCIE or m.2 adapter.

| Chipset | Antenna |
| :--- | :--- |
| [BCM94360CS2](https://web.archive.org/web/20191003030122/https://wikidevi.com/wiki/Broadcom_BCM94360CS2) | MHF4 |
| [BCM943224PCIEBT2](https://web.archive.org/web/20191002215550/https://wikidevi.com/wiki/Broadcom_BCM943224PCIEBT2) | MHF4 |
| [BCM94360CD](https://web.archive.org/web/20191020204211/https://wikidevi.com/wiki/Broadcom_BCM94360CD) | M.FL |

_Note: Be sure to use an adapter that is suited for your WLAN socket and only install to the WLAN socket. Other m.2 sockets may not have all of the paths needed for WIFI and Bluetooth to function. WWAN sockets may be USB or not available without modded BIOS._

## Configuring WIFI

WIFI configuration is pretty straightforward, you only need an injector kext to help macOS see and configure the card. For that we'll need to download the AirportBrcmFixup kext from its project page at Github. Add the kext to C/k/O and reboot when convenient.

[Download AirportBrcmFixup @ Github](https://github.com/acidanthera/AirportBrcmFixup)

The location of the kext should match the tree below.

```text
EFI
└── CLOVER
    └── kexts
        └── Other
            └── AirportBrcmFixup.kext
```

## Setting up Bluetooth

Internal Bluetooth devices are almost as easy, but they do require being configured as internal USB devices. If you haven't mapped your USB ports yet, do that first. Unless you're using a native Apple card you will need to download BrcmPatchRam from the Acidanthera repo. Within this package you will find several kexts. These kexts are detailed in the table below.

| Kext | Use |
| :--- | :--- |
| BrcmBluetoothInjector.kext | Use when firmware loading is not required. |
| BrcmPatchRAM.kext | Loads firmware into the bluetooth device and activates it.  For macOS 10.10 or earlier. |
| BrcmPatchRAM2.kext | Loads firmware into the bluetooth device and activates it.  For macOS 10.10-10.14. |
| BrcmPatchRAM3.kext | Loads firmware into the bluetooth device and activates it.  For macOS 10.15. |
| BrcmFirmwareData.kext | Provides firmware to BrcmPatchRAM2. |

Once you've determined the kexts that you'll need, add them to C/k/O and reboot. If you're unsure, start with BrcmBluetoothInjector.kext. If your Bluetooth device isn't working after a reboot, you need a firmwareloader. Add the appropriate BrcmPatchRAM?.kext along with BrcmFirmwareData.kext. Leave BrcmBluetoothInjector.kext as well as it is also required.

[Download BrcmPatchRam @ Github](https://github.com/acidanthera/BrcmPatchRAM)

When finished, the kexts should be located in your EFI as indicated in the tree below.

```text
EFI
└── CLOVER
    └── kexts
        └── Other
            ├── BrcmBluetoothInjector.kext
            ├── BrcmPatchRAM{2,3}.kext
            └── BrcmFirmwareData.kext
```

If you need additional help with installing and configuring WIFI and Bluetooth, you should read Toleda's Broadcom WiFi/Bluetooth guide which is linked below.

[Broadcom WiFi/Bluetooth \[Guide\]](https://www.tonymacx86.com/threads/broadcom-wifi-bluetooth-guide.242423/)

That wasn't so bad, right?

