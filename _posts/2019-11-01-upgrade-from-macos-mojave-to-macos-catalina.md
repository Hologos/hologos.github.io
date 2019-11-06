---
layout: post
title: Upgrade from macOS Mojave to macOS Catalina
categories: [ macOS, Hackintosh, HP EliteBook 840 G3 ]
---

This guide is meant for _HP Probook/Elitebook/Zbook_ but it can be used for all hackintoshes. The only difference is that you have to get all kexts by yourself.

## Before upgrade

### Backup

Always make a backup before you update/upgrade. Time Machine backup **is not enough** for Hackintosh, bit-by-bit backup is recommended. I do my backup using 2 hard drives and a USB stick with macOS Installer on it, because I don't want to pay for Carbon Copy Cloner (stay tuned for my guide _How to clone disk using APFS snapshots_)).

I connect both my hard drives (using SATA or USB, the only difference is the speed) and boot into the installer.

**You have to be very careful not to override your primary hard drive or to format it!**

On the main screen, launch Disk Utility. Format the spare hard drive to have to empty. Then close the Disk Utility and run Terminal from Utilities menu.

Identify disk number of your primary and secondary disk.

```bash
diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *128.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                 Apple_APFS Container disk1         127.8 GB   disk0s2

/dev/disk2 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *128.0 GB   disk2

...
```

Here my primary disk is `/dev/disk0` and secondary is `/dev/disk2`. We will reference these drives as `/dev/rdisk0` and `/dev/rdisk2`.

```bash
dd if=/dev/rdisk0 of=/dev/rdisk2 bs=1m conv=noerror,sync
```

Now we have to just wait till it's finished.

### Update essential kexts

Boot into the OS and update and install kexts from Rehabman's repo according to the [guide](https://www.tonymacx86.com/threads/guide-hp-probook-elitebook-zbook-using-clover-uefi-hotpatch.261719/). Namely sections **Updates to the patch repositories** and **System updates**.

## Upgrade

**DON'T FORGET TO DO A BACKUP!**

Now you can start the upgrade from the Mac App Store.

## Post-upgrade

Install kexts from Rehabman's repo according to the [guide](https://www.tonymacx86.com/threads/guide-hp-probook-elitebook-zbook-using-clover-uefi-hotpatch.261719/) again.

With my setup, my Wifi was working but the Bluetooth was not. Since Rehabman is currently not active, I had to migrate to [Mieze's BT kexts](https://github.com/Mieze/OS-X-BrcmPatchRAM-Catalina). Download latest BT kexts from thread [BrcmPatchRAM2 for 10.15 Catalina](https://www.insanelymac.com/forum/topic/339175-brcmpatchram2-for-1015-catalina-broadcom-bluetooth-firmware-upload/?page=6) (`BrcmPatchRAM3.kext`, `BrcmFirmwareRepo.kext` and `BrcmBluetoothInjector.kext`) and copy them to `/Library/Extensions` (remove old ones first).

**Update:** You can use my EFI folder if you have the same model (choose the branch according to the version you are upgrading to).

Since macOS Catalina, system partition is read-only, you have to mount it first as read-write:

```bash
sudo mount -uw /
```

Then rebuild kext cache:

```bash
sudo chown -v -R root:wheel /System/Library/Extensions
sudo touch /System/Library/Extensions
sudo chmod -v -R 755 /Library/Extensions
sudo chown -v -R root:wheel /Library/Extensions
sudo touch /Library/Extensions
sudo kextcache -i /
```

Also, don't forget to disable hibernation (it can enable with the update):

```bash
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
```

That's it, everything is working for me (except the mute button with builtin speakers, but that's a story for another time).
