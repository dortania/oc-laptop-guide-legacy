# Choosing a MacBook SMBIOS

One of the final steps on your Hackintosh installation journey is choosing the right SMBIOS for your laptop. This is a very important step as the wrong choice can impact lots of aspects of macOS including power management, and USB functionality. So you're probably asking what is an SMBIOS, and how do I choose it? Great question, let's dive in.

## The SMBIOS

SMBIOS or System Management BIOS tells macOS what hardware you're using so it knows how to set things up in a way that's optimal for the hardware it represents. All of this work happens in CLOVER via the config.plist. Choosing the right SMBIOS is actually easier than you might think. All we have to do here is match the CPU in your laptop to the MacBook, MacBook Air, or MacBook Pro that it most closely resembles.

It's not quite as simple as choosing an SMBIOS from a table, as there are lots of different models available. Instead, head over to EveryMac and look through the three models. Once you have made your selection you will need the Model ID. We'll use the table below to help visualize the data you need.

| MacBook Pro | Model ID |
| :--- | :--- |
| Apple MacBook Pro "Core i5" 2.6 13" Mid-2014 | MacBookPro11,1 |

Add the model id of your chosen device to your config.plist. Remember that the model id is case sensitive so be sure to add it exactly as it is listed.

## Serial Number Generation

Like most devices, MacBooks have unique identifiers that need to match the model id. With a Hackintosh we must be careful to not choose a serial number that is already in use, as that could cause problems for you and for the real Mac owner. To generate a serial number, we'll use a command line tool called macserial. Download it, and then run it with a command line option and your model id.

[Download MacInfoPkg @ Github](https://github.com/acidanthera/MacInfoPkg)

```text
$ ./macserial --model MacBookPro11,1
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
XXXXXXXXXXXX | XXXXXXXXXXXXXXXXX
```

As you can see, there are two data points in the output. On the left of the pipe is the generated serial number. To the right, the board serial number. Before using the generated serial number, test each one at Apple using the link below to make sure it is infact invalid. If you receive anything but the output below, discard that serial number and try again.

![](../.gitbook/assets/screen-shot-2019-11-16-at-1.38.05-pm.png)

[Check Your Service and Support Coverage at Apple](https://checkcoverage.apple.com)

Once you have an invalid serial number, you should also test it at EveryMac to ensure that even though it is invalid, the serial number structure matches the model id you have chosen. If it returns more than one device, that's ok!

[Everymac's Ultimate Mac Lookup](https://everymac.com/ultimate-mac-lookup/)

If everything checks out, add the serial number \(left\) and the coresponding board serial \(right\) to your config.plist.

## SmUUID

The SmUUID is a unique identifier that macOS uses for a variety of different things ranging from storage of some of your preferences to association within applications like messages. Generating this one is simple, on any Mac just run the uuidgen command. Copy the output to the SmUUID key in your config.plist.

```text
$ uuidgen
XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
```

That wasn't too bad, right?

