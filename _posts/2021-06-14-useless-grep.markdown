---
layout: post
title:  Most Useless use of grep?
date:   2021-06-14 06:47:00 -0400
---


The [Useless Use of Cat Award](http://smallo.ruhr.de/award.html), has been around for decades and is given out for shell constructs that invoke `cat` unnecessarily, as in:
```
cat foo.txt | wc -l
```

Where something like this:
```
< foo.txt wc -l
```
Gets the same job done with one less fork and exec, and slightly less typing. There are plenty of arguments people give as to why the useless use of `cat` here isn't actually useless. The most compelling of these to me, is that in the latter construct, accidently typing a greater-than instead of the intended less-than will result in overwriting and clobbering the file you are trying to read from. Oops.

However you often see the useless `cat` even when the alternative does not require a file redirection, as in this one I've caught myself using before:
```
cat foo.txt  | grep "foo"
```
Which could be better written as simply:
```
grep "foo" foo.txt
```

One reason people (including myself) sometimes use this construct is that it mirrors the thought process that goes into constructing the sequence of pipes: "I want the data to flow from here, then to here, then to here". You could first write just the `cat` command to see all the data, then add more to the pipe sequence to further modify the data, repeat until you get what you want. If you have a command sequence and you want to send a file through it, just throw a cat in front, just like if you have a command sequence but only want to output a subset of the lines currently being outputting, just throw a `grep` on the end. 

By this same logic, when I have a command line that I've been working on that returns the desired results, one per line, but what I really want is just the count of how many results there are, I'll just append a quick `|wc -l` to the end. Of course if the last command in the command line was a grep, it would be better to just add a `-c` to the grep invocation for the same effect, but the agglomerative nature writing a command line sometimes pulls to the less efficient solution which just involves taking a working pipeline and adding one more step, rather than thinking about how to change an existing step to get the same effect.

Writing a command that returns more results than desired and then later paring them down with `grep` is a pretty common event. Sometimes this is necessary, but plenty of times one of the previous commands in the pipeline could have easily preformed the filtering itself

I've many times ended up quickly writing a command  like
```
find . | grep foo
```
Instead of the probably much more efficient: 

```
find . -name "foo" 
```
But the most useless use of grep, is in pipe lines where one command is intentionally configured to include certain information only to have that same information removed further down the pipe line. By default, the command `ps` only shows processes for the current user. To view all processes for all users, and tag each with the username, you can use`ps aux`. I've caught myself several times writing this construct:

```
ps aux | grep <my user name>
```
Which I think must be the most useless use of `grep` ever conceived.

















