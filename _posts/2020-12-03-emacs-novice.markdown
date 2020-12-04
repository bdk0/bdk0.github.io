---
layout: post
title:  Celebrating Twenty Years as an Emacs Novice
date:   2020-12-03 16:47:00 -0400
---

I first decided to learn Emacs in 2000. At the time I'd been a regular Linux user for about five years, and had gone through a Computer Science degree, working mainly through a terminal to a Unix system. I knew the very basics of Emacs and vi, but always used a simple text editor like pico on the terminal, and an X11 GUI IDE or text editor when editing code locally. I actually don't recall what IDE I was using for coding on Linux at the time, which goes to show how little it mattered.

At the time I was mainly working on an Open Source project in Java, which ran on Windows and Linux and was related to MIDI and music. I mostly developed on Windows since that's where most of my other MIDI software ran. I had the idea of migrating this aspect of my computer usage to Linux, hence the Open Source project. On Windows I mainly just used edit.com in a DOS Window.

A second motivation for writing this Java MIDI program, was to learn Java, I'd just graduated college and was finding some disconnect in the skills I saw in job postings and the skills I had at the time. Java was definitely in demand around the year 2000. Learning Emacs was another aspect of this. I'd hoped to develop software for Linux or Unix, and Emacs and vi were what the professional "Real Linux Developers (TM)" used-- or at least that's what I read online.

I figured to be more productive and not embarrass myself, I'd need to be a competent user of one of those two editors. I'd never liked vi, so Emacs it was. I remember starting out going through some tutorials, and trying to memorize some basic keystrokes, and just changing over for writing code, and for twenty years, I've never looked back.

My first real developer job was indeed working in a Linux environment, and I knew I'd made the right choice; everyone around me coded in either vi or Emacs, not a lesser editor to be found.

In my first few years using Emacs, I ran the X11 GUI version, and customized it a fair bit, even going as far as hacking together some elisp code to do some more advanced jumping between declarations and definitions in my C++ code that took into account the way we structured our code and branches. I used Emacs to write my code, to compile it and fix the errors, and to debug in a gdb buffer, never leaving the editor. My .emacs file grew longer and longer. I still only used a tiny fraction of Emacs's functionality, but I had everything I need.

At some point I changed from running Emacs as an X11 application to running it as a terminal application under GNU Screen. This was useful as I started working some days from home. From home I could just ssh into my work PC, reattach the screen session, and continue exactly where I had left off-- not just as far as Emacs was concerned, but any other applications or shells I had running in other windows.

During this time, my usage of emacs actually simplified. More and more I found myself editing in emacs, but then using another terminal under Screen to compile my code or run gdb, using grep on a command line instead of multi-file finds in Emacs. Over time I moved from an "Emacs as the IDE" idiom to a "The OS is the IDE, Emacs is the text editor" idiom, which is still how I work and suits me well. When I changed jobs a few years later, I didn't even bring my .emacs with me, by now there were just a handful of non-default settings I really cared about.

Twenty years later I'm still only really comfortable in an editor with Emacs keybindings, but I feel guilty pretending to be an Emacs user. I use Emacs as a basic text editor, and on occasion, google how to do something more complex. I remember reading some advice on interviewing and hiring which was to make sure to figure out if someone really has "Ten years of experience in X", or has "One yar of experience in X, ten times.". I'm definitely in the "One year experience X times" when it comes to using Emacs, where by now X=20.

Of course I knew how much more complex and powerful Emacs was compared to what I was using it for, but I was only really driven home a couple of weeks ago, when an 'Emacs user survey' was posted on a forum I read, which I decided to take. Some of the questions were pretty eye opening, "What package manager to you use with Emacs", Uh,I do have one third party .el which I downloaded with wget and 'require'd in my .emacs, does that count?

Its not that I regret being a twenty year novice of my text editor, The 'OS is the IDE' model I referred to earlier suits me, and while I've tried for a period of time bringing more of my functionality back into Emacs, I haven't found all that much efficiency or synergy over just using emacs to edit text, and a separate terminal window for building and debugging. I will at times run my terminal in an ansi-term buffer, but that's about as integrated as I get these days.

A few months ago I tried moving over to Visual Studio Code for a few weeks, it had some nice features, at least on paper, integrating with the docker environment which I had moved to for building code. It didn't last though, I used it long enough so I could feel I'd given it a fair shake, but more and more I found myself just opening Emacs for 'a quick edit' on a file.

Interestingly between the time I conceived of this blog post and posted it, I read something similar on a [blog I read](https://rubenerd.com/thinking-out-loud-about-vim/) written by a vim user. In his case, he at least feels like an 'intermediate' user of vim after all these years. I don't think I'm quite that far along in the Emacs journey.













