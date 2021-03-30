---
layout: post
title:  Git for Local Backups
date:   2021-03-30 16:47:00 -0400
---

At ${WORK} we use Perforce for version control where the central repository lives on a server that is backed up to multiple places. Our home directories on the various servers we use to develop and build on are also backed up. There is no backup system in place, however, for my desktop, especially now that I'm working full time from home. The assumption is that all important work is done on the servers and nothing important would be lost if a desktop needed to be replaced.

This is mostly true, but I do have some files, such as my dot files and scripts I write to help implement my desktop environment, and notes take in emacs-org, which are stored locally, and I would not care to lose. I also have some non-work related files such as for this blog, and other media and files I've created.

To back these up I set up a QNAP NAS and configured a backup share to be mounted on my desktop. Some things, such as my music files, I just have a static backup directory and copy things over manually, as it rarely changes. However for other directories where I create and modify files a lot, I didn't want to have to worry about forgetting to backup, and I also wanted the ability to go back to or see an older version of a document I'd been working on.

So, I set up a few git repositories on my desktop, one in my ~/scripts directory, one in my ~/emacs-org directory, etc, and have begun keeping these directories under version control. I also created bare repositories on the NAS and push to them as a backup.

This works well, but rather than manually have to push after committing, I wanted to make a post-commit hook that would automatically push to backup on each commit. While this could be configured manually easily enough, as I had multiple repositories and wanted to be able to easily create new ones, I went looking for a script.

I found a [Helpful Blog Post](https://medium.com/@fitzgeraldpk/git-dont-push-to-backup-698459ae02f2) which contained a quick and dirty script to create the bare repository and set up the remote and commit hooks in the repository to be backed up.

It worked, but it was a little too 'quick and dirty': There was no error handling, so if something went wrong it could continue running commands in unexpected places, it didn't check if the required parameters were passed in or if directories expected to exist did so. It also failed in a confusing way if passed in a relative path for the repo to be backed up. It would also silently overwrite any existing post-commit hooks already defined in your repro when it added the backup hook. (All things I ran into when trying it out)

I made some changes to the script to address these issues:

```bash
#!/usr/local/bin/bash

# Add a Post Commit Hook to a git repository to backup to another bare repository
# on every commit. Will create the bare repository if it does not exist.
#
# Usage: ./git_backup.sh -r <repo> -b <backup>
# eg. ./git_backup.sh -r ~/my_repo -b /media/nas/git/my_repo_backup
#
# Originally from https://medium.com/@fitzgeraldpk/git-dont-push-to-backup-698459ae02f2

#get the command line parameters

die() { echo "!! $*" 1>&2 ; exit 1; }

while getopts r:b: option
do
  case "${option}" in
    r) repo=${OPTARG};;
    b) backup=${OPTARG};;
  esac
done

if [ -z "$repo" ] || [ -z "$backup" ]; then
      echo "Usage: get_backup.sh -r <repo> -b <backup location>"
      exit 1
fi

if [ ! -d "$repo" ]; then
    echo "Repo [$repo] does not exist"
    exit 1
fi

 mkdir -p $backup || die "Failed to make directory $backup" 
echo "* Creating $backup"
cd  $backup 
 git init --bare || die "git init failed in $backup"
cd - > /dev/null
cd  $repo

echo "* Adding remote"
git remote add --mirror=push backup $backup || die "Failed to add remote to $repo"
git remote -v

echo "* Adding New post-commit hook"
if [ ! -f .git/hooks/post-commit ]; then    
   echo '#!/bin/sh' > .git/hooks/post-commit || die "Failed to create post-commit hook file"
fi   
echo " " >> .git/hooks/post-commit
echo " git push backup" >> .git/hooks/post-commit || die "Failed to add post-commit hook"
chmod a+x .git/hooks/post-commit || die "Failed to make post-commit hook file executable"
```

Putting this up in case someone else will find this useful.



















