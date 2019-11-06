---
layout: post
title: How to prevent volume automount on macOS Catalina
categories: macOS
---

I have a hard drive dedicated for Time Machine backups and recently I decided to split the partition into 2. The second one became storage for a disk image of my main drive (in case my Hackintosh update fails). What bothered me was that every time I connected my Time Machine drive, the second partition got connected as well. How to prevent that?

**Disclaimer:** Proceed with caution and on your own risk.

First, we have to find out, what is the _Disk identifier_:

```bash
diskutil list
...
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.1 GB   disk2
   1:                        EFI EFI                     209.7 MB   disk2s1
   2:          Apple_CoreStorage Time Machine            349.9 GB   disk2s2
   3:                 Apple_Boot Boot OS X               134.2 MB   disk2s3
   4:       Microsoft Basic Data Stash                   149.7 GB   disk2s4
...
```

The volume in question is called **Stash** and its _Disk identifier_ is **disk2s4**. Now we have to get the _Volume UUID_:

```bash
diskutil info /dev/disk2s4
   Device Identifier:         disk2s4
   Device Node:               /dev/disk2s4
   Whole:                     No
   Part of Whole:             disk2

   Volume Name:               Stash
   Mounted:                   No

   Partition Type:            Microsoft Basic Data
   File System Personality:   ExFAT
   Type (Bundle):             exfat
   Name (User Visible):       ExFAT

   OS Can Be Installed:       No
   Media Type:                Generic
   Protocol:                  USB
   SMART Status:              Not Supported
   Volume UUID:               7727ED95-7E4C-3A6F-A25C-79F2D5F0721B
   Disk / Partition UUID:     D2A8FB28-2F27-4BEA-990A-D94A3691FA5D
   Partition Offset:          350241161216 Bytes (684064768 512-Byte-Device-Blocks)

   Disk Size:                 149.7 GB (149732458496 Bytes) (exactly 292446208 512-Byte-Units)
   Device Block Size:         512 Bytes

   Volume Total Space:        0 B (0 Bytes) (exactly 0 512-Byte-Units)
   Volume Free Space:         0 B (0 Bytes) (exactly 0 512-Byte-Units)

   Read-Only Media:           No
   Read-Only Volume:          Not applicable (not mounted)

   Device Location:           External
   Removable Media:           Fixed

   Solid State:               Info not available
```

The only thing left is to disable auto mount. For that we have to call `sudo vifs` as it is recommended to alter _fstab_ using `vifs`.

**If you are not familiar with `vi` program, DO NOT PROCEED. Find some tutorial first and when you are comfortable with it, come back and continue.**

There we have to enter line like this:

```bash
UUID=7727ED95-7E4C-3A6F-A25C-79F2D5F0721B       none    exfat   rw,noauto
```

Don't forget to alter the line according to your values (UUID and the filesystem in the third column).
