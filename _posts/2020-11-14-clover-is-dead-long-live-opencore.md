---
layout: post
title: Clover is dead, long live OpenCore!
categories: [ macOS, Hackintosh, Clover, OpenCore ]
---

Moving forward I will upgrade from **Clover** to **OpenCore**. It seems very well documented and there is pretty good [install guide from dortania](https://dortania.github.io/OpenCore-Install-Guide/).

## The transition

As Apple is slowly transitioning from x86 architecture to ARM in their laptop/desktop lineup, you can too with the bootloader. Is it wise to create bootable USB with OpenCore and slowly tweak it. Once everything is working, you can copy the bootloader files from USB to your main drive EFI partition.

It is also wise to transition from known state rather then a new one. If you upgrade the bootloader, kexts and/or your OS, you don't know what caused the problem and you will have to invest more time to debug the problem. So try to make your OpenCore installation working on your current setup.
