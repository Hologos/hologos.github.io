---
layout: post
title: How to use 2 external monitors with Macbook Air M1 using DisplayLink
categories: [ macOS, macbook, m1 ]
---

That's right, I bought myself a Macbook. My hackintosh was getting old and I really strugled with my daily workflow so I decided to jump on the hype train of Apple's **amazing** M1 SoC. And I have to say, the rumors were right.

What I didn't know was that Macbook Air 2020 M1 is limited to **one** external display only. The same goes to Macbook Pro M1 2020 13-inch and Mac Mini M1 2020 (all 1st gen M1 computers). I really needed to use 2 external displays plus the builtin display.

I started to search the internet and found a lot of articles about it but no one was trying to find the solution, they just informed people that this is the way it is.

_Not for me._

## DisplayLink technology is the solution

I read about a technology called [DisplayLink](https://en.wikipedia.org/wiki/DisplayLink) (don't confuse with DisplayPort). It creates a virtual graphic card (software driven) and using DisplayLink compatible hub, it can connect multiple monitors. And it worked.

**What you need:**

- DisplayLink software compatible with macOS such as [DisplayLink Manager Graphics Connectivity](https://www.synaptics.com/products/displaylink-graphics/downloads/macos)
- USB hub / docking station compatible with **DisplayLink technology** such as Dell D3100, Dell D6000, HP USB-C/A Universal Dock G2 or some i-tec capable one

I found a deal on Dell D3100 so I got that one. The only downside is that it doesn't support power delivery using the USB-B to USB-C cable. You have to power your mac using the other port. It is just a matter of how much you want to pay for it.
