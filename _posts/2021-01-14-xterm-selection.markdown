---
layout: post
title:  XTerm Selections Revisited
date:   2021-01-14 16:47:00 -0400
---

I wrote about a method I had used to handle [Manual X11 Clipboard Paste Selection](https://bdk0.github.io/2020/10/22/xclip-paste.html). There I set up a shell script hot key so that **Shift-Ctrl-V** would paste my PRIMARY X Selection (and **Ctrl-V** would paste my *CLIPBOARD* selection as normal. This worked (mostly), but it was a bit of a hack that I never really liked. I knew of an alternative, to configure XTerm to always copy to the *CLIPBOARD* instead of the *PRIMARY* selection, but I wasn't enamored with that approach either, as I often want to use the *PRIMARY* selection.

I recently ran across a blog post [XTerm: It's Better Than You Thought](https://aduros.com/blog/xterm-its-better-than-you-thought/) which clued me into a much better way to handle this. in `.Xresources`, they add:


```
XTerm.vt100.translations: #override \n\
   Ctrl Shift <Key>C: copy-selection(CLIPBOARD) \n\
   Ctrl Shift <Key>V: insert-selection(CLIPBOARD)
```

This allows **Shift-Ctrl-C** and **Shift-Ctrl-V** to copy and paste to the *CLIPBOARD* selection, while still leaving the normal XTerm *PRIMARY* selection functionality alone.

More interestingly, however, that same blog introduced me to an easy way to open URLs in an XTerm in a webbrowser. They provide a hotkey and a script to cause XTerm to open a menu of all the URLs visible in the XTerm, and upon selecting one, open it in a browser. This is useful to me, since I always have an IRC Window open for chat with my co-workers, and links are often pasted into chat. Normally I have to select the link with the mouse, and then paste it into Chromium with middle click, like some sort of caveman. The `.Xresources` configuration worked fine for me, but I had a hell of a time modifying the shell script provided to work for me on FreeBSD.

The script as provided on the blog was
```
#!/bin/sh -e
grep -Eo '\bhttps?://\S+\b' |
    uniq |
    ifne rofi -dmenu -i -p "Open URL" -auto-select |
    xargs xdg-open
```

I swapped out the `rofi -dmenu` for just normal `dmenu` for aesthetic reasons, and was pretty quickly able to change the regex to `'https?://[^[:space:]]+[[:>:]]'` which is the same functionality but works on BSD Grep. However even after that, while the script worked fine when run manually, when run from the hotkey inside XTerm, it would hang forever trying to read from stdin. After trying many, many, things, it turned out that, for whatever reason, XTerm wasn't closing stdin after writing the screen out to it, and nothing I could do would convince it to do otherwise, so the script was always waiting on more input.

I did finally work out the following hack:

```
#!/usr/local/bin/zsh

screen=""
while read -t 0.1 line
do
    screen="$screen\n$line"
done
echo "$screen" | grep -Eo 'https?://[^[:space:]]+[[:>:]]'  |
    uniq |
    ifne dmenu -i -p "Open URL" |
    xargs xdg-open
```

Since `cat` would hang forever until stdin was closed, I had to find a way to read from stdin without waiting. `read` allows for a timeout value, but it only reads a line at a time, and strips newlines. Running it in a loop and concatenating all the lines together while reinserting the newlines does the trick. This means my script requires a shell like `zsh` or `bash` to run, since `sh` doesn't support `read`, but that's not a problem for me.

After these changes, the script worked great for me, until I tried to use it on a URL that line wrapped, and it cut off the URL at the newline. I can resolve this by not inserting the `\n` between each line, but then URLs that **should** end at the end of a line, get merged with the beginning of the next line. Fortunately, I really only want this script for pulling urls from `irssi`, so I should be able to  remove the `\n` and write the regex to end at the next real line by pattern matching the date/username that starts each line on IRC. I haven't implemented this yet, but it should take care of the issue.

Its unfortunate that XTerm doesn't distinguish between actual newlines on the screen and linewraps. Dragging the XTerm big enough on so the URL doesn't wrap, makes it detect the entire thing. I did try out the xfce4-terminal with its "Right Click and select Open-Url from the Menu" functionality, and it suffers from the same issue-- it considers the URL to end at the line wrap as well.















