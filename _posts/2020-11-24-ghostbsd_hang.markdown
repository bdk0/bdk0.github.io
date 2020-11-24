---
layout: post
title:  The Day I Couldn't boot GhostBSD
date:   2020-11-24 16:47:00 -0400
---

I wrote in a previous post about my experiences using GhostBSD on my primary desktop. For several months everything was going great; I especially liked the update station app, which pops up in my Xfce panel whenever there are new updates to install and gives me GUI showing what packages need to be upgraded or reinstalled. Then I can just click 'install updates' and everything automatically downloads and installs, prompting me for a reboot when it's finished. You really can't put off the reboot much, some things really do stop working until you do. I especially like how GhostBSD updates the core OS and packages in the same manner, there's no separate tooling for upgrading the base OS separately from the packages. Update station is all you need. Another great feature is a little checkbox to "Create Boot Environment Backup" which really saved my butt recently.

It was September 28th and I was about to head to bed. Update Station had been trying to get my attention for a couple of days, but I'd been looking the other way because I was in the middle of some stuff and didn't want to reboot. Finally, before going to bed, I told it to do its thing and turned off the monitor. The next morning when I turned my monitor on, I saw the upgraded were done and GhostBSD wanted to reboot. "Sure, go ahead" I told it. I'd applied updates at least a dozen times for GhostBSD in the few months since I'd been using it, and never had an issue; but that streak was about to change. Upon boot I was greeted with:

~~~
OpenRC 0.41.2 is starting up FreeBSD 12.2-STABLE (amd64)

Press I to enter interactive boot mode

* Caching service dependencies ...
* WARNING: abi has started, but is inactive
* Starting the System Clock Adjuster (UTC) ...
~~~

"Cool, new OS version, I didn't even realize FreeBSD 12.2 was out yet" was my first reaction, and "Hmm, whats taking so long" was my second. I waited and waited and finally came to the conclusion that my system was hung. I tried a few different things, including spamming that "I" key mentioned during boot up, but couldn't get anything different to happen. Finally, I booted into the boot manager and told it to boot from the backup boot environment labeled as being from September 28th, and was relieved to be greeted by my familiar desktop. I used backup for a few days to get my work done, and I opened a [thread on the GhostBSD forums](https://forums.ghostbsd.org/viewtopic.php?f=58&t=1754) and got a response a few days later from a GhostBSD developer telling me I had "The VirtualBox Kernel Problem". That was good news to me, since if the developers had a name for the bug I was experiencing, that meant they knew what it was and a fix should be forthcoming. When more updates were available in the update station, I downloaded them greedily,rebooted back to the normal non-backup boot environment, and promptly hung in the same spot. Back to the backup I was. More upgrades became available over the next month and I installed those as well, with no improvement. Eventually I pinged the thread only to be told that the problem was resolved and I just needed to upgrade... but I already had the latest upgrades-- or so I thought.

Eventually I had some time off work and decided to finally get to the bottom of this. My plan was that since I was booted off of a backup boot environment, I would first need to mount the 'initial' boot environment somewhere and troll through and find the spot it was hanging in. I quickly found that the OS had already mounted the 'initial' environment under /mnt for me, which was convenient. I installed 'beadm' anyway so I could manipulate my boot environments and read up on what they actually are, and then my mistake hit me. I'd been thinking of my backup boot environment kind of like a backup kernel that you leave when you compile a new one in case your new one doesn't work. Actually, however, its a bootable clone of a ZFS snapshot of the entire filesystem. So when I was booted in my backup and installing updates, I was installing them into this clone, not the 'initial' environment. So every time I tried to boot 'initial' I was still booting the same snapshot that had the issue all along, here I was installing updates into my backup-2020-09-28 boot environment and expecting them to affect my 'initial' environment. Duh. Further, my backup-2020-09-28 environment now had all the latest and greatest updates installed including the upgrade to FreeBSD 12.2, and was working fine. My issue was already resolved, I just hadn't realized it yet.

My first thought was that I'd need to figure out some way to get 'initial' to boot so I could install the upgrades there, but after reasoning a bit about boot environments and ZFS snapshots, I decided there wasn't anything special about 'initial' and renamed it to 'broken', then renamed my backup-2020-09-28 environment to 'initial' and made it active. After that, on reboot, everything worked fine again without having to choose a backup environment in the boot loader. ZFS (and the GhostBSD devs) save the say and we all live happily ever after.




