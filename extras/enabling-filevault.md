# Enabling FileVault

## What the heck is FileVault?

FileVault is Apple's solution to whole disk encryption. Once enabled, it will use your CPUs encryption extensions to encrypt and decrypt data as you're using it. The credentials to unlock the disk are tied to your user and iCloud accounts, and integrated into the system for a relatively seemless experience.

## [Setting up FileVault](https://desktop.dortania.ml/post-install/security)

You should never just enable FileVault on a Hackintosh without first installing the prerequisites. Doing so will encrypt your disk, leaving you stuck. OpenCore was made with security/filevault in mind so it has good support assuming it is set up correctly. If you want to set it up, take a look at the page linked above.


