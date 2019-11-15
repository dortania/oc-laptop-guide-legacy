# Setting up Ethernet Adapters

If your laptop has an Ethernet adapter, or you have a USB dongle that you'd like to use it's possible that it is compatible with macOS, but probably not by default.  If it's a supportable device, it should be as simple as adding the kext to C/k/O for macOS to see it.

## Internal Ethernet

The table below contains a list of kexts that may support your ethernet adapter.  Before adding them, research to make sure your device is supported! If it is, add the kext to C/k/O and reboot.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Vendor</th>
      <th style="text-align:left">Kext</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Atheros</td>
      <td style="text-align:left"><a href="https://github.com/Mieze/AtherosE2200Ethernet">AtherosE2200Ethernet</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Broadcom</td>
      <td style="text-align:left"><a href="https://github.com/chris1111/BCM5722D">BCM5722D</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Intel</td>
      <td style="text-align:left"><a href="https://github.com/Mieze/IntelMausiEthernet">IntelMausiEthernet</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Realtek</td>
      <td style="text-align:left">
        <p><a href="https://github.com/Mieze/RealtekRTL8100">RealtekRTL8100</a> (10/100
          Cards)</p>
        <p><a href="https://github.com/Mieze/RTL8111_driver_for_OS_X">RealtekRTL8111</a> (GbE
          Cards)</p>
      </td>
    </tr>
  </tbody>
</table>## External Ethernet \(USB\)

USB Ethernet adapters may work when connected, so you want to check Network Preferences, or System Information after plugging it in and see if it is detected automatically.  If it isn't, you may need to either install the support package for your device or extract the kext and add it to C/k/O.  The support page for your device should have an installable driver.  The table below lists USB Ethernet chipsets and where to find drivers.

| Vendor | Chipset & Driver |
| :--- | :--- |
| ASIX | [AX88179](https://www.asix.com.tw/download.php?sub=driverdetail&PItemID=131) |

### External \(USB\) Ethernet for Installation

If you need to add the driver to CLOVER to boot, you will need to extract the kext from the installation package after downloading.  Here's how you can do that.

* Install The Unarchiver \(App Store\)
* Open the package's DMG to mount it.
* Copy the installation package from the DMG to a temporary directory.
* Right click \(Control-Click\) the package and open it with The Unarchiver.
* Open a terminal and change to the package directory that has been created.
* Follow the steps below.

```text
$ cd {package}.pkg
$ tar -xvf Payload
```

* Copy the extracted kext to C/k/O

The USB adapter should be detected by macOS upon reboot into the installer.

* * * 


