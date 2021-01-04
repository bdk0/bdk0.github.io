---
layout: post
title:  Notes from NetBSD
date:   2021-01-03 16:47:00 -0400
---

Greetings to all my (non-existent) readers from **NetBSD**, where I am currently writing this blog entry. I originally installed NetBSD 9.0 on a spare drive in my main desktop (which normally runs GhostBSD) months ago to play around with, but after spending a few hours with it, then booting back to GhostBSD the next day for work, I hadn't revisited it until now.

I had some time off from work this week and used a small amount of it to upgrade my NetBSD install to 9.1 and finish configuring it to something similar to [My normal desktop configuration](https://bdk0.github.io/2020/12/30/my-unix-desktop.html) that I use on other OS's:. 

{% include image.html url="/assets/netbsd_desktop.png" description="You can tell its NetBSD by the wallpaper." %}

The following are my notes and observations so far.

## NetBSD 9.0 -> 9.1 Upgrade

Reading about the upgrade process made it look trivially easy-- just run **sysupgrade**, answer a few questions about config file merges and it will take care of the rest. In practice though, as a NetBSD n00b it didn't go quite so smoothly.

- After installing sysupgrade and trying to run it, and realizing it wasn't in my path, I managed to get it running with the command:
~~~
sudo /usr/pkg/sbin/sysupgrade auto \
    http://cdn.NetBSD.org/pub/NetBSD/NetBSD-9.1/amd64
~~~
However it quickly exited with a *404* error when trying to download the first file. It was looking for a **.tgz** file but the files on the site were **.tar.xz**. I had to edit one line in the **sysupgrade** script:

~~~
shtk_config_set ARCHIVE_EXTENSION "tgz"
~~~

Replacing the **tgz** with **.tar.xz** fixed the problem and everything downloaded and extracted, erroring out near the end when trying to run **postinstall** and **etcupdate**. I quickly discovered that those files were also in **/usr/pkg/sbin** and that manually using the full path when running **sysupgrade** isn't enough. Adding the directory to my path took care of the problem.

It did error out once more,  but helpfully gave me the exact command I needed to run to fix it, and afterwards the process completed smoothly. After the upgrade was complete, I celebrated by upgrading all my packages and rebooting, and was relieved to be greeted with NetBSD 9.1

## A Few Small Problems (And Their Solutions)

While setting up my environment after the upgrade, I had to troubleshoot a few problems

- First every now and then all my commands would start failing with error messages like "*Too Many Open Files*". I discovered that Firefox had a ton of file descriptors over and that killing it would resolve the issue for a bit before it came back. Based on (This advice)[https://minux.hu/solving-too-many-open-files-problem-netbsd] I tried increasing my **ulimit -n** from the default of 1024, to around 3400 and the problem has not occurred again. I'm still not sure if this is a descriptor leak in Firefox, and I'll eventually hit the new maximum, or if Firefox, for some reason, just needs to have 1000 file descriptors open.

- While trying to install (Jekyll)[https://jekyllrb.com/] so I could work on this blog, I was getting SSL certificate errors trying to download the Ruby Gems. While trying to troubleshoot, I attempted to use **curl** to download a certificate and had a similar error there. I eventually [found](https://www.reddit.com/r/NetBSD/comments/dveadk/ssl_errors_with_git_and_curl) I needed to run **mozilla-rootcerts install** and after that it worked.

- I like to run Xfce with the *Greybird* theme, which gives me the nice dreary looking dark grey panel you see on my screenshot above, however I was unable to find the Greybird theme anywhere in pkgin. I ended up copying the directory */usr/local/share/themes/Greybird/* from my FreeBSD machine to *~/.local/share/themes/Greybird/* on my NetBSD system, and then was able to select and use the theme.

- I wanted to download some add-ons for Firefox, but when I went to the extension page in Firefox to try to download some, it gives an 'Unsupported Platform' error. Luckily, the platform is just communicated to the server as a URL parameter, and I was able to edit it in place, tell them I was FreeBSD, and run the addon I needed fine.

## A Few (as of yet) Unsolved Problems

- While FreeBSD and GhostBSD have no problem with it, NetBSD doesn't seem to like my Plantronics USB Headset. I'm not able to get any audio from it, and I see a message in **dmesg**: *uaudio1: autoconfiguration error: ignored audio interface with 2 endpoints*. Fortunately I was able to get audio output (though not input) through the built in sound card, so at least I have that much going for me. I've posted a plea for help on the [UnitedBSD Forum](https://www.unitedbsd.com/d/353-help-getting-microphone-and-smartcard-reader-working) but haven't yet figured anything out.

- I also have not yet managed to get my USB SmartCard Reader recognized. As on Free/GhostBSD and Linux, I installed the **opensc** and **ccid** packages and ran **pcscd -df** from the command line, but it never sees my reader or card.

- I like to set up my desktop to have certain hotkeys that run scripts which open an application and place it at a certain location and size on a certain desktop. I accomplish this with calls to the **wmctrl** tool, but find these scripts don't work on NetBSD. Further investigation shows that **wmctrl -l** does not list all the available Windows open on my desktop, just some of them, so this is probably related. I haven't yet been able to discern a pattern or find a cause.

- My desktop has Nvidia graphics, and it seems much smoother on GhostBSD. If I'm not mistaken, Ghost/FreeBSD has a binary driver from Nvidia for this, while NetBSD uses Noveueau, which is probably the cause. Currently I have visible screen tearing and some temporary redraw glitches-- as I drag one window in front of another the redrawing of the background window and motion of the dragged window is slow and flickery. I haven't spent any time yet seeing if this can be improved yet.

- NetBSD seems slower and a bit laggy at times compared to GhostBSD on the same system, but its hard to say how much of this is NetBSD, and how much is that I'm running GhostBSD from an SSD and NetBSD from a Hard Drive. I can't blame the OS for slower loading of programs, but doing things that shouldn't hit the disk at all can seem slower.

- I've had one full OS hang in my first day of using it, I was trying to change my Xfce background image when the mouse and everything else hung. I had to hard reboot and on startup, NetBSD has a kernel core dump for me.

- I've also managed to 'soft hang' the system a few times trying to quit X or change to a terminal and getting just an unresponsive black screen . I don't think its really hung here and have the feeling if I had **ssh** running I would have been able to log in that way still, but as it is I had to reboot to get it back. I"ve had similar issues with Debian GNU/Linux at work and even on FreeBSD on my laptop with an external monitor, so its not just NetBSD that has trouble with this.

- After leaving FireFox open overnight and coming back the next day, it was super slow and stopped reliably loading pages. I was unable to close it from the GUI and had to pkill and relaunch it.

- I'm used to using VirtualBox to virtualize a couple of guest Operating Systems that I use for work, but that doesn't exist on NetBSD. My preliminary reading  points to qemu+nvmm as a possibility for this, but I haven't researched it enough yet or tried it out.

## Next Steps

I've had a bit of a rough start with NetBSD, a few crashes and a few things I need not working, but I plan to stick with it and see how things shake out. If nothing else its a fun experience to try different things. 

I'm back to work (from home) tomorrow after my holiday break. I plan to keep my primary desktop booted into NetBSD and see how far I get with it.  Ninety percent of my job at ${WORK} consists of mucking about in terminals on various Linux Servers, which I can do fine in NetBSD since I have **openconnect** working to connect to my work VPN, and **ssh**. Most of the rest involves working in a webbrowser, for which the Firefox Nighly provded by NetBSD should be fine. Wireshark and audacity which I often need are both available, and everything else I'd use would be command line tools.

The two things I can't yet do that I need for work is to access my email (Need SmartCard support working) and Skype, which we use for all our team calls (Need a working headset / microphone). I did test the Skype web client under NetBSD, by setting the UserAgent to Linux Chrome and was able to make a call and hear the remote side (Couldn't send audio though, since I don't have microphone support working). Hopefully If I can get that working, this will suffice for work calls, otherwise I can try out running the Linux Skype Client under a virtualized Debian, which is how I do it on other BSDs.

Luckily, I keep my FreeBSD laptop on the desk next to me in my home office, so my plan for tomorrow is to bring up my email and skype on that and use NetBSD on my workstation for everything else. If things seriously go sideways, I can always boot back into GhostBSD. Fingers crossed.



















