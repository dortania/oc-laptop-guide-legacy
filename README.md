# Getting Started

## Why OpenCore?

Many guides out there use Clover - and for good reason. It's been around for many years and there is a large knowledge base around it. Recently though, Clover has been showing glaring pitfalls - many of which OpenCore solves. Starting with release 5092, Clover does not inject ethernet/wifi kexts when going into the installer, making the internet installation method impossible. Additionally, laptops sometimes will refuse to boot with Clover.

* OpenCore has a cleaner codebase and clear debug logging
* OpenCore has a large amount of documentation
* On average, will start up faster
* Supports Bless and Bootcamp switching
* Has further developments to AptioMemoryFix directly within OpenCore and FwRuntimeServices
* Uses a kext injection method which is more future proof, which does not break System Integrity Protection \(SIP\)

That said, Hackintosh on a laptop is _still_ hard. You'll likely still spend tens of hours reading documentation and troubleshooting. If you are here to just make things work and not spend the time learning, then you are better off buying a macbook instead.

This guide is forked from Fewtarius' laptop-guide, and a lot of the content from this guide not only comes him, but many other people. Here is a section from his guide:

`On that note, while it's impossible to personally thank everyone in the community; I do want to specifically thank the following people for their help and their contributions to myself, to this guide, and to the community. This guide would not have been possible without their guidance, and their knowledge.  
  
* CorpNewt  
* Midi  
* Dids  
* Dhinak G  
* Alexandred  
* Hackintosh Slav  
* osy  
* Goldfish64  
  
Also a special thanks to all of the helpers on /r/Hackintosh and HackintoshParadise.`

This guide is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-nc-sa/4.0/), like the guide that this is forked from. That means you're welcome to share and adapt the work, but this guide is not for commercial use. If you do reuse it in whole or in part, any bits that you do use must use the same license and you'll need to provide proper attribution.

![Attribution-NonCommercial-ShareAlike 4.0 International](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-Lmzu1VXuh4aEElEhDUq%2F-LtpixV0Vo8qV7aALPNN%2F-Ltpj3q8HDnQFB8crjg5%2Fcc-by-nc-sa.png?alt=media&token=b695b8db-6dbe-407a-b8b7-bea52ef68898)

Alright, what are you waiting for? Click the box at the bottom of the page to continue and let's get started

