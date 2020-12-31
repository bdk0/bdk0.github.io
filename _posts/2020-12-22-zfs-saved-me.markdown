---
layout: post
title:  ZFS Saved Me or Screwed Me, Still not Sure Which
date:   2020-12-22 16:47:00 -0400
---

The other day on my GhostBSD workstation, Chromium crashed. This is not a totally strange event for me, but what was strange is that it refused to restart. Running it from a terminal, I saw it was complaining about cache files. Figuring that the cache files had somehow become corrupt, and that they were after all just a cache, I should be able to remove the cache and let it recache stuff on startup, I went to my cache directory and did a **mv** on it to rename it from **Cache** to **Bad_Cache** or something like that, and Chromium happily restarted and started caching to its hearts delight. Problem solved?

I figured I didn't need this bad cache sitting around and attempted to **rm -rf** it, only to be greated with an error about *Directory not Empty*. This was odd, since **-rf** should basically tell it 'don't care if the directory is empty or not, just delete it'. I manually deleted all the files inside the directory, making extra sure that there were no hidden files lurking in there, and attempted to **rm -rf** it with the same result.

Eventually I tried **zpool status -v** which told me I had some checksum errors on the pool, including the directory I was trying to delete. Presumably my drive was failing and it just so happened that rather than a file getting some bad data, some directory metadata on the filesystem did, and zfs was now incapable of deleting said directory. **zpool scrub** wasn't able to accomplish anything useful, so I verified that my backups of my important files were good and up-to-date, and ordered a replacement drive for the system.

I wasn't looking forward to reinstalling and reconfiguring GhostBSD to my liking again so I decided to do a full backup of everything to an ISCSI target on my NAS for safe keeping. I formatted the target as ZFS, mounted it on my workstation and attempted a **zfs send ... \| zfs receive ...** to move the data over, only to find out that the zfs send reliably died when it got to the unremovable directory. Ugh. I tried a few different things to get rid of the directory so I could back everything else up, I discovered that while I couldn't **rm** it or **mv** it to another pool, I could **mv** the directory to another dataset on the same pool, so I ended up moving it out of the way, backing up the dataset it was on, then moving it back so I could backup the other pool, and eventually ended up with a copy of all my datasets on my backup pool.

I was relieved to see that **zpool scrub** and **zpool status -v** did not see any errors on the backup pool. I was a little concerned, not knowing enough about how zfs send worked, that sending a pool with corruption might send along the corrupt data structures with it, but it does not appear to do so.

During the next few days I continued to use my workstation while waiting for the replacement drive, and things mostly worked fine except for on one occasion where a large file I downloaded via ssh gave errors decompressing. After investigation I found the md5 has was wrong on my downloaded copy and that **zpool status -v** was once again complaining about this file. Luckily erasing it and redownloading it resolved the issue. A few days later I had everything re-installed on a shiny new 1TB SSD.

I'm kind of disgruntled that ZFS wouldn't provide any mechanism to fix corruption in its on-disk representation of a directory, to at least let me remove the data and back up the pool. Far inferior filesystems have repair tools to at least let you peacefully lose the data it can't recover (I should know I've unfortunately had to use them many, many times), so for ZFS to just throw up its hands and shrug is kind of disconcerting. On the other hand, I was able to work around the issue and get everything backed up in the end, so no harm done, and I'm pretty sure all the checksumming that ZFS does helped me notice the drive was going much earlier than normal and save all my data. So, thanks?















