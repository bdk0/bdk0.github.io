---
layout: post
title:  Manual X11 Clipboard Paste Selection
date:   2020-10-22 12:00:00 -0400
---

I often need to copy text from my terminal and paste it into an email, but xterm, by default, when you highlight text, only places the text in the PRIMARY buffer, making it ready for pasting with the middle mouse button. The secure email client that I'm forced to use for work only allows pasting with Ctrl-V, which uses the CLIPBOARD buffer, making it hard to get the data I want to paste into an email there, so I need some way of synchronizing the two clipboards.

The first option would be to configure xterm to use the CLIPBOARD buffer instead of PRIMARY. This can be accomplished by editing **$HOME/.Xresources** to add:
~~~
XTerm*selectToClipboard: true
~~~

I didn't want to go this route, since I didn't want every bit of highlighted text to make it to my clipboard. I wanted to keep my clipboards separate but be able to choose what to paste. There are some Clipboard management programs like **parcellite** which live in your toolbar and let you synchronize, but rather than another GUI to click around in, the solution I decided to go with was **xclip**.

I ended up with the following script, which I saved in **~/scripts/paste_selection.sh**:

~~~
#!/bin/sh
TMP=`xclip -o -selection clipboard`
xclip -o -selection primary | xclip -i -selection clipboard
xdotool keyup shift
xdotool keyup v
xdotool key ctrl+v
echo "$TMP" | xclip -i -selection clipboard

~~~

This command backs up the current text in the CLIPBOARD buffer, copies the PRIMARY text into it, then it fakes a Ctrl+V keypress, The two Keyup events are necessary because I run this script from a hotkey assigned to Ctrl-Shift-V, and without this those keys would still be pressed down when the fake ctrl+v occurs. Finally I restore the CLIPBOARD to its original values.

I put this in a script in my scripts directory and set up a hotkey to call it using **xbindkeys** by adding the following to my **$HOME/.xbindkeys** file:

~~~
"~/scripts/paste_selection.sh"
  Shift + Control + v
~~~

With this functionality I can now at any point use Ctrl-V to paste my CLIPBOARD buffer into an application, or use Ctrl-Shift-V to paste the PRIMARY selection buffer without affecting the current clipboard contents, and can finally paste from xterm into my email app which does not support pasting the PRIMARY buffer with middle click.





