# Getting ready to build Hackintosh using OpenCore

Do you know what Hackintosh is? In short:

> Building a PC computer using off-the-shelf parts capable of running macOS. Then actually tricking macOS to install and run. 

When I got the idea to attempt this, I was a complete noob. Utter and complete noob as more than 90% of people I’ve seen seeking help online.

I believe it’s helpful to read something with perspective of a newbie who managed to achieve the goal. Note that this is *not a guide*, just a diary of my journey, with the hardware I have.

Thus don’t expect to find all the answers here. 
You’ll still need to learn and do your research. It never hurts to learn.

· ▪︎ · ▪︎ · ❖ · ▪︎ · ▪︎ ·

Let me tell you right out of the box: to do this, you *need* to: 

- Be patient.
- Be tenacious.
- Be very comfortable changing stuff in BIOS / UEFI of your PC. If you don’t know what those terms mean or you never entered into one before, *don’t even try*. Sincerely.
- Be prepared to read *a lot* of very technical documentation and learn a ton of new stuff.
- Have a working 64bit Windows 10 installation *on separate disk*[^1] in the machine you want to convert into Hackintosh. 
- Have decent *plain text* editor and two USB 3.0 flash drives (one of any size and another of 16GB).
- Have at least a week or two to spend on this. As minimum.

If you don’t see yourself checking all those points, this is not for you. I really mean it: **this may not be for you**. There’s a fair chance to screw up your BIOS and make the machine so useless you’ll need to find a service nearby, you may render your (possibly) existing Windows 10 installation unusable etc.

Just save up more money, buy a proper Mac as built by Apple and enjoy what you have. 

### Hardware

If you don’t already have PC, then you must be reading this in preparation to build your first *and* make it a Hackintosh. That’s…ambitious. 

