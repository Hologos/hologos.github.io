---
layout: post
title: Stay away from macOS Catalina 10.15.4!
categories: [ macOS ]
---

This update seems to be [broken as hell](https://www.softraid.com/forum/showthread.php?tid=1299). What's the worst - the **Time Machine** is broken so there is no way back if you don't have full-disk backup (for bit-by-bit backup, check [this guide](https://hologos.github.io/upgrade-from-macos-mojave-to-macos-catalina/#backup)). Other than that, you will get constants kernel panics, you won't be able to copy files larger than 30 GB if it's a APFS volume. Also **ssh** is broken, but to be honest, who uses builtin **ssh** when we have [brew](https://brew.sh).

Until Apple fixes this, **STAY AWAY**!
