# Optimizing Battery Life

You may notice that your laptop uses a lot of power, possibly more than it did when you were using Windows. That's because Apple is configuring your device to operate like an Apple device. Since you have different hardware, you have a few extra steps to take to optimize battery life. Below you will find a few methods to test power usage, and to adjust your system parameters to improve battery life.

## Measuring Power in macOS

macOS exposes overall power usage under the SPPowerDataType data type. You can find the data either using terminal, or using coconut battery. Here's a simple function that calculates watts on demand.

```text
watts() {
  FORMULA=$(system_profiler SPPowerDataType | awk '/Amperage\ \(mA\):/ {printf $3" * "}; /Voltage\ \(mV\):/ {print $3}')
  WATTS=$(echo "scale=3; ${FORMULA} / 1000000" | bc 2>/dev/null)
  if ! [[ ${WATTS} =~ [0-9] ]];
  then
    WATTS=0
  fi
  echo "${WATTS} mW"
}
```

Run it in a terminal to create a function called 'watts'. If you would like the command to be persistent, you should add it to your user profile \(~/.profile\). You'll see a negative number when you're discharging which is your discharge watts. When you're charging that number will be positive, that's the power draw of charging. If you see a negative number while charging, you're probably in the 97-100% range and overcharging protection is preventing the computer from charging.

Below is another function that is similar to Activity Monitor's Energy tab, but run from a terminal.

```text
powertop() {
  top -stats pid,command,power -o power
}
```

As before it isn't persistent, so you'll want to add it to your profile if you want to use it after a reboot or after you close the terminal. To use it, just run 'powertop'.

Another command that you can use to track energy usage is called powermetrics. It is a macOS built-in command that will provide more in-depth stats on what your CPU is actually doing.

```text
$ sudo powermetrics --show-process-energy
```

### Coconut Battery

If you prefer GUI tools, or would like power usage in your menu bar, CoconutBattery is a great resource. You may notice that you aren't getting any cycle count statistics and your battery temperature is reporting -273C. Don't worry about that, your battery probably doesn't have those features.

[Visit the Coconut Battery website](https://www.coconut-flavour.com/coconutbattery/)

## Optimizing Your Battery Life

### CPUFriend

If you are using the SMBIOS of a 2015+ Macbook or 2016+ Macbook Pro, the tool for this job is called CPUFriend and you can download it from Github. If you are using the SMBIOS of an earlier Macbook or Macbook Pro, skip to the ssdtPRGen section instead.

[Download CPUFriend @ Github](https://github.com/acidanthera/CPUFriend)

CPUFriend needs a helper to operate properly on your system, and corpnewt has written a tool that will help you do exactly that called CPUFriendFriend. Visit the project page for instructions on how to download and use it.

[CPUFriendFriend @ Github](https://github.com/corpnewt/CPUFriendFriend)

When you run CPUFriendFriend you will be prompted to enter your base clock speed. This is the TDP-down clock rate if supported. If you don't know, or if it isn't disclosed, try 800MHz.

Look your CPU up on Intel's Ark website to find the data that you need.

[Visit Intel's Ark Website](https://ark.intel.com/content/www/us/en/ark.html#@Processors)

Next you will be asked to enter an EPP value. This is explained in the instructions, but below is a table that may help you make your decision.

| Value | Description |
| :--- | :--- |
| 0x00 | Performance - Instructs the processor to scale as fast as possible. |
| 0x40 | Balance performance - Scale at a rate that offers better performance while considering battery life. |
| 0x80 | Balance power - Scale at a rate that prefers battery life while slightly sacrificing performance. |
| 0xC0 | Max Power saving - Sacrifice performance for battery life while still performing well enough for normal tasks. |

Once CPUFriendFriend has been run, copy the CPUFriend and CPUFriendDataProvider kexts to C/k/O. Once that's finished, open your config.plist and make sure that SSDT/Generate/PluginType is set to true. Finally, reboot.

You should immediately notice a difference in battery life.

### ssdtPRGen

Users of SMBIOS of Macbook Pros earlier than 2016 should use tool called ssdtPRGen written by Piker Alpha to configure scaling. Visit the project page and use the tool to create an SSDT for your CPU.

[Visit ssdtPRGen @ Github](https://github.com/Piker-Alpha/ssdtPRGen.sh)

Using ssdtPRGen is relatively simple, run the utility and follow the prompts. This will create a frequency configuration for your system and place the files in ~/Library/ssdtPRGen. Copy ~/Library/ssdtPRGen/ssdt.aml to CLOVER/ACPI/patched, update CLOVER if you are using a strict SSDT configuration \(sorted order with an explicit list of SSDTs in config.plist\), make sure SSDT/Generate/PluginType is set to True, and reboot. If you are not using a strict SSDT configuration in CLOVER you can ignore that step and just reboot.

