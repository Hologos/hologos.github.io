---
layout: post
title: Problems with macOS 10.15 update
categories: [ macOS, Hackintosh, Clover, OpenCore ]
---

Since last year I haven't have much time to solve problems regarding updating all my hackintoshes. I finally found some spare time last week to update my HP EliteBook 840 G3 that was stuck on **macOS 10.15.1**. I wanted to update all the way to **macOS 10.15.7** in one go but it didn't went as I was hoping.

## Preparation for update

### Backup

First and foremost **I did full backup of my installation** (my method [using nothing but Terminal](/upgrade-from-macos-mojave-to-macos-catalina/#backup)).

### Verifying bootable USB EFI partition

 I verified my bootable USB had working EFI partition by booting from it. This is very useful in case your machine is not capable of booting anymore (after kexts update, Clover bootloader update, etc).

### Updating essential kexts

I moved to updating my outdated kexts using [Kext Updater](https://bitbucket.org/profdrluigi/kextupdater/downloads/) since it's easier than checking it by hand. I excluded AirportBrcmFixup, BrcmPatchRam, BT4LEContinuityFixup and Clover from the update because I like to update Clover separately as well as Wifi/Bluetooth. I had some problems with [BrcmPatchRam3 v2.5](/brcmpatchram3-v2.5.0-bcm94352z-dw1560-is-broken/) hence I update and test properly.

I restarted the machine and booted from main drive EFI partition and it worked like a charm so I moved to updating **Clover bootloader**. Kext Updater downloaded r5126 so [I read through the changelog](https://github.com/CloverHackyColor/CloverBootloader/releases) and found nothing of interest so I gave it a go. Sadly I wasn't able to boot with it anymore, I kept getting `[EB|#LOG:EXITBS:START] 2020-11-14T23:41:02`. Thanks to my bootable USB I was able to fix it right away by downgrading back to r5097. If I didn't have working bootable USB, I wouldn't be able to repair that and I would have to do full restore or in case I didn't have a backup, I would have to do fresh install (I use m2 SSD and I don't have the ability to connect it to my other hackintoshes).

![Bootlog of Clover r5126](/images/2020-11-16-problems-with-macos-10.15-update/bootlog.jpg)

_This is the error message I was getting while booting (screenshot is courtesy of [norton287](https://github.com/CloverHackyColor/CloverBootloader/issues/84#issuecomment-601294615))._

## Updating to macOS 10.15.7

I decided to give the update a go with current Clover installation but I wasn't successful. I googled a little and [found this Clover issue](https://github.com/CloverHackyColor/CloverBootloader/issues/84). Reading through showed me I had to use [at least Clover r5107](https://github.com/CloverHackyColor/CloverBootloader/issues/84#issuecomment-604327750) to be able to get past macOS 10.15.4. I went back and read through the Clover changelog and discovered r5122 is the last version noted as v5.0, following versions are v5.1. I updated to Clover v5.0 r5122, restarted and finally booted. I then proceeded with the macOS update and I was finally successful.

## Moral of the story

Developers of Clover bootloader are notoriously known for not informing users about changes. If I didn't search the internet, I would't have known the changes introduced in r5123 and [mandatory changes to your config.plist](https://www.tonymacx86.com/threads/new-clover-release-v5-1-r5123-big-sur-support.304543).

For this very reason, **I decided to move from Clover bootloader to OpenCore bootloader**. I will post my findings about the transition and upload my EFI folder to my [Github repo](https://github.com/Hologos/hackintosh-hp-elitebook-840-g3) as well.
