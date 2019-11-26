---
layout: post
title: BrcmPatchRAM3 v2.5.0 + BCM94352Z/DW1560 is broken
categories: [ macOS, Hackintosh ]
---

If you own DW1560 (also branded as BCM94352Z or AW-CE162NF) and experience some issues with Bluetooth, check the version of the kext. I found out the hard way, it's somewhat broken for DW1560.

## macOS Catalina + BrcmPatchRAM2.kext

Once you upgrade to macOS Catalina, `BrcmPatchRAM2.kext` no longer works. You have to  upgrade Bluetooth kexts to [`BrcmPatchRAM3.kext`](https://github.com/acidanthera/BrcmPatchRAM). I noticed that booting with `BrcmPatchRAM3-v2.5.0` breaks the 'Call from iPhone' feature.

## The bug has been reported

I contacted the author of the patches on [insanelymac.com](https://www.insanelymac.com/forum/topic/339175-brcmpatchram2-for-1015-catalina-broadcom-bluetooth-firmware-upload/?do=findComment&comment=2696479) and filed an issue on [acidanthera's BugTracker](https://github.com/acidanthera/bugtracker/issues/563). So far, the cause hasn't been identified.

## Workaround

Previous testing version [BrcmPatchRAM3-V2.3.0d3](https://www.insanelymac.com/forum/applications/core/interface/file/attachment.php?id=335073) works fine for me. Bare in mind that to be able to download the file, you have to be logged in to insanelymac.com.
