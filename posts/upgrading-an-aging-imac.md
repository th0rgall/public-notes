---
title: "Upgrading an aging iMac"
slug: "upgrading-an-aging-imac"
published: "2017-02-02T00:00+02:00"
year: 2017
description: "Installing an SSD, a clean OS and adding memory to a Late ‘09 iMac"
tags: ["mac", "apple", "hardware", "tinkering"]
---

Lately, my families 27” iMac from Late 2009 ran slow.

The 4GB memory space quickly became crowded on a normal workload. A memory-leaking Safari on an old and dirty OS X version didn’t help either. You could hear the stutters of the hard disk after eight years of tiring duty.

So, it was time for some fresh components and software for this still beautiful machine!

## Memory 🆙

![ram.jpg](./assets/upgrading-an-aging-imac/ram.jpg)

We decided to double the memory to 8GB. This gives applications a very comfortable headroom.

It’s best to match exisiting memory patterns, so with 2x2GB in place, we filled the other two slots the same way. Fitting memory modules could easily be found at a local Apple dealer, but they’re on the internet too.

Installing was only a matter of unscrewing the bottom panel and inserting the modules.

## From hard to solid

:::gallery
![1 set](./assets/upgrading-an-aging-imac/1_set.jpg)
![2 new](./assets/upgrading-an-aging-imac/2_new.jpg)
![3 old](./assets/upgrading-an-aging-imac/3_old.jpg)
:::

Replacing a hard disk with a solid state drive reduces noise and increases system responsiveness, essentially what we needed.

After some research, I found that installing an SSD on a Late 2009 iMac is a little tricky, but feasible. Unfortunately you need a rather expensive [upgrade kit](https://eshop.macsales.com/shop/ssd/owc/imac-27-inch/late-2009) including a temperature sensor cable for the new drive.

As for the SSD itself, we got a Crucial MX300, a low-end option. Getting an SSD with top-notch speed would be a waste anyway, as it’s limited by the older SATA II 3Gb/s interface of the iMac.

## Installation

I wanted a clean installation of the latest macOS (Sierra) on the SSD, with user data from the old system.

I had a peculiar idea of doing this, with some doubts as to whether it would work. I put a [forum thread](https://discussions.apple.com/thread/7821640) up at Apple Communities, but in the end I just tried it:

1. Backed up the important stuff (*of course*).
2. Connected the SSD to the iMac *via an USB adapter*.
3. Installed macOS Sierra directly to the SSD *as an* [*external startup disk*](https://support.apple.com/en-us/HT202796).
4. Booted the iMac  SSD *via USB* & migrated old data and apps manually.
5. Made an external backup of important documents & most of the `/Library` folder, using `rsync`.
6. Disassembled the iMac & replaced the internal HDD by the SSD as [shown by OWC](https://vimeo.com/139362797), then reassembled it.
7. Done!

The iMac directly booted without problems from the previously external startup disk that became an internal one. This was one of my worries, but luckily there were no weird bootloader complaints.

## The mail crux

Now on to the the most difficult step in the whole process: transferring mails.

I didn't want to use the Migration Assistant, as it offers very coarse selection capabilities and would copy over all the rubbish from the old system too.

According to numerous blog and forum posts, transferring them manually was easy. But the old mac ran OS X Mountain Lion, and starting from El Capitan, Apple seems to have changed the internal structure for mail app storage, effectively obfuscating it. Also the way mail accounts are stored has changed. After many attempts, a copy-paste of data and config files just didn't work out.

I ended up recreating accounts, exporting and importing mailboxes as archives as described [here](http://apple.stackexchange.com/questions/208766/mail-gone-after-clean-install-of-el-capitan). Not ideal, but it works.

## Results?

The operation was succesful! Boot time has improved, but not by an amazing margin. Applications definitely react quicker. Not unimportant, we removed a *hell of a lot* of dust from the computers' internals.

The Mac's fans however are constantly humming (softly), and I'm not sure whether this was the case before. Maybe there's something wrong with the temperature sensor cable, though the measurements seem fine. Maybe it's a new software policy in Sierra. It's not too annoying.

:::gallery
![3 old](./assets/upgrading-an-aging-imac/3_old.jpg)
![IMG 7161](./assets/upgrading-an-aging-imac/img_7161.jpg)
![IMG 7163](./assets/upgrading-an-aging-imac/img_7163.jpg)
![mac stof](./assets/upgrading-an-aging-imac/mac_stof.jpg)
![IMG 7166](./assets/upgrading-an-aging-imac/img_7166.jpg)
![IMG 7170](./assets/upgrading-an-aging-imac/img_7170.jpg)
![mac onderkant](./assets/upgrading-an-aging-imac/mac_onderkant.jpg)
![IMG 7175](./assets/upgrading-an-aging-imac/img_7175.jpg)
![ram](./assets/upgrading-an-aging-imac/1LQXIG-ram.jpg)
:::
