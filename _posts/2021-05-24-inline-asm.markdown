---
layout: post
title:  Two Strange Bugs
date:   2021-05-24 06:47:00 -0400
---

Someone recently posted a link on an online forum to an article on the GCC Wiki entitled [Reasons you should NOT Use Inline Assembly](https://gcc.gnu.org/wiki/DontUseInlineAsm). The gist of the article is given in the below quote:

> Even 'minor' mistakes like failing to clobber memory or earlyclobber a register can result in problems that may not show up until well after the asm has been executed. And examining the asm output during a compile (-S) doesn't guarantee that the inline asm was written correctly either, since small and (seemingly) unrelated changes in surrounding code might someday cause the compiler to resolve the constraints differently.

This reminded me of a time when I was bitten by this exact issue, and was thoroughly confused until I realized what happened. This was back in my college days in the mid 1990's and I was working on some sort of game for DOS in Borland C++. I had a bit of inline assembly in there, probably for something like setting the graphics mode to Mode X, but that code had already been working for a while, so my mind didn't immediately jump to there. As I recall, I was having some unexplained crashes in some seemingly benign C code, located well after any calls to inline assembly.

My normal debugging method at the time, of commenting out parts of the code until it stopped crashing, wasn't converging on a solution, and after a while I discovered that putting a comment-- any comment- at a certain location in the code would stop the crash and everything would work fine. My first instinct was to blame the compiler, so I could check what differences there were in the outputted assembly with and without a comment. I quickly found that the register being used to store a certain pointer value was different between the two cases, which eventually led me to the true source of the issue. Earlier on in the code, my inline assembly function was called, and I was clobbering a register I shouldn't be. Borland C++ at documented rules about what registers a function could clobber and which it needed to save and restore, and I was failing to save one that I should have been. The compiler assumed that it knew what value was in a particular register at the time of the crash, only to be surprised that my assembly code had clobbered it.

Its an obvious bug in retrospect, but it through me for a loop at the time until I realized the problem as my mental model of how C++ worked didn't allow for "Adding a comment can affect the program's behavior."

--

Another bug I encountered around the same time was a "random crash" when writing my operating system. In 1996, I decided I wanted to write my own 32-bit Operating System in i386 assembly. It was pretty minimal, but it could boot up, enable protected mode, and run simple programs and multitask between them. To make development easier, I made it possible to run as a DOS extender as well as a stand alone OS. In this mode, you could run the OS Loader as a DOS Executable, and it would allocate a block of XMS Memory and the OS would run entirely within that block of memory, leaving conventional memory untouched. Then when shutdown, it would return to real mode and return to DOS. This saved me a lot of rebooting time during development since the OS was never self hosting, and development was done with an MS-DOS editor and TASM.

One day while working on my kernel I started experiencing random freezes in my OS, requiring a reboot. This wasn't anything odd, I'm sure I rebooted that poor 486 I was working on a million times while working on that project, but this crash didn't seem to be triggered by any new code I'd added or any specific thing going on, it was just... random. I'd have my kernel booted up, sometimes for a few seconds, sometimes for several minutes, and it would just hard lock up. Worse yet, and possibly a clue, this would even happen sometimes after I'd returned to DOS-- I'd boot up DOS, enter my OS, quit my OS, and then at some point later the computer would hang. I spent quite a while one evening trying to track this down. Eventually, by luck, I had rebooted my PC after a crash and was reviewing my code when the PC crashed yet again. But wait a minute, I'd never even run my OS since the last reboot. Turns out all this time the 'bug' I'd been unsuccessfully trying to track down was a CPU Fan that had stopped spinning. My computer was periodically overheating, but since I was working on an OS Kernel at the time, I was totally in the mindset of assuming it was something I was doing in code that was causing the problem.

These are two of my go-to examples I give when talking about "difficult" or "random" or "amusing" bugs with other programmers. In both cases, the bug was something I hadn't originally considered and was happening in a way that made normal debugging difficult, triangulating to a certain line in the code just wasn't working, because the bug wasn't in the particular line of code that was crashing, it was somewhere else in the program entirely, or in the second case, not in the code at all.













