---
layout: post
title:  Making xfce4-terminal look like XTerm
date:   2021-01-18 16:47:00 -0400
---

Ever since I started using Linux and Unix systems in the mid-90's, I've always relied on either XTerm or urxvt for my terminal needs while in X. These have always worked well, and provided me with exactly what I want when I want a terminal-- a black box where I can type in and view text-- nothing else taking up space or demanding my attention. No menubars or toolbars, no scrollbars, nothing but text. Currently I'm using xterm, with mostly default settings. As far as visuals, my only custom configuration is:

```
xterm*background: black
xterm*foreground: lightgray
xterm*faceName: monospace:pixelsize=12
```

Since I've been using the Xfce variant of GhostBSD, my computer came with xfce4-terminal pre-installed for me. However, with all the extra widgets visible and the inexplicably huge default text size, it never really occurred to me that this was a terminal to get real work done in beyond running `sudo pkg install xterm` in it. However, just because it has bad defaults, doesn't mean its a bad terminal. The other day I actually opened it to check if some bad behavior in XTerm was also present there (It was). Once I had it open though, I decided to try to tame it.

The first few steps are straight forward. In the "General" Tab of the Preferences Dialog, set the Scrollbar to "Disabled", then on the "Apperance" Tab, I set the Font to Monospace Regular with a size of 9. I'm not sure why size 9 on xfce4-terminal matches size 12 on XTerm, but it does. While on that tab, disable displaying of the menubar and toolbar. Next switch over to the "Colors" tab and select the helpfully provided "XTerm" palette under 'Presets'. After this swap the text color and background color to get the classic one-true-terminal-colorscheme of light grey text on a black background. Finally, on the "Advanced" tab, make sure the default character encoding is set to UTF-8.

After restarting xfce4-terminal, it looks a lot like Xterm now at first glance, but we aren't quite there. This is painfully obvious upon running emacs and getting a completely different color scheme than on the same system under Xterm. This can be resolved with some environment variable tweaks:

```
unset COLORTERM
export TERM=xterm
```

After this, theres still one more problem though that took me a bit of googling. Under XTerm, the Emacs minibuffer prompt was aqua colored and easy to see on the black background. On xfce4-terminal, however, it was drawing as a low-contrast dark blue and was mostly unreadable. While I could tweak the color on the 'Colors' Tab of xfce4-terminal, this would also change the color of other blue elements, like the blue hackground color of highlighted text, which is supposed to be blue. It turns out that Emacs understands the idea of XTerm with a black background and automatically compensates when picking colors, but for xfce4-terminal its trying to pick a color that would work well on a white background. Throwing a `(setq frame-background-mode 'dark)` into my `.emacs` took care of this.

{% include image.html url="/assets/xterm_xfce4.png" description="An XTerm and an xfce4-terminal. They could be twins!" %}

So that did't take too much work, but it was a fair amount of work just to get back to the starting point, so why bother with xfce4-terminal at all if you're just going to make it look exactly like XTerm? Well in my case, one thing I wanted to try adding to my workflow was to use a tabbed terminal. I've tried a few times running XTerm inside of `tabbed` but it never took with me, maybe it will be different with the natively supported tabs in xfce-terminal

I usually have one big xterm on my main workspace ssh'd into work running GNU Screen with a bunch of windows logged into a bunch of machines there. Whenever I need to do something on my local machine, however, I just pop open a new XTerm, and often end up with marathon XTerm closing sessions every few days when it gets completely out of hand. I'm hoping that by having a "remote" tab and a "local" tab on my main work terminal with an easy hotkey to switch back and forth, I can train myself to use that in cases where I'd normally just spawn a new XTerm real quick, and then forget to close it when I'm done.




















