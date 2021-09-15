---
layout: post
title:  Fixing X11 Screen Tearing on my Thinkpad P15 w/ Void Linux
date:   2021-09-15 06:47:00 -0400
---

I noticed today I had some screen tearing on my new Thinkpad P15, noticeable mainly when scrolling in Firefox. Since I had the same issue originally on my GhostBSD desktop, and both systems use Nvidia GPUs, my first instinct was to attempt the same fix, however on my laptop, this gave an error: (line breaks added here for formatting)

```
$ sudo nvidia-settings --assign CurrentMetaMode="nvidia-auto-select +0+0 \
  { ForceFullCompositionPipeline = On }"
ERROR: Error resolving target specification '' (No targets match target
specification), specified in assignment 'CurrentMetaMode=nvidia-auto-select
+0+0 { ForceFullCompositionPipeline = On }'.
```

A bunch of searching on line found this to be a common problem, but most posts were just pointing around at other posts with similar issues, and not providing useful information. I did find a forum post eventually that pointed out that since on systems such as my laptop, the Nvidia isn't the primary video card, and is mainly used for offloading more GPU intensive applications, so whatever fix works, it isn't going to be at the Nvidia level.

Another forum recommended using a compositor, and specifically mentioned `picom`, Running picom didn't make any difference, but the man page provided a few options that seemed relevant. What ended up working for me was:

```
picom --vsync --backend=glx
```

I'll have to add this into my startup somewhere, but just leaving this running in a terminal does seem to prevent the screen tearing from occurring.
