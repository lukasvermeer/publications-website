---
layout: post
title: "Copying Bits in the Fastlane"
date: 2010-07-03
tags: ["Android","Bash","Linux"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2010/07/03/copying-bits-in-the-fastlane/
---

I've finally gotten 'round to [rooting](http://wiki.allshadow.com/index.php/G1_Root) my 'old' [G1](http://www.t-mobileg1.com/) phone and installing the [Cyanogen](http://www.cyanogenmod.com/) firmware. I will not bore you with the technical details of flashing firmware, but I would like to share an observation. To illustrate my point, I will need to cite some of the steps involved.

**The technicalities of the following citations do not matter all that much, so if you want, you can skip right to the short translation. There will be no pop-quiz at the end, I promise.**

Rooting the G1 is not as complicated as you might expect from the [enormous amount of steps involved](http://mapleator.de/?p=19). The biggest obstacle for me proved to be the fact that I do not have a copy of Windows installed on any of my computers, so the following steps in the ["the unlockr" guide](http://theunlockr.com/2010/03/10/how-to-create-a-goldcard/) (referred to by none other than [Cyanogen](http://wiki.cyanogenmod.com/index.php/Full_Update_Guide_-_G1/Dream_Firmware_to_CyanogenMod#Troubleshooting) himself) were practically impossible for me (underlining is mine; and as promised, there is a short translation below).
> <span style="color:#c0c0c0;">9. Now, goto </span><span style="color:#c0c0c0;">http://download.cnet.com/HxD-Hex-Editor/3000-2352-10891068.html?part=dl-HxDHexEdi&subj=uo&tag=button</span><span style="color:#888888;"><span style="color:#c0c0c0;"> to </span><span style="text-decoration:underline;"><span style="color:#c0c0c0;">download the HxD Hex Editor</span></span><span style="color:#c0c0c0;">. Save it and install it to your computer.</span></span>> 
> 
> <span style="color:#c0c0c0;">10. Take your SD card out of your phone and put it into the SD adapter it came with. Then put that into your computer so it shows up on your computer as Removable Disk.</span>> 
> 
> <span style="color:#888888;"><span style="color:#c0c0c0;">11. Open the Hex Editor (</span><span style="text-decoration:underline;"><span style="color:#c0c0c0;">Run as Administrator</span></span><span style="color:#c0c0c0;"> if one Vista or Windows 7) and click on the Extra tab, then click on Open Disk. </span></span>**<span style="color:#c0c0c0;">Under Physical Disk</span>**<span style="color:#c0c0c0;"> select Removable Disk (your SD card you just put into the computer). Make sure to UNcheck "Open as ReadOnly". Click OK.</span>> 
> 
> <span style="color:#c0c0c0;">12. Goto the Extra tab again and click Open Disk Image. Open up the goldcard.img that you saved from your email. You should now have two tabs, one is the SD card (Removable Disk) and the other is the goldcard.img. Press OK when prompted for Sector Size 512 (Hard Disks/Floppy Disks).</span>> 
> 
> <span style="color:#888888;"><span style="color:#c0c0c0;">13. Click on the Goldcard.img tab and click on the Edit tab and click Select All. Then click on the Edit tab again and click </span><span style="text-decoration:underline;"><span style="color:#c0c0c0;">Copy</span></span><span style="color:#c0c0c0;">.</span></span>> 
> 
> <span style="color:#888888;"><span style="color:#c0c0c0;">14. Click on the Removable Disk tab (Your SD Card) and select offset 00000000 to 00000170 then click on the Edit tab and click </span><span style="text-decoration:underline;"><span style="color:#c0c0c0;">Paste</span></span><span style="color:#c0c0c0;"> Write.</span></span>> 
> 
> <span style="color:#888888;"><span style="color:#c0c0c0;">15. Click on File then click </span><span style="text-decoration:underline;"><span style="color:#c0c0c0;">Save</span></span><span style="color:#c0c0c0;">.</span></span>
Translation: download software of dubious origin, run software as Administrator, open disk, click a few things, copy/paste a few bits, save. Apparently all we want to do here is copy some bits from a file to (the beginning of) a disk. Copying a few bits can't be this hard, right?

**I mean, this is a computer; it copies bits all the time!**

Luckily I was not the only Android enthusiast stumped by these Windows-only (and frankly, rather convoluted, manual  and error-prone) steps. [Someone else had already solved the problem for me](http://mapleator.de/?p=19) (thanks, Sven). The steps in his guide (for Mac or Linux) seems a lot simpler to me (again, underlining mine and translation below).
> <span style="color:#c0c0c0;">1 .Open your Mac's Terminal under Applications -> Utilities ->Terminal (Or your Linux-Terminal)</span>> 
> 
> <span style="color:#c0c0c0;">2. Enter </span>_<span style="color:#c0c0c0;">diskutil list </span>_<span style="color:#c0c0c0;">and confirm</span>> 
> 
> <span style="color:#c0c0c0;">3. You should be able to see your SD-Card now. You can recognize it from its size (mine is 2GB) and that its type is </span>_<span style="color:#c0c0c0;">DOS_FAT_32</span>_<span style="color:#c0c0c0;">. As IDENTIFIER it says disk2s1.Remember this identifier.</span>> 
> 
> <span style="color:#c0c0c0;">4. Now unmount the card the folowing way: If your identifier was disk2s1 enter </span>_<span style="color:#c0c0c0;">diskutil unmountDisk /dev/disk2</span>_<span style="color:#c0c0c0;"> and confirm. If not, you have to replace the 2 with your value (the value that is written right after the word disk).</span>> 
> 
> <span style="color:#c0c0c0;">5. Now create your goldcard with </span>_<span style="color:#888888;"><span style="text-decoration:underline;"><span style="color:#c0c0c0;">sudo dd bs=512 if=~/goldcard.img of=/dev/disk2</span></span></span>_<span style="color:#c0c0c0;">. If you need to, replace the 2 again. Confirm, wait, enter your users password (or under linux your roots password) and confirm again.</span>
Translation: figure out which disk you need, copy bits to disk. The [dd command](http://en.wikipedia.org/wiki/Dd_(Unix)) used here is available in every flavor of *nix I've ever encountered. Let me break that crucial fifth step down for you.

**[sudo](http://xkcd.com/149/)** I am the Administrator, do the following as I say! (normal users are not allowed copy bits this way, that would be a Very Bad Idea)

**dd** Copy bits

**bs=512** in [chunks of 512 bits](http://unix.derkeiler.com/Mailing-Lists/FreeBSD/questions/2008-09/msg01345.html) ("bs" stands for "block size", in this case, not [cow manure](http://en.wikipedia.org/wiki/Bullshit))

**if=~/goldcard.img** from this file ("if" for "in file" and "~/" just means the file is in my home folder)

**of=/dev/disk2** to this disk ("of" for "out file" and "/dev" is the [Device File ](http://en.wikipedia.org/wiki//dev)folder, where *nix pretends devices attached are just files e.g. disk2 for my SD-Card).

It could be me, but that looks a lot simpler (and faster and less error-prone) to me than the Windows instructions.

The problem seems to be that, although Windows is perceived to be easy to use (and it probably is, up to some point), the lack of real and raw power under-the-hood can make anything as trivial as copying a few bits impossible without resorting to downloading dubious applications. It's like having a car that is pretty and easy to drive, as long as you stay out of the fastlane.

And that, my friends, is one of the reasons why people like me do not like Windows very much. We nerds, we like living [life in the fastlane](http://www.youtube.com/watch?v=o5FXw0ARBUo).