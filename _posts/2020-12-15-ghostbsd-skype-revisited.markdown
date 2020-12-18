---
layout: post
title:  Trying the Skype Web Client on GhostBSD
date:   2020-12-15 16:47:00 -0400
---
A couple of months ago, I wrote about my experiences with running the [Linux Skype Client in a Debian VM under FreeBSD](https://bdk0.github.io/2020/10/30/freebsd_skype.html).

In summary I have everything working pretty well for my uses, with one main problem-- since I run Skype on a VM and forward the GUI via X11, I've never managed to be able to share my screen with whomever I'm talking to. My workaround has been to have them share there screen and walk them through whatever it is I want to show them. Its not something that comes up more than once every few weeks at $WORK, but it is an annoyance that I've been looking to resolve. I'm thinking that If I installed a full X11 system in my Debian VM and used the desktop inside the VirtualBox window, I'd at least be able to share that screen and open a terminal or whatever there to share. I've not been ready to try that route yet though, I prefer just having a light command line VM and forwarding my applications to my host's X11 via ssh as opposed to a 'desktop in a window' VM.

One of the first things I had tried a year ago was to use the [Skype Web Client](https://web.skype.com). I had been unsuccessful originally but decided to give it another shot as I've seen others online claiming it works for them. Chromium was still a non starter. I'm able to log in but the 'call' button is grayed out with a mouse over tip informing me that audio and video calls are unsupported on my browser. I downloaded a UserAgent switcher and pretended to be Chrome on Linux and even Chrome on Windows, but with no change. Based on my reading, I believe the issue is that Skype uses some codecs that are not included with the open source Chromium browser, but are only available in Chrome.

I next moved on to Firefox. Since Firefox isn't supported by the Skype Web Client, I had to again download a UserAgent Switcher and pretend to be Chrome. This time I was actually able to log in and make a voice call. Previously when I tried this same thing, the audio quality was choppy to the point of being unusable, but this time I was able to successfully make a voice call and it sounded pretty good. I'm not sure if the difference is due changes in the software or operating system in the past year, or due to hardware (Last time I did this I was on FreeBSD 12.1 on a Thinkpad T450, now I'm on 12.2 on a Dell Desktop). In any case that much works now.

It was with great anticipation that I attempted to share my screen, and I was excited when a dialog popped up showing me all my X11 windows and asking me whether I wanted to share my whole monitor, or just a window. It even had previews of my various windows, but alas, upon choosing to share my screen, nothing happened. The remote end never saw a screen appear, and on the sharing end, I couldn't even tell it to 'Stop Sharing', it was like nothing ever happened. My guess is that the screen access API is a bit different in Chrome and Firefox, and making Skype think I'm Chrome for the benefit of letting me make calls, it also breaks the ability to Share screens. Too bad.

I wasn't quite ready to give up yet though, I'd earlier read a thread on the FreeBSD forums about [Running the Google Chrome Linux Binary on FreeBSD](https://forums.freebsd.org/threads/linuxulator-how-to-run-google-chrome-linux-binary-on-freebsd.77559/) by using the Linuxulator Linux emulation layer. I had been under the impression that the Linux emulation in FreeBSD was outdated, only emulating a rather old Linux kernel, so this was something I'd never even considered in my quest for a usable Skype, but it appears some great strides have been made recently.

One evening I sat down and ran through the steps given in that thread for setting up an Ubuntu Linux environment and running Chrome in it. Despite the fact that I'm running GhostBSD, not vanilla FreeBSD, pretty much everything worked the same

- The first gotcha I ran into was that the command 'service linux start' returned an error, but apparently it wasn't needed-- I planned to go back later and figure out the GhostBSD alternative once I'd run through the procedure, but after running the rest of the steps, it turns out nothing more was needed.

- The second gotcha was that GhostBSD already had some file systems mounted under /compat/linux and even though I removed them from fstab and ran 'mount -al' as instructed, I was getting weird errors that based on the thread, other people also had when they didn't have the mounts set up properly. I eventually figured out I needed to manually unmount, then run 'mount -al'. I also needed to manually chmod /compat/linux/dev/shm for some reason, even though the permissions aready in the fstab.

- Third, I find I need to manually run 'mount -al' and the chmod again after every boot, something isn't starting up correctly on boot up, though I haven't spend any time investigating what exactly I'm doing wrong

With those items out of the way, however, I was soon staring at a terminal full of 'unsupported system calls' and a seemingly functional Google Chrome Window. A quick visit to Youtube convinced me that sound was working and that everything seemed servicable, and it was off to web.skype.com I went.

I successfully logged into Skype  and sent and received chat messages, but was not able to send or receive calls. If I called the BSD system, I just never got any incoming call. If I tried to make an outbound call It would try for a bit and then complain about a weak wifi signal, which is odd, since I'm on a wired ethernet with pretty good connectivity to the internet. I tried a bunch of stuff without much luck, and the suddenly, I managed to make a call and, after further fiddling with microphone settings, I had working bidirectional audio. Again, I gathered up my courage to try to share my screen and Success! This time I was able to share my screen and view it on the remote end. I hung up and tried again. Failure again. I tried again, same thing. And Again, and Again.

I had a few more successful call connections and many more failures. I tried to A/B it to figure out what made it work, but was unsuccessful. It seems random, but if I try a dozen times or so it seems to work eventually, so kinda success? I can't rely on this to answer calls since it won't reliably ring, so I'm sticking with my Skype client in my Debian VM for day-to-day usage, but I figure whenever I need to call a team mate for a pair debugging session or a show-and-tell , I'll be able to login to the web client and spam the dialler a few times until a call goes through, so I'm at least in a better position than I was a week ago as far as screen sharing.

Now that I had the Linuxulator working, I did get the bright idea to try to run the Linux binary (non web) client in it, but no luck, it aborts early in the startup sequence, but it does appear that progress is being made with Linux emulation and who knows, maybe with FreeBSD 12.3, I'll finally have this working.



















