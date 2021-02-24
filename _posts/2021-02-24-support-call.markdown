---
layout: post
title:  My Favorite Support Call
date:   2021-02-24 16:47:00 -0400
---

I was sitting at my desk one afternoon, about fifteen years ago, when the phone rang. This by itself was an odd occurrence. I was a programmer after all, who wanted to talk to me? Even more oddly, the caller was a receptionist from the main building down the street wanting to transfer a customer support call to me.

Not that I never handled customer support issues-- I was lead programmer on a module which our system used to connect to a myriad of third party systems, and there were often integration issues that needed to be sorted out when a new site went live. We would test against a manufacturer's 2.04 software and 2.05 software only to find out on site that there was an undocumented 2.04a release that only ever went out to a single customer, with undocumented protocol differences, and guess who just bought our system? However usually by the time an issue got to me it was distilled down into a ticket "Error getting data from Foobar Baz 2.3 system". It was pretty rare for me to talk directly to the boots on the ground.

Our system was a big box that connected to other even bigger boxes via thick cables with a lot of pins and spoke to them using various protocols, some standard, some proprietary, some undocumented and reverse engineered, and this was why this particular customer was calling.

Upon taking the call, I was greeted by a friendly and polite, but clearly exasperated, caller. His path in getting to me clearly explained his exasperation. He was a technician working for our customer and was assigned to install the device my company made into the data center at a secure government location. He had run into trouble getting our device talking to their systems and reached out to the reseller's technical support team.

After running through some troubleshooting steps, escalating within the reseller, and then running through them all again with the a second level technician, he'd been shunted over to the manufacturer's (my company's) technical support team, to troubleshoot a third time. After reaching the depths of our technical support he ended up in our QA team, who tried to figure out if what he was experiencing was a bug in our system or not, and finally when that came to naught, we was redirected to me as the lead developer for the module in question.

It quickly became clear why we were having so much trouble diagnosing the problem, and why no one could give me an actual case with any sort of details to work on. Our normal troubleshooting steps would involve connecting a modem to a phone line and having our device on site dial home, where we could log in to the underlying OS to investigate issues. We also had a myriad of other methods for remotely accessing our devices for troubleshooting, but at this site, being a secure government location, they were all off the table. There wasn't much of a tool chain in place for troubleshooting without remote access, just a spartan front panel with some readings, a tired technician and me. Everyone else had already failed, so now it was my turn.

The thick cable that connects our device to the device we are talking to connects to a large multi-pin connector on the back of our device, and the diagnostic readout can show the voltage level on each pin once per second. Not a ton to go on when the values can fluctuate thousands of times per second, but that's what I had him read to me.

"All Zeros", he assured me.

"No readings at all?", I replied, "Are you sure the cable is plugged i..."

He cut me off before I could finish. "Listen, I've been on the phone for five hours. I've talked to six different people and all of them wanted me to make sure the cable is plugged in. I assure you it is. I'm not an idiot, I'm looking at the back of your unit now, and I can see that the cable is definitely plugged in. I can see the cable and the connector. Both are in my field of view right now, and they are definitely plugged together. I've also already tried unplugging and replugging the cable, and cleaning the connector. Its not the cable. The cable is great, its brand new. Can we just assume I'm not an idiot who forgot to plug this thing in, and troubleshoot the actual issue? Whats the next step?"

It seemed reasonable, problem was, without remote access I didn't really have a next step, and all those pins reading zero volts almost certainly pointed to a cable issue. I wasn't sure if I should go there, he already seemed kind of touchy, but I didn't have any other ideas, so I opened my mouth:

"Is the other end of the cable plugged in?"

This was met with a moment of silence as he put down the phone to check, and quickly came back on the line, obviously embarrassed.

"Ok, thank you for your help, I think I can handle this from here. *click*"  He was in quite a rush to get off the phone. 

Goes to show that support technicians can check if a cable is plugged in, but you need the lead developer to remember that cables have two ends. I guess that's why we earn the big money.

All kidding and self-aggrandizing aside, this was my favorite support call I've ever been on, all the build up to the punch line, all the techs who went down the same train of thought, but didn't quite take it to its logical conclusion, the sudden denouement, it felt like being in a comedy skit.



















