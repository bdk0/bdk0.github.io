---
layout: post
title:  Remote emacsclient with tramp-term
date:   2021-09-03 06:47:00 -0400
---

For a long time, my workflow for development at ${WORK} has been to ssh from my workstation into a server and run emacs in a terminal to edit and build my code. Recently, I've started experimenting with running emacs locally and accessing resources on the remote server via TRAMP.

My emacs usage as traditionally been very basic, mainly starting emacs to edit a file, then either quitting to the shell to perform other commands, or having another shell already running under GNU Screen and switching to it. As part of my experiment I've been working to configure emacs to provide more of what I need so much more is done from inside emacs itself, providing a nicely integrated experience. To that end, I've been testing out `tramp-term` a package which allows a terminal to be opened on a remote host (via ssh), and then tracks as you move between directories. Therefore at any time from the terminal window if you `C-x C-f` to open a file, or `C-x C-d` to open a `dired` buffer, emacs knows the current directory of the terminal and defaults to that directory.

One missing feature, however, is the ability to edit a file from the command line  inside the terminal window and have it open the file in a new buffer in the local emacs instance, rather than run a new instance of emacs on the remote server inside the tramp-term. Normally `emacsclient` would be used for that, but out of the box, it doesn't work remotely. This would be useful to have working; even though you could just `C-x C-f` to open the file, because some programs will open an editor automatically, looking at the `$VISUAL` or `$EDITOR` environment variables. Setting those to `emacsclient` allows those files to open in buffers in your existing emacs session.

After much research, I did manage to cobble something together that works:


In **~/.emacs.d/init.el** add the following:
```
(setq server-use-tcp t
      server-port    9999)
(server-start)
(defun my-tramp-term--emacsclient-hook (host)
  (term-send-raw-string (concat "alias emacsclient='emacsclient --tramp /ssh:"
                         host ":'" (kbd "RET")))
  (term-send-raw-string (concat "mkdir -p ~/.emacs.d/server/ ; clear"
                         (kbd "RET")))
  (copy-file "~/.emacs.d/server/server" "/ssh:c:.emacs.d/server/server" t)
  )

(add-hook 'tramp-term-after-initialized-hook 'my-tramp-term--emacsclient-hook)
```

In **~/.ssh/config** Add the following for all hosts that you want to be able to use remote editing from
```
 RemoteForward 9999 localhost:9999
```

Then when you log in to one of these remote servers inside `tramp-term` you can use `emacsclient` on the remote to open a file locally.

**Caveats**
 - You must have emacsclient installed on the remote host and it needs to be a new enough version to support the `--tramp` option
 - Since on the host we are binding emacs server to port 9999, if you run more than one copy  of emacs at a time, susequent copies will give an error here on startup. This could probably be improved to first check if an emacs-server is already running before starting the server
 - This will only work if only one remote emacs is used to edit on the remote host at a time. If you log in from multiple emacs instances to the same remote, this solution will always try to use the most recently connected instance.
 - TCP Port 9999 must be free for binding on both the local and remote systems.

Inspired by [This Solution](https://andy.wordpress.com/2013/01/03/automatic-emacsclient/), though modified to create and use an alias instead of needing a bash script on the remote and altered to work with tramp-term.

I've also posted these instruction as a comment on the relevant ticket on the [Tramp-term.el Github](https://github.com/randymorris/tramp-term.el/issues/5) since if anyone else is searching for a solution, people are more likely to end up there than this blog.


