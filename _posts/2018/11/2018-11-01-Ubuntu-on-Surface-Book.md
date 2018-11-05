---
layout: post
title: Using Linux at Work - Linux on Surface (Book)
categories: [Tips, Tools, Using Linux at Work]
tags: [Family, Linux Desktop]
fullview: false
comments: true
---

I use Surface Book (first generation) in my work. It is a smooth laptop with Windows: high resolution, high speed CPU, great GPU for StarCraft II, etc. But lots of my work need Linux enviornment. Of course, I could remote with SSH, VNC, etc. But it is not quite easy when there is no good network. 

Need a way to install Ubuntu on it. In the meanwhile better keep my Windows for the situation when I have to use Windows, e.g. VPN, Skype for Business meetings, Microsoft Teams meetings, etc.

After did some research on Internet, I notice that actually from 16.04, better from 18.04, there are already some drivers for Surface Book, I could just install it directly and setup grub to have dual boot! To keep the Windows as much as possible, I used a small USB flash disk as the boot device and install GRUB over there to make it more flexible.

So far after almost a week usage, 90% of my user scenarios are satisfied. The missing pieces are:

1. No touch screen
2. Bluetooth and WiFi are not stable. Sometimes only reboot could fix it.
3. Bluetooth mouse could only bind to Ubuntu or Windows. Using the other OS will have to use touchpad.
4. No sleep. Battery will be used up, if just close the laptop.
5. If resize hard drive patition to install Ubuntu, the Windows BitLocker will be triggered and have to recover. 

Will see if anything else I could find. But the current XUbuntu 18.04 is awesome on my Surface Book!