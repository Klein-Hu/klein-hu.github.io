---
layout: post
title: ZSH and Oh-My-Zsh
categories: [Tips]
tags: [Linux, Tools, Shell]
fullview: false
comments: true
---

Last time using Linux/UNIX as daily work environment was still in 2004-2008. After 10 yeas, seems user experience on Linux has been improved a lot. My home laptop is on Ubuntu 18.04 and my daughter enjoys WPS on Ubuntu 16.04 every day.

## Why ZSH

Boring BASH!

I came back to Linux world from Windows in about 2 years ago. In Windows CMD, there is nothing good except case-insensitive file names. It is easy for lazy guys like me. So, I'm trying to find a similar experience on Linux. Trent, a fresh man in my team told me to take a look `zsh` and promised me it will be very impressive. 

He was right! In the very first day I got the following experience:

1. `cd aNythiNG <tab>`, if there is only one match, it will show up right away; if more than one possible matching, more `<tab>` will highlight the right one and you just need an `<enter>` to pick it up.
2. `git` command auto-complete
3. "Open with..." feature in zsh: in `.zshrc` add `alias -s log="less -MN"`. Next time when you say `/etc/log/Xorg.0.log`, the log file will be opened by `less` directly with parameter `-MN`. 

I could imagine there are more features in ZSH. But this was good enough for the very first day! Then from that day, all my Linux machines/VMs start using zsh.

## Oh-My-Zsh

Trent heard my experience the next day. He shamed on me because after the first day I was still not using "Oh-My-Zsh". WTF~

But he was right again..... Oh-My-Zsh is amazing! It gives me a better shell experience with many many themes. What I love so far are:

[af-magic](https://github.com/robbyrussell/oh-my-zsh/wiki/themes#af-magic)
![af-magic](https://cloud.githubusercontent.com/assets/124808/21915191/89dffcac-d97b-11e6-8b46-ea5fbddde02a.png)

and [Bullet Train](https://github.com/robbyrussell/oh-my-zsh/wiki/External-themes#bullet-train)
![Bullet Train](https://camo.githubusercontent.com/c5b0c78df1c3ca27bb2c5577114a92018bbdbee0/687474703a2f2f7261772e6769746875622e636f6d2f6361696f676f6e64696d2f62756c6c65742d747261696e2d6f682d6d792d7a73682d7468656d652f6d61737465722f696d672f707265766965772e676966)


## Reference

1. [Why Zsh is Cooler than Your Shell](https://www.slideshare.net/jaguardesignstudio/why-zsh-is-cooler-than-your-shell-16194692)
2. [Oh-My-Zsh GitHub Repo](https://github.com/robbyrussell/oh-my-zsh)