---
layout: post
title:  Fun with 'du', symlinks, and bind mounts
date:   2020-12-12 9:47:00 -0400
---

The **du** command estimates the amount of space taken up by a directory tree, I usually use it when I'm trying to figure out why something is taking up so much space, piped to a '**sort -n**' to see what the largest subdirectories of a given start point are.

As a first approximation, **du** prints the directory tree under the starting point annotated with sizes, but that's not exactly correct. Take the following example run on a Debian GNU/Linux box:

```
$ mkdir dir
$ ln -s dir link
$ touch dir/foo
$ du
4       ./dir
8       .
```

As you can see **du** did not follow the symlink, since this is the default behavior, unless the **-L** option is provided to make it dereference symlinks, as such:

```
$ du -L
4       ./dir
8       .
```

Hmm, ok that still didn't work. It turns out that since the purpose of **du** is to determine how much space is used up by a directory tree, it tries really hard not to account for the same space twice, and realizes that the **./link** directory has already been accounted for and skips it. In fact I ran this test a few times and was even to sometimes get it to skip the **./dir** and process the **./link** instead, probably something to do with inode number ordering I'm guessing.

Bind mounts are another alternative to symlinks in Linux that do sort of the same thing in a very different way and with different consequences. How does 'du' handle them?

```
$ mkdir mnt
$ sudo mount --bind dir mnt
$ du
4       ./dir
4       ./mnt
12      .
```

It appears that using a bind mount successfully tricks **du** into counting the space twice, but that's not entirely true:

```
$ du -L
4       ./dir
8       .
```
The man page says that **-L**, "*Dereferences all symbolic links*", but it appears that the man page is not telling the whole story. It appears that **-L** also turns on the "make sure we don't count the same space twice" feature. Presumably without that option enabled, **du** assumes it doesn't have to worry about such a situation and doesn't run that code, allowing the bind mount to sneak in the back door.

You can leave off the **-L** option to tell **du** not to follow symlinks, but what if you don't want to follow bind mounts? There is a convenient **-x** option which will "*skip directories on different filesystems*", which sounds like it would help:

```
$ du -x
4       ./dir
4       ./mnt
12      .
```

But nope, the tool doesn't see the bind mount as a separate filesystem, since its really part of the same filesystem mounted at a new point.

Unlike the  Linux version of **du**, which is part of GNU Core Utils, the **du** from FreeBSD is a different program, originally from Version 1 AT&T Unix, and it does not appear to make this attempt to avoid double counting space. On FreeBSD all three directories are shown with the **-L** option is enabled, and the **./dir** and the **./mnt** when it is not enabled. (Using nullfs instead of bind mounts on FreeBSD, since it doesn't support the latter by default)



















