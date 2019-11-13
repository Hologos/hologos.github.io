---
layout: post
title: How to upgrade Clover v2.4k to v2.5k (rev. 5000+)
categories: [ macOS, Clover Bootloader, Hackintosh ]
---

Guys developing Clover made a change to Clover's directory structure and somehow forgot to inform people. In my opinion, mentioning it in a [forum post](https://www.insanelymac.com/forum/topic/304530-clover-change-explanations/?do=findComment&comment=2681356) is not enough.

**UPDATE: It seems like they finally implemented some sort of automatic migration in latest version (rev. 5097) of Clover installer although they specifically said no migration will be done in the forum post.**

The structure for bootloader drivers changed in v2.5k from

```
EFI
  CLOVER
    drivers64
    drivers64UEFI
```

to

```
EFI
  CLOVER
    drivers
      BIOS
      off
      UEFI
```

Also, driver names no longer contains `-64` in them (e.g: `ApfsDriverLoader-64.efi` becomes `ApfsDriverLoader.efi`).

## Updating Clover bootloader

**CAUTION: Always have bootable USB drive (RECOVERY) with working CLOVER installation ready in case something fails!**

It's a good practice to have separate USB drive to test Clover updates on. First make it work on the testing USB drive and then move to the main drive. If everything is working after few weeks, update the RECOVERY USB drive as well.

## Downloading Clover bootloader installer

Download latest version of Clover bootloader installer from [official Github](https://github.com/CloverHackyColor/CloverBootloader/releases).

## Run Clover bootloader installer

Run the installer and follow instructions from your installation guide (select mandatory drivers for your setup).

## Fixing the installation from command line

Now your Clover bootloader rev. 5000+ is broken and you will not be able to boot from it. You have to manually fix it.

### Mount your EFI partition

```bash
diskutil list

    /dev/disk0 (internal, physical):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:      GUID_partition_scheme                        *240.1 GB   disk0
        1:                        EFI EFI                     209.7 MB   disk0s1
        2:                 Apple_APFS Container disk1         239.8 GB   disk0s2

    /dev/disk1 (synthesized):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:      APFS Container Scheme -                      +239.8 GB   disk1
                                        Physical Store disk0s2
        1:                APFS Volume OSX                     10.9 GB    disk1s1
        2:                APFS Volume VM                      1.1 MB     disk1s2
        3:                APFS Volume OSX – data              128.6 GB   disk1s3
        4:                APFS Volume Preboot                 83.7 MB    disk1s4
        5:                APFS Volume Recovery                528.5 MB   disk1s5

    /dev/disk2 (external, physical):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:      GUID_partition_scheme                        *500.1 GB   disk2
        1:                        EFI EFI                     209.7 MB   disk2s1
        2:          Apple_CoreStorage Time Machine            349.9 GB   disk2s2
        3:                 Apple_Boot Boot OS X               134.2 MB   disk2s3
        4:       Microsoft Basic Data Stash                   149.7 GB   disk2s4

    /dev/disk3 (external, virtual):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:                  Apple_HFS Time Machine           +349.5 GB   disk3
                                      Logical Volume on disk2s2
                                      3DEBA78B-97CF-4C5A-AB4E-5393FC91D142
                                      Unlocked Encrypted

    /dev/disk4 (external, physical):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:      GUID_partition_scheme                        *7.8 GB     disk4
        1:                        EFI EFI                     209.7 MB   disk4s1
        2:                  Apple_HFS CloverBootloader        7.4 GB     disk4s2
```

Find _Disk identifier_ of EFI partition you have just upgraded. In my case, it's `disk4s1`. To mount this partition, run:

```bash
sudo diskutil mount disk4s1
```

It will mount the EFI partition to `/Volumes/EFI`.

**Update:** I released an article dedicated to [mounting volumes using command line](/how-to-mount-efi-from-command-line-terminal).

### Moving & renaming drivers

Commands below will move drivers to appropriate location and rename them. It will move only those drivers **not updated** with Clover installer.

```bash
cd /Volumes/EFI/EFI/CLOVER

mkdir -p drivers/{off,BIOS,UEFI}

for driver in $(find drivers64 -name "*.efi" -print0 | xargs -0 -n1 basename); do
    if [[ ! -e "drivers/BIOS/${driver/-64/}" ]]; then
        cp "drivers64/${driver}" "drivers/BIOS/${driver/-64/}"
    fi
done

for driver in $(find drivers64UEFI -name "*.efi" -print0 | xargs -0 -n1 basename); do
    if [[ ! -e "drivers/UEFI/${driver/-64/}" ]]; then
        cp "drivers64UEFI/${driver}" "drivers/UEFI/${driver/-64/}"
    fi
done
```

Now you should have working Clover installation v2.5k rev. 5000+.

After few weeks if everything is ok, you can remove `drivers64` and `drivers64UEFI` folders.
