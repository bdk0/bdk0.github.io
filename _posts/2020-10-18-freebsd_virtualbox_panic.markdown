---
layout: post
title:  FreeBSD Suspend with VirtualBox Running
date:   2020-10-18 16:47:00 -0400
---

I often have a couple of Virtual Machines running on my FreeBSD laptop. The main one is a Debian Linux VM that I use to run a couple of pieces of software that don't support FreeBSD and that I need for work. I pretty much have this running 100% of time, since one of the things I use it for is Skype.

When I first set up my laptop, I was happy to get Sleep / Resume working. And then one day it stopped working, I could suspend fine, but I'd get a kernel panic on resume. Not fun. After some troubleshooting I discovered that the panic occurred if I had VirtualBox running. I was unable to find a fix for this, so a workaround would have to do. Suspending all my running VMs before Sleep works, but its kind of tedious to have to remember to suspend them before closing the lid, and then restart them after, so my goal was to script it to automatically suspend the VM when I closed the lid, and bring it back up on resume.

After some trial and error, the following changes did the trick.

- In /etc/rc.suspend I added the lines

~~~
/usr/local/bin/VBoxManage controlvm debian-vm savestate 
/bin/sleep 3
/bin/pkill VirtualBox
/bin/pkill VBox
~~~

- In /etc/rc.resume I added the line
~~~
/usr/local/bin/VBoxManage startvm debian-vm --type=headless
~~~

My VM, obviously is called debian-vm. I run it headless since I don't want a GUI on my desktop, I use it by ssh'ing into it and forwarding X with -X. If you wanted to be a bit more clever, you could change the command  to look up all the running VMs and suspend them all. The sleep and pkill on suspend is kind of hacky, but I found it was necessary to reliably not panic. I ended up also adding some pkill -9 for those two processes later on just to make sure.

Since I wanted the VM always running, I also added the startvm command to /etc/rc.local and the savestate command to /etc/rc.shutdown

No more annoying kernel panics on Resume.

