---
layout: post
title:  Notes from NetBSD - Part 2
date:   2021-01-06 16:47:00 -0400
---

I failed at NetBSD.

At least my first attempt to use it as a work environment went somewhere between fairly bad, and quite bad. My plan, as I wrote in the previous post was to leave my desktop booted into NetBSD for the full work day to use for everything except email and skype, which I'd access via my FreeBSD laptop sitting next to my workstation.

I made it about two hours.

Things started off alright, I started the work day by VPNing into my work network with **openconnect** and opening a couple of xterms ssh'd into my GNU Screen session for IRC and emacs. Then I opened a Firefox and started browsing through my JIRA tickets.

The Firefox on NetBSD self identifies as 'Nightly', so I guess it was built from a nightly build of Firefox instead of a release, and at least for me, its pretty glitchy. I often get partial page redraws, with random data from previous pages left behind on part of the page and other artifacts. Also sometimes it will completely refuse to go to a new URL and I have to quit and restart. I was considering maybe trying to install the Firefox ESR package instead, but then I stepped away from my computer for about twenty minutes while I was on a call.

When I came back, I clicked my panel launcher to open an xterm and nothing happened. Hmm. I also couldn't launch a new Firefox. Click the button and nothing happens. Luckily I already had an xterm open on another workspace, and that seemed to be working so I tried running 'xterm' from there, at least now I could see the error message:

~~~
invalid mit-magic-cookie-1 keyxterm: xt error: can't open display: :0.0
~~~~

I vaguely remember some stuff about mit magic cookies and .XAuthority from way back, so I decided to google the error to refresh my memory. Immediately Firefox (Nightly), told me it had upgraded in the background and needed to restart, then quit, and of course when it tried to restart failed. So I googled the solution on my FreeBSD laptop instead.

I found many similar error reports with various fixes, but none of them seemed to apply for me, and none of the obvious stuff fixed it. I'm sure there's an easy fix and I was just overlooking something, but I was on the clock and had other things I needed to work on, so I called my experiment a failure and booted back into GhostBSD.

Most of the similar mit magic cookie issues I see involve accessing X remotely, and its pretty weird to see it on a local connection and to have it just happen suddenly when previously the logged in session was able to create X windows just fine.

It will have to be a mystery for another time though when I have some free time to play around more with NetBSD. 




















