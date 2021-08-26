---
layout: post
title:  Skype Web on FreeBSD Finally
date:   2021-08-26 06:47:00 -0400
---
I've written [before](https://bdk0.github.io/2020/10/30/freebsd_skype.html) about various work-arounds I've used to access Skype from FreeBSD and GhostBSD. Adequate Skype connectivity is necessary for me to be able to use these as my daily driver, since thats what we use at ${WORK} to stay in touch.

Running Skype under a headless Debian VM and fowarding the screen via X and the audio via Pulseaudio to the host machine has proven pretty reliable, though high CPU utilization is the norm, and I didn't have the ability to share my screen. I've tried a couple of times to get the Skype Web client working, both in Firefox and Chromium as well as in [Google Chrome running with FreeBSD's Linux Emulation](http://bdk0.github.io/2020/12/15/ghostbsd-skype-revisited.html) with limited success.

Recently, however, when I gave it another shot in Chromium on my GhostBSD workstation, everything suddenly works! I'm not sure if its related to a recent Chromium update (I currently have 91.0.4472.164), or to something that Microsoft recently changed on the servers, but either way, I'll take it.

Not only do inbound and outbound calls work, but so does screen sharing! I can even use my webcam and show my face at work, though its a little glitchy from time to time. My webcam works fine under `pwcview` but it has some glitchy frames on all web sites under Chromium.

One odd thing with the Skype Web client is that periodically, perhaps once a day, it "Deactivates" my Skype session and pops up a notification that I need to activate it again. If I get a call while its not activated, it doesn't ring, but does pop up another notification telling me to reactivate it because a call is coming in. This is pretty weird-- its obviously still connected to the servers since it knows a call is coming in, so why do I need to 'Reactivate'? I've been meaning to look into a chromium plugin that will automatically refresh the page every twelve hours or whatever.

I've closed down my Debian VM on my desktop and have been using the Web Client now for the past two weeks with good results . I've also verified it works on my FreeBSD Laptop (Currently on 12.2), so its not just a FreeBSD 13 thing, though I don't have much mileage on it there.

Zoom has also been working pretty well for me under Chromium on GhostBSD for me lately, though I did have one meeting where incoming video died and I couldn't get it to re-enable. 


