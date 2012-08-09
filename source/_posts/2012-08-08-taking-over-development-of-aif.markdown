---
layout: post
title: "taking over development of AIF"
date: 2012-08-08 23:05
comments: true
categories: AIF archlinux
---

[Deiter Plaetinck](https://github.com/Dieterbe) recently handed
[AIF](https://github.com/jdodds/aif), the [Arch Linux](http://archlinux.org)
Installation Framework over to me, I'm pretty excited about it.

There's a lot of work that needs to be done -- AIF is currently fairly
broken. I'm aiming to fix that, and it looks like some major restructuring will
be in order. There are some other popular installation tools around --
[Archboot](http://projects.archlinux.org/archboot.git/), and the current tools on
the install disks,
[arch install scripts](https://github.com/falconindy/arch-install-scripts). There
is also work being done on a
[port to python](https://github.com/bwrsandman/pyaif), which I'm keeping an eye
on.

So far, I have support for `grub-bios` and `syslinux` working as bootloaders. I
recently spent a couple of days setting up a development environment for making
testing AIF less of a pain, there will be a post describing that coming soon.

I'm interested in continuing work on AIF for a few reasons. For one, it's really
a pretty nice tool. The ideas that it's trying to implement are, at the core,
pretty solid in my opinion. It was also the default installation tool for arch
for quite some time, and people seem to miss it. In addition, I've been wanting
to start contributing back to the arch community for a little while, and I
happen to be working on some tools for making generation of customized Linux
install and live images easy that are partially built on top of AIF.

If you'd like to help out, patches, advice, and feedback are always welcome.
