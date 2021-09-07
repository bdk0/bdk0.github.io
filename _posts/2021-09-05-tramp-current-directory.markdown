---
layout: post
title:  Remote Terminals in Emacs and Following the Current Directory
date:   2021-09-05 06:47:00 -0400
---

Previously, I wrote about configuring emacs terminals for [remote emacsclient usage](https://bdk0.github.io/2021/09/03/emacs-tramp-emacsclient.html). Today I want to write about another aspect of configuring ansi-term in emacs which turned out to be suprisingly difficult.

One of the main purposes of running a terminal inside of emacs instead of as a separate process, is for the integration we get between the terminals and our editor. When editing a file, a keybinding can pop up a terminal in a buffer that is already in the same current directory as the file we are editing, without the need to go traversing the filesystem to get there. This works out-of-the-box with local terminals, but by default, once you ssh into another host, this integration is lost. In addition, if we are in a terminal at a certain directory, and run a command like `C-x X-f` to open a file or `C-x C-d` to open a Dired buffer, we would like emacs to default to the same directory we are currently at in the terminal. 

With TRAMP, emacs can easily open files on the remote server as if they were local, this two-way integration between the terminal and the rest of emacs is lost without special configuration. To make this work, the remote host needs to send information about its current directory, user, and host. This is done by sending text in a special format, generally as part of the prompt. To avoid visual clutter, escape codes are used to make the text visually invisible, though emacs can still parse it.

Though all of this can be configured manually by editing the `.bashrc` or equivalent on each remote host you want this to work on. Rather than manually set all this up, there is a small package which wraps `ansi-term` called `tramp-term` which can be installed with `package-install`. This package prompts for a host to connect to (or takes it as a parameter), and then connects to the remote host and sets up environment variables on the remote host to send the necessary data back for the session.

This is all a well traveled path, but no matter what I did, I could not get it to work, whenever I had it set up properly to send the necessary data back from the remote host, attempting to use it by opening a file, etc, would return an error about being unable to resolve host where the host it was trying to resolve was a hyphen (-). Messages showed:

```
Tramp: Opening connection for - using ssh...
Tramp: Sending command ‘exec ssh   -o ControlMaster=auto -o ControlPath='tramp.%C' -o ControlPersist=no -e none -’
Tramp: Waiting for prompts from remote shell...
Tramp failed to connect.  If this happens repeatedly, try
    ‘M-x tramp-cleanup-this-connection’
Tramp: Waiting for prompts from remote shell...failed
Tramp: Opening connection for - using ssh...failed
```

Clearly something was wrong, but no amount of googling or ducking found anyone else with a similar issue. It seemed like this just worked fine for everyone but me. I did finally find the culprit, when I first set up tramp, I had put a ine in my `init.el` to make it use "simplified" syntax for Tramp hosts, so I could leave off the `/ssh:` when typing host names. Apparently this breaks everything. Once I finally found and removed this line this worked ok.

The second part of the issue was to be able to open a terminal and have it automatically open at the same host and path as my current emacs directory, so if I was looking at a file on localhost, it would open an `ansi-term` at that directory, and if the current file was a tramp file on a remote host, it would open a `tramp-term`, connect to that host over ssh at the same directory as the file. For this I ended up writing some elisp:

```lisp
(defun my-tramp-term--cd-hook(host)
  (term-send-raw-string (concat "cd " (tramp-file-name-localname ts) ";clear" (kbd "RET")))
)

(defun my-tramp-term-here()
    (interactive)
    (if (tramp-tramp-file-p default-directory)
         (let ((ts (tramp-dissect-file-name default-directory)))
           (if (string= (tramp-file-name-method ts) "ssh")
               (progn
                 (add-hook 'tramp-term-after-initialized-hook 'my-tramp-term--cd-hook)
                 (if (tramp-file-name-user ts)
                     (tramp-term (list (tramp-file-name-user ts) (tramp-file-name-host ts)))
                   (tramp-term (list (tramp-file-name-host ts)))
                   )
                 (remove-hook 'tramp-term-after-initialized-hook 'my-tramp-term--cd-hook)
                 )
             (message "tramp-term-here: Unsupported Tramp method. Only ssh supported"))
           )
      (ansi-term (or explicit-shell-file-name "bash") "localhost")
      )
    )
```

Now I can just call `my-tramp-term-here` on an open buffer, and it will do the right thing. Looking at this again, I don't think I really need the hook, I could just run the `term-send-raw-string` right in the method body after connecting the tramp-term.

Incidently as I am just now learning elisp, I was surprised that the hook function was able to access the `ts` variable defined in the `my-tramp-term-here` function locally. At first I figured it was some sort of automatic closure and had to do with where the hook was defined, but it actually turns out to be due to elisp's dynamic scoping and the `ts` variable is only visible because the hook is called from the tramp-term call which is called from my `my-tramp-term-here` function. If I were to leave that hook in place and `tramp-term` got called from a different function, this wouldn't work. I don't think I've ever worked in a language with dynamic scoping before, its kind of different.



