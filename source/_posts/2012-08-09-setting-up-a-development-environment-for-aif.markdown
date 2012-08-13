---
layout: post
title: "setting up a development environment for AIF"
date: 2012-08-09 04:17
comments: true
categories: AIF archlinux
---

I've spent the last couple of days setting up a development environment for
working on AIF. In this post, I'm going to walk you through the setup I came up
with. If you have any suggestions, please let me know -- some of this involves
stuff that I'm not incredibly well-versed in.

## Prerequisites ##

First off, I'm assuming you're working on a machine running
[arch](http://archlinux.org). You'll also need a copy of the
[latest install media](http://www.archlinux.org/download/) and
[git](http://git-scm.com/), if you don't already have that installed.

You'll need to have a local checkout of both AIF and libui-sh, a dependency of
AIF:

```
$ git clone https://github.com/jdodds/aif
$ git clone https://github.com/Dieterbe/libui-sh
```

## Setting Up a VM ##

I'm using [qemu](http://wiki.qemu.org/Main_Page). As far as I can tell, you
should too. The setup required is quite a bit more involved than the setup
for (for example) [virtualbox](http://www.virtualbox.org), but it's a much more
suited tool for what we need to do.

Install qemu:

```
# pacman -S qemu
```

The first thing you'll need to do is create a disk image for qemu to use:

```
qemu-img create -f qcow2 ~/vms/aif-testing/base.qcow2 8G
```

If your machine supports [kvm](http://www.linux-kvm.org/page/Main_Page), you'll
want to load both the `kvm` and either the `kvm-intel` or `kvm-amd` kernel
modules:

```
# modprobe kvm && modprobe kvm-intel
```

You'll probably want to stick those in `MODULES` in `/etc/rc.conf`. You'll also
want to add yourself to the `kvm` group:

```
# usermod -a -G kvm $USERNAME
```

And then either re-login or run `su -` in your terminal emulator of choice.

Now we should be able to start arch in our vm. I use a simple script:

{% include_code vm-aif lang:sh %}

Stick the above somewhere in `$PATH` and run it, and you should see something
like this:

{% img /images/arch-live-splash.png %}

Press `Ctrl-Alt-Shift-2` to get to qemu's "monitor". You can do a lot of stuff
from here, but for now you should at least know that:

+ `quit` quits
+ `savevm foo` creates a snapshot named foo
+ `loadvm foo` loads the snapshot named foo
+ `info snapshots` shows information about snapshots

Press `Ctrl-Alt-Shift-1` to get back to the boot screen, and boot. Once you're
in, your host machine is available to the vm as `10.0.2.2`.

## git-daemon ##

There are a few ways you could make your working copy of AIF and libui-sh
available to your vm, `git-daemon` is what I went with. If you have git
installed from arch's repos, you should already have it.

I have all the code I actively work on under `$HOME/workspace`. If you use a
similar setup, modify `/etc/conf.d/git-daemon.conf` to look something like:

{% include_code git-daemon.conf lang:sh %}

The `--user-path` option allows us to clone from the vm easily later.

Now, start up `git-daemon`:

```
# rc.d start git-daemon
```

And then over in your vm, you need to get some basics set up:

```
# pacman -Sy git binutils dialog make
# git clone git://10.0.2.2/~$USERNAME/path/to/aif
# git clone git://10.0.2.2/~$USERNME/path/to/libui-sh
# cd libui-sh && git checkout -t develop && make install && cd -
# cd aif && git checkout -t develop
```

### Take a Snaphot ###

At this point, you're pretty well set-up in your vm. Switch over to the monitor,
and use `savevm` to create a snapshot that you can reload later with `loadvm`.

## Sharing Packages With Your Host Machine ##

I ended up using [pacserve](http://xyne.archlinux.ca/projects/pacserve) to do
this. Here's my `/etc/conf.d/pacserved`:

{% include_code pacserved lang:sh %}

I'm now considering just mounting the host machine's `/var/cache/pacman` via nfs
or similar, but the code in the experimental branch of AIF assumes the above,
and it works pretty well.

At this point you could `loadvm` if you need to and:

```
# make install # assuming you're in the aif checkout on the vm
# aif -p automatic -c unofficial/jdd-generic-install-sda-using-host-packages
```

And everything should work. You should be able to reboot into a working arch
system. In fact, if you do this and it doesn't work, please let me know.

## (optional) Making AIF Available as a Package on the vm ##

This is really only necessary for using or working on`aif-test`, and is by far
the area where I'm the least sure of whether I'm doing stuff right. In the
"experimental" branch, there are two PKGBUILD templates in
`unofficial/pkgbuild-templates`. Here's a rundown of what you need to do:

```
# cd
# mkdir -p packages/{aif,libui-sh}
# cp aif/unofficial/pkgbuild-templates/PKGBUILD.aif packages/aif/PKGBUILD
# cp aif/unofficial/pkgbuild-templates/PKGBUILD.libui-sh packages/libui-sh/PKGBUILD
```

Now, edit the two PKGBUILDS, and set `_gitroot` to an appropriate value. This
should be the only required edit.

Next:

```
# cd packages/libui-sh && makepkg --asroot && cd -
# cd packages/aif && makepkg --asroot --nodeps && cd -
# mkdir repo
# repo-add repo/aif.db.tar.gz \
#   packages/libui-sh/libui-sh*.pkg.tar.gz packages/aif/aif*.pkg.tar.gz
# cp packages/{libui-sh,aif}/*pkg.tar.gz repo/
```

And now the `aif-test` scripts should run, and make use of your host's
packages. As far as I know, none of them are actually *working* entirely, and
this entire step is only actually necessary if AIF is a dependency on the vm
after the install finishes in order for `aif-test` to finish. I haven't read
enough of it to know quite yet.

It would also be unnecessary if AIF and libui-sh were still available in the
repos. I don't particularly like the PKGBUILD-template idea, even though it
works. If this actually is necessary right now, hopefully it won't be for long.

## Basic Development Workflow ##

+ Work on your host machine
+ `loadvm` in qemu monitor
+ `git pull`
+ `make install`
+ run the part of aif you want to test

## Conclusion ##

Well, that was a lot of setup, but it also helps keep the feedback loop short,
which is nice. If you've got any suggestions, I'd love to hear them!


## Updates ##

+ Updated on Mon Aug 13 10:05:47 EDT 2012 to fix typo pointed out by Mr Greem.
+ Updated on Mon Aug 13 14:27:49 EDT 2012 to fix erroneous groupadd command
  pointed out by Mr Greem.