First read through [recommended hardware](https://github.com/khronokernel/Opencore-Vanilla-Desktop-Guide/blob/master/extras/hardware.md). Choosing something that other people already used to build Hackintosh will certainly make the task easier. You stand a better chance of someone helping you out.

If you already have some PC, still read through that list. You’ll know if your hardware is usable and what parts you may need to upgrade / replace.

### Software

Good tools make stuff much easier. 
To edit `config.plist`, you need to use one of these:

* [PlistEdit Pro](https://www.fatcatsoftware.com/plisteditpro/), for macOS. My pick.
* [Xcode](http://developer.apple.com/xcode/)
* [ProperTree](https://github.com/corpnewt/ProperTree)
* plain text editor like [TextMate](https://macromates.com) or [BBEdit](https://www.barebones.com/products/bbedit/index.html) or whatever is useful on Windows (Notepad will work too).

You will need one more tool to reveal your EFI (boot) partition where the OpenCore setup will be copied. 

* [MountEFI](https://github.com/corpnewt/MountEFI), command line utility

There are other ways to reveal this partition but this one was enough for me.

The following tools are incredibly useful for post-macOS-installation fine-tuning.

* [MaciASL](https://github.com/acidanthera/MaciASL/releases) – automatically reads DSDT for the machine, can create new SSDT files in ASL format. 
* IORegistryExplorer (for Mac) to browse active ACPI setup. You can find this tool all over the Internet’s (more or less shady) download sites. If you have an Apple developer account, you can find it as part of Xcode 11 additional tools (search for them in [More Downloads](http://developer.apple.com/download/more/) section).
* [gfxutil](https://github.com/acidanthera/gfxutil/releases), command line utility to lookup / extract Device Properties
* [Hackintool](https://mac.softpedia.com/get/System-Utilities/Hackintool.shtml) to review active USB ports, PCI device list and other info (although some of the stuff it displays is wrong on AMD systems).

### Assumptions

If you want to follow this guide, you need to forget that Clover and various *Beasts exist. You’ll encounter them all the time, all over the place but just ignore their existence. I did.

The aim here is to install macOS Catalina (also known as macOS 10.15) on the target machine based on AMD’s X570 chipset and Ryzen 3600 CPU. Starting with 10.15.2, [OpenCore](https://github.com/acidanthera/OpenCorePkg) is the only way to build Hackintosh *if* your PC build uses AMD CPU. 
I recommend to go with OpenCore regardless of what CPU platform you are using, Intel or AMD.

People that work on it are trying to make it completely independent from Apple’s software, as in – don’t touch nor alter anything in macOS installation. Instead, they prepare the environment in which the installer (and later macOS itself) is running so it works out of the box.

I have used my MacBook Pro to prepare the OC setup, then copied the EFI to the USB or the target machine. Thus all explanations will reference macOS-based tools.

## Where to start

There’s an [extensive guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/) on how to prepare the OpenCore environment. That guide is super-detailed but written by someone who is deep into this domain. Throughout the guide you will find sentences which make sense when you “know stuff” but sound like utter gibberish when you’re a newbie. Like this one:

> For those having trouble understanding the SSDTs regarding EC can use CorpNewt's SSDTTime to properly setup your SSDT. 

Reading this for the first time, I had no idea what SSDT nor its DSDT cousin are.[^2] You’ll have these “WTF…🥺🤯” moments all the time. This is why I said you need to be ready to learn; *a lot*.

Be that as it is, *that guide is indispensable*. It’s full of great screenshots, lots of hints to optional stuff and it invites you to explore, search and learn more. Plus it’s constantly being updated.

Double, triple and *quadruple-check every sentence and screenshot* in it and make sure you set up your keys and values in the `config.plist` as required. I read it multiple times before trying for the first time and *still* needed 6-8 attempts before getting the base minimum of macOS working. Every time I encountered some error, I returned to the guide, re-read the parts that seemed relevant and found something I did wrong or completely missed to do. *It is hard*, don’t believe anyone saying it’s a breeze.[^3] 

The second important document you need to read, cover to cover, is [Configuration.pdf](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf) from the OpenCorePkg itself. It contains thoroughly detailed explanation of every option you’ll find in the `config.plist`, the heart of the OC setup. This is surprisingly comprehensive documentation and it shows dedication deserving of utmost respect.

Final resource is the community you can find at:

- [Hackintosh](https://www.reddit.com/r/hackintosh/) subreddit (hint: read the sidebar links)
- [AMD OS X](https://vanilla.amd-osx.com), their [forums](https://forum.amd-osx.com) and [Discord](https://discord.gg/EfCYAJW) channels.

As in any community, be respectful and polite. Don’t ask general questions like “how to map my USB ports”. Focus on specific issue you are facing, have a screenshot or a photo to share; make the effort to show you have read the guide. I asked a lot of questions and about 1/3 of them got answered or at least I got pointers where to dig.

Lastly: *don’t act entitled to time nor answers* from people that make this possible. 

## Prepare macOS installer

You’ll need a macOS installation loaded onto 16GB USB stick which you can boot from. I already have Macbook Pro so it was easy to get the installation from App Store and [create the bootable USB](https://support.apple.com/en-us/HT201372).

Otherwise, checkout AMD OS X Vanilla’s [Installation part](https://vanilla.amd-osx.com/#installation-section) or [similar section](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/opencore-efi/winblows-install) for Windows in OpenCore Vanilla Desktop guide.

I mentioned two USB sticks. The second, smaller one, will be used for the OpenCore setup which can be under 1MB but it can also be up to 10MB or more. Regardless, any USB stick from last 10 years would work although I recommend to get something using USB 3.0 since booting up will be way faster. 

Do not use the same USB disk as for the macOS installer. Just…don’t. I find it best to keep all these things separated and avoid stupid mistakes.

I’ll end this introduction here and leave you to build your PC, if you don’t already have one. 

---

Make sure to install Windows 10 *64bit* edition on your PC and make sure that it’s working proper and stable. That means: all drivers installed, all hardware working. No hardware should be marked with yellow warning triangles in [Device Manager](https://support.microsoft.com/en-us/help/4026149/windows-open-device-manager). Test the machine with one of many stress test tools to make sure hardware is OK.

If base installation of Windows 10 is not working properly, the macOS won’t either. 

Another reason that you need Windows running on that machine is that some of the [first steps of OpenCore build](../opencore-first-steps.md) will need to be performed on that target machine.

[^1]: It’s technically possible to do dual-OS-boot on the same disk but this is far more advanced setup than I ever intend to do. Buy another SATA SSD, things are much easier that way.

[^2]: I’m not sure I entirely do, even today.

[^3]: Side-benefit: you get to have a whole new level of respect for people that do that work for you, at Apple, Corsair, HP, Dell, Lenovo etc.