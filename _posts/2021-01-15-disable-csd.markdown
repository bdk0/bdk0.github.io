---
layout: post
title:  How to Disable Client Side Decorations on FreeBSD / GhostBSD
date:   2021-01-15 16:47:00 -0400
---
**Client Side Decorations**, where the application draws its own window decorations instead of the Window Manager, is just a terrible idea. Why go through the trouble of picking out a window manager you like, configuring and theming it exactly how you like it, only to have some random application decide it knows better than you how you want your windows to act. Until recently the only application I used that insists on client side decorations is my PDF viewer, **evince**. Unlike other windows, it doesn't get an OpenBox title bar, so not only does it look out of place on my desktop, but I don't have my normal set of OpenBox right-click-menu options. Honestly I need to open a PDF so infrequently that the annoyance was just below my threshold of finding a different application for PDFs or finding a solution.

But then recently, GhostBSD popped up a notification that it had new updates for me, which included an upgrade to Xfce 4.16. Unfortunately the Xfce developers decided to force client side decorations on all their users in this version, without even an option to disable them and re-enable sanity. I didn't want to forgo the upgrades, so I finally spent the time finding a solution for disabling CSD system wide.

Client side decorations are a "feature" of gtk-3.0, and have been used in Gnome for some time, so some kind soul created `gtk3-nocsd`, which provides a replacement library to `LD_PRELOAD` to disable client side decorations and works on many Linux distributions. While there is no port / package for FreeBSD, its quite easy to download and build from source.

First, download the source, from [this Github repro](https://github.com/lairevett/gtk3-nocsd/tree/freebsd). There are a few different copies of it available on Github, but this one has the necessary compile fixes for FreeBSD already merged. The instructions provided in the README pretty much work on FreeBSD, there are only a couple of tricks:

 First, the package names it lists as pre-requisites don't exist under those names on FreeBSD. You won't need any of the 'devel' packages, since the normal gtk3 packages include the necessary header files. If you already have Xfce or similar environment running, you probably already have all the gtk development files you'll need. On my GhostBSD system, they were already present. There's no pkg-config package on FreeBSD, that is provided by the `pkgconf` package, but again, I already had that installed. The only package I needed to install was `gmake` since the tools relies on GNU Make to build.

 Second, download and extract the source from Github, enter the directory, and type `gmake`. It spit out a few warnings for me, but did successfully build. It creates both a binary and shared library. To test that it works, run a program using the binary as such:
```
./gtk3-nocsd /usr/local/bin/evince
```
If all is well, the program will pop up with normal window decorations. To make this the new default system wide, put the shared library somewhere and add these lines:
```
export GTK_CSD=0
export LD_PRELOAD=/path/to/libgtk3-nocsd.so.0
```

To somewhere where they will always be set when X applicatons run. In GhostBSD I added them to my `.xprofile`, but depending on how you start X, you might need to put them somewhere different. Important thing is to have those environment variables set whenever you launch a program.

Quick and easy, and sanity is restored.



















