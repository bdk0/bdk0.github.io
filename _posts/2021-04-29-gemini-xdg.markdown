---
layout: post
title:  Gemini, Chromium, and XDG, Oh My
date:   2021-04-29 06:47:00 -0400
---

I found [Vermaden's series of blog posts on setting up a FreeBSD Desktop](https://vermaden.wordpress.com/freebsd-desktop/) were very useful when I was first working on getting FreeBSD installed and usable on my Thinkpad.

The most recent installment posted a few days ago about configuring and using XDG, [Part 24 - Configuration - Universal File Opener](https://vermaden.wordpress.com/2021/04/22/freebsd-desktop-part-24-configuration-universal-file-opener/) reminded me of my own struggles with XDG and xdg-open a few months ago.

I don't make a lot of use of this sort of feature on my desktop. My natural usage patterns tend to have me think in terms of "I'll open wireshark (either from a command line or dmenu), and then load a PCAP", as opposed to "I'll click on this PCAP and open it", so I've never worried much about how these linkages for what program is opened for each file type are made or how to configure them.

However a few months ago I ran into an annoying issue that I wanted to fix. I've been using [Lagrange](https://github.com/skyjake/lagrange), a very nice looking graphical browser for [Gemini](https://gemini.circumlunar.space/). I found that if when browsing a Gemini site in Lagrange, I ran accross a hyperlink to an http site, I could click it in Gemini, and the link would open in Chromium. However if I ran accross a gemini:// link in Chromium, clicking on it would only give me an xdg-open error dialog and I would have to copy the link, open lagrange, and paste it in manually, and who has time for that?

Since Chromium was calling xdg-open to try to figure out what to do with the link, I figured a brief foray into the xdg-open documentation would tell me how to configure it to understand what to do with gemini:// links. However, I came up empty. There may have been a simple way to do this that I just missed, but everything I found with xdg-open was for deciding what program to make based on mime types and extensions. I couldn't find a way to say: "match `^gemini://`" and open this program. So I decided to hack it.

My "solution", kluge, workaround, whatever, was to make a shell script called 'xdg-open' and put it in my ~/bin folder, which appears early in my PATH. This way whenever Chromium wants to open a link, this script gets called and I can do whatever I want as far as pattern matching and spawning applications. Then if my script doesn't want to do anything for a particular link, it can just call the real xdg-open for the normal behavior. Its currently a very short script that looks like this:

```bash
#!/bin/sh
case "$1" in
    gemini:*)
        exec lagrange "$@"
        ;;
    *)
        exec /usr/local/bin/xdg-open "$@"
        ;;
esac
        
```

Now I have a simple way to control what application chromium will spawn for a given link without needing to muck around with xdg-open configuration. I much prefer having a simple script where I can fully customize behavior for cases like this than have to fight against other software to configure it to do what I want.











