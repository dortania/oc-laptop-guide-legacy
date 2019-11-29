# Disabling TouchID lookup

When using the SMBIOS of a MacBook Pro with TouchID, you may notice quite a lot of lag getting to the login screen, or whenever you need to enter your password. The cause of this is macOS trying to communicate with the TouchID sensor. Additionally, there can be issues with the device hanging at the Welcome screen after installing macOS. Even though you don't have one, the OS thinks that you do so it will continue to try to authenticate with it until you turn it off. The good news here is that turning it off is incredibly simple, you just need the NoTouchID kext from al3xtjames's Github. Download it using the link below, and install it to C/k/O. If you're using an SMBIOS that doesn't have TouchID, you can skip this one.

[Download NoTouchID @ Github](https://github.com/al3xtjames/NoTouchID)

The location of the kext should match the tree view below.

```text
EFI
└── CLOVER
    └── kexts
        └── Other
            └── NoTouchID.kext
```

Finished? Great!

