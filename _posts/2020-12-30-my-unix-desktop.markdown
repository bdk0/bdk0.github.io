---
layout: post
title:  How I Actually USE my Unix Desktop
date:   2020-12-30 16:47:00 -0400
---

# Introduction

Sometimes when I'm bored I'll browse [/r/unixporn](https://reddit.com/r/unixporn) and ogle the attractive nut impractical Unix desktop layouts. [/r/usabilityporn](https://reddit.com/r/usabilityporn) is better as far as seeing desktops that someone might actually make use of beyond setting them up in a virtual machine and taking a screenshot.

However, even there, you can't really get a good idea of how they might actually use the desktop environments they are posting. What I always want to know is *"How do you use your desktop? What hot keys, what software do you lean on? How does it feel to exist in that environment? Why do you choose this instead of something else?"* Alas, these questions are rarely answered beyond perhaps a list of themes and software shown.

So in the spirit of "*No one writes them like they used to, so it might as well be me*", I wrote this post to provide way more information about my own desktop use than anyone in their right mind would ever care to know.

One interesting thing to me, is how much more the desktop environment affects the day to day usage experience than the OS itself does. The screenshots on this post are taken from my GhostBSD Desktop, but I have FreeBSD, NetBSD, and Debian GNU/Linux installs use pretty much the same configuration, so for day-to-day usage they feel a lot like the same system until I need to go and install packages or something.

# Part 1: The Look

{% include image.html url="/assets/ghost_desktop1.png" description="An empty, clean desktop. I never see this in real life." %}

## Desktop Environment 
My desktop starts off with [Xfce](http://xfce.org), mostly for the Xfce4-panel, which I like because it can be simply configured to exactly how I want it to look and act. I remember trying both Gnome and KDE back in the late 1990's but found them both heavier and more opinionated than I liked, so I pretty much always used used various window managers instead of a full desktop environment, culminating in my discovery of the [Notion Window Manager](https://notionwm.net), which at the time was called *Ion* (Notion is basically "*N*ot *I*on" and was forked when the original developer of Ion took his ball and went home).

I used Notion on all my desktops from 2002-2018, and got really used to its workflow, so even though I'm no longer using it, I can see echoes of my usage of it in my current configurations in how I open and close windows and how I layout my applications. In 2018 I was reinstalling Debian at work after a hard drive failure, and decided to try Xfce, pretty much on a lark. I'd heard it was much lighter than Gnome and KDE, and that it was fairly configurable and not as opinionated as those two. Notion is all workflow and no eye candy at all, which is pretty much how I like it, but perhaps maybe a little eye candy would be ok. I find having a nice desktop background, window decorations and a status bar changes the way I feel about the computer I'm using, its nothing huge, but perhaps like the difference in working in a nice office versus in a run down, but efficiently laid out warehouse (I've done both).

I have no use at all for things like File Managers, or icons living on my desktop, so while Xfce is the basis of my work environment, my interaction with it is limited primarily to xfce4-panel and occasional use of a utility like the Xfce Screenshot tool (which I used to take the images for this blog post).

I'm also not been big on busy, complicated background wallpapers. When using Notion for years there was no concept of a background image, and on Windows I just use plain blue. I am a stickler for throwing up the logo of the OS I'm currently using though, as long as its not something I'm embarrassed of (Perhaps the reason my MS  Windows backgrounds tend to just be plain blue). My FreeBSD laptop background is black with the shiny red daemon-head, my Debian desktop is blue with the Debian Spiral, etc.  I turn off all icons on my desktop and never use them and I have better ways to quick launch something.

## Panel
I'm currently using xfce4-panel, part of  Xfce. A few times I've considered porting my panel settings to something standalone, as it feels a bit silly to pull in an entire desktop environment that I mainly just use to set a background wallpaper and display a panel. I could probably do just as well with something like feh + tint2, but Xfce is pretty light weight anyway and hasn't been a problem for me. Its also one of the two default flavors of GhostBSD, the other being Mate).

My preferred panel setup, is, oddly enough, reminiscent of the setup Microsoft introduced in Windows 95. You can usually tell when a *nix user comes from a Windows or Mac background based on their preferred panel setup, though in my case I actually started using Linux a few years before Windows 95 was released. I'm guessing I originally picked up an affinity for this style from with fvwm95 window manager, which I used for a few years in the 1990's.

One thing I've never liked though, either from Windows, or any of the X11 Desktop environments that copied it, is the start menu with a bunch of programs in it. I hate it even when they make it a 'right click on the desktop' menu instead.  Much like my disdain for icons on the desktop, I have better ways to launch a program than to scroll through a start menu. In place of a start menu, I put two launcher buttons for the two Windows I tend to launch the most-- Chromium and xterm. The 'up arrow' next to Chromium, also grants me the ability to also run Firefox or the Linux version of Google Chrome (under Linux emulation) for those sites where Chromium just won't cut it. The empty folder icon hides all my programs and shows me the desktop, which actually serves no purpose other than to let me stare at the calming blue desktop when I get stressed by how many Windows I have open, since  In addition to not storing icons on my desktop, I rarely use and never configure the Xfce 'right click on the desktop' menu.

The rest of my panel is extremely standard stuff, icons for all my open programs, a view of my four workspaces, which I'll write more about below, a notification center, which mainly is just to let me know when GhostBSD has updates for me to install-- which it does in the screenshot, and the current time. My laptop version of my configuration also has a battery status icon between the workspaces and time. I've seen a lot of really cool status bar setups people do with things like [polybar](http://polybar.github.io), but truthfully, just isn't a lot of information that I want to stare at *all the time*. Instead I have hot keys that pop up a status bubble so I can quickly peak at something like my Load or CPU Temperature if I care to see it. 


## Window Manager

There's nothing wrong with the default Xfwm window manager that comes with Xfce, and I sometimes wonder if I'm just a difficult person who just likes to make things complicated, but I always replace the default Xfwm window manager with OpenBox. I originally moved to OpenBox because  Xfwm had some visual glitches with the Intel Graphics drivers on FreeBSD 12.1, but soon moved my other systems to it as well-- It looks great (with the Bear2 theme), and I like how it briefly pops up the name of the workspace when you change workspaces. This, along with the fact that unlike Xfwm, it won't let me Go "Right" from the Right most desktop to get back to the left most, helps with my feeling centered in a 'physical' layout, as if my workspaces are physical things in spaces. Without these features, I feel less grounded in my system and tend to forget what workspace I'm in at any given time.

{% include image.html url="/assets/ghost_desktop2.png" description="A highly staged screenshot showing dmenu and some applications. Everyone runs neofetch at all times, right?" %}

The default Window decorations from OpenBox provide me all I need- standard minimize, maximize and close buttons. There's also a right click menu I can use to move the window to a different workspace, or make it 'always on top'. I also have hot keys to do these things, I tend to use mouse or keyboard for this interchangably depending on where my hands happen to be. If I'm scrolling through a webpage with the mouse and want to move the browser to another workspace, I'll use the right click menu. If I'm typing in an xterm and want to do the same, I'll use the hotkey.

One other thing when it comes to Window Managers that is a must have for me is 'focus follows mouse'. Anything else just seems wrong, because it is.

# Part 2: The Hotkeys

Far more important to me than the visual elements in a desktop, are the keybindings. Most important of course, are the keybindings that actually get used. Between my desktop environment, Window Manager, terminal, GNU Screen, and the software I actually *run* underneath all that. There are a ton of available keybindings that I've never committed to memory or at least my hands won't reach for automatically. Its the keybindings you use, that are important, not the ones configured.

Some of the keybindings I use are configured through OpenBox, but whenever possible I like to configure them in xbindkeys. It's an older program, but I prefer it as opposed to configuring hotkeys inside my window manager or desktop environment whenever possible for two reasons: First, I can take it with me and wouldn't have to re-configure everything If I want to try out a new desktop environment or window manager. Second, it feels more unixy  to have a standalone daemon that handles the hotkeys and doesn't mess with anything else. It has a simple configuration file format, much nicer than editing XML to configure OpenBox, or clicking around in some GUI. 

Whenever possible I bind my desktop Hotkeys to use the "Windows Key". Its there on the keyboard anyway, and nothing else uses it, so it doesn't interfere with hotkeys I want to send to my applications. The main exception is the hotkeys I use to change to a different workspace, which are Bound to Ctrl-Arrow Keys to go right,left, up or down. This can sometimes conflict with keys that applications want to use, but I prefer it over the OpenBox default (which If i recall is Ctrl+Shift+Arrow), as I use these  a ton, and having it on ctrl also allows me to use the right ctrl button and do the hotkey with just my right hand. Also, Win-Arrow is already taken by the hot key to change which window on the desktop is active. My most commonly pressed HotKeys go something like:

Ctrl + *Arrow* | Change to a different workspace
Alt + Ctrl + *Arrow* | Change to a different workspace and bring the focused window with me
Win + *Arrow* | Change to a different window in this workspace
Win + c | Close Focused Window
Win + x | Open xterm
Win + z | Open dmenu- Lets me run any program by typing the name (with tab completion)
Win + b | Show Battery / CPU status (CPU Frequency, Temperature, Current Drain, Time Left)
Win + v | Prompt me for a password and connect my VPN to ${WORK}
Win + t | Open xterm and run top. Upon hitting ESC to quit top, the window will close
Win + Shift + *letter*| open xterm and ssh to the server at work whose name begins with said letter

In addition I have several hot keys bound to open specific programs and put them on specific workspaces at preset locations and sizes, but I'll write more about those in the next section. With all the possible hotkeys betweeen those I've configured and those that are preconfigured for me, writing it all down, its suprising how few it takes to add up to 90% of my usage.

There are four main ways I launch programs. For xterms and chromium, I have a launcher icon I can click in my panel. More commponly I'll spawn an xterm with the Win+x hotkey and run whatever I want inside it. If I don't want or need a controlling xterm sitting around for a GUI program, I'll click Win+Z to bring up a bar at the top of the screen, type the program name in, and hit enter to run it. I use (dmenu)[https://tools.suckless.org/dmenu/] from suckless to for this. I actually use the *run_recent* script for dmenu to show me the most recent programs I've launched from there. It also allows me to append a semicolon to a command to automatically run it in an xterm, though if I'm going to go that far, I'm more likely just to spawn an xterm with Win+X and run it from there.

One important, but invisble aspect of my workflow, is that I have OpenBox configured to always spawn new programs under the mouse. This, coupled with the focus-follow-mouse setting means that I can open an xterm with Win-X, begin typin in it immediately, then close it with Win+C when done with it without my hands ever leaving the keyboard, So "Win-X ls<enter> Win-C" shows me all the files in my home directory in an xterm and then gets rid of the window when I'm done looking. Nothing about my configuration is all that custom, but its simple configuration options in combination like that that make me love using this environment. I feel as if I'm using a workstation designed for me, the way I want it to work, not something designed for how some UX designer things I should want it to work.


# Part 3: The Programs

The programs I spend the most time in, ordered approximately from most to least:

## XTerm
I easily spend more time in an XTerm than any other program. There's plenty of other terminals of course, and every desktop has its own default terminal, but I find they are really only useful for typing **pkg install xterm** or **apt install xterm**. There's something about good old xterm that just feels comfortable. I use it with pretty much default settings. I just throw a couple of lines in my **.Xresources** and I'm good to go:
~~~
xterm*background: black
xterm*foreground: lightgray
xterm*faceName: monospace:pixelsize=12
~~~

Despite using XTerm so much, I can't say I'm a power user of it, in fact, the XTerm hotkeys I can think of off the top of my head that I use regularly are **Shift-Insert** to paste the X Selection buffer, and **Shift-PgUp** to scroll back and see something I've missed.

Every now and then I'll think that I should really start using tabs in my terminals, and will install [Suckless tabbed](https://tools.suckless.org/tabbed) and start using it for a few days, but I always fall out of using it. I guess I don't really need it.

## Chromium

Chromium is currently my standard 'View some random page' web browser. I used to mainly use FireFox, and think that in some ways I still prefer Firefox, but I use Firefox in a specific way on my system which leaves Chromium for everything else. I don't have a lot to say about Chromium, I tend to think of web browsing as a mousy activity as opposed to using a lot of keyboard activity, so I don't use a ton of chromium hotkeys, except for Ctrl+L to focus the URL bar, Alt-Left and Alt-Right to navigate back and forth in history, and Ctrl-Tab and Ctrl-Shift-Tab to change tabs. Pretty much everything else I do via mouse.

I just realized that I don't actually have a hot key to open Chromium, instead I have Win+J which opens up Chromium, moves it to my second Workspace (the Browsers Workspace), maximizes, it, and loads up my Work JIRA instance (Hence the J). I run this shortly after starting up my computer and from there, to view random sites in Chromium, I just use the New Tab or New Window in Chromium.

## Firefox

For work I have to use an annoying "Secure" email client which is basically a VMWare Horizon Virtual Windows Machine running Outlook. For "Security Reasons" Cut and Paste is disabled and its painful to actually get access to files I'm sent. The actual VMWare Client doesn't support my Operating Systems of choice, but luckily the Web Client does work for me, more or less . While It does work in Chromium, I have better luck with it in Firefox. Connecting to my email requires the use of a SmartCard reader and an ID card.

To make The Website reliably access the SmartCard, I have a special script which first kills any open firefox, then kills and restarts the opensc daemon that talks to the Smartcard reader, before starting Firefox and pointing at the Webclient URL. This causes it to pop up a prompt for my PIN. This only works reliably by killing and restarting both firefox and opensc before every use. I've had this problem in both Linux and BSD, so I've just gotten used to it. This is the reason I can't use firefox for other uses, it seems if I leave other firefox windows open, restarting the email window dowsn't work reliably.  

I have this bound to Win-H (For Horizon)

## Wireshark

I've been using Wireshark from back when it was called Ethereal. I do a lot of TCP/IP, Protocol development, and VoIP stuff for work, so after Terminals and a Web Browser, this is the most important program for me. Usually I just run this right from the XTerm I'm working in, eg 'scp server:whatever.pcap . && wireshark whatever.pcap', though othertimes I'll run it from dmenu.

## VirtualBox

I've always got a couple of virtual machines running. On my BSD workstations, I've got an old 32-bit Windows XP running an ancient version of Visual C++ that I still need on occasion to build a change to a legacy product we still support. I also keep a Debian 10 VM running all the time, that I use for running Skype as well as Docker and a few other things. Other than those, I'll sometimes spin up NetBSD or another Linux distribution I want to check out, but just for fun or curiosity.

For many years, I ran Windows on my personal laptop and used VMWare to run a virtual Linux desktop when I needed to work from home. This worked well since I got the hardware support of Windows (Running on a Laptop with spotty Linux hardware support), but the user environment from Linux, which I prefer. Eventually I bought a different laptop specifically for good Linux/BSD compatability.

When I first moved to FreeBSD, I tried to move to bhyve for virtualization, but I find myself more at home with VirtualBox. The only complaint I have about it is that USB2 and USB3 passthrough isn't supported on FreeBSD.

## Skype

My team uses Skype for all of our conference calls and for ad-hoc group debugging sessions, so its important for me to have a workable client. I've had mixed luck with running the Skype Web Client under Free/GhostBSD, so my workaround is to run the [Linux client on a Debian VM](https://bdk0.github.io/2020/10/30/freebsd_skype.html). Of course on my Debian system I can just run the client directly. I don't run X11 on my VM though, and often just run this one headless. I have a hotkey (Win-S) which ssh's into the VM and starts up Skype with X Forwarding for GUI and PulseAudio forwarding over the network for Audio. Its not perfect, but it works good enough for my use. Skype is always running on an out-of-the-way workspace waiting to let me know someone's calling me.

## Audacity

Another program I use a ton for work, mainly when working on codec or Voip stuff. Run some code that decodes or processes some audio. Listen to it in Audacity. It sounds like total Sh*t. Repeat ad-infinitum.

## Evince and LibreOffice

Evince to read specifications and standards that are in PDF format. LibreOffice to read them if they are .docx. I pretty much don't use them for anything but that. If I need to write something, I'll do it in emacs, maybe with markdown or raw html if it really needs to be formatted.

## RhythmBox

Its funny how quickly I've run through all the different GUI Applications that I actually ever use. Almost everything I use is just different command line programs running under XTerm. Whenever possible I prefer to install the 'No X' version of packages, like emacs, and use it from the terminal instead.

The last GUI app I can think of is that sometimes I'll open up Rhythmbox and use it to play my music collection while I work (What I still think of as my iTunes Library even though I'm not using iTunes anymore). More commonly I just stream stuff since I'm on an internet connection anyway, but I do still use this from time to time.

# Part 4: Monitors and Workspaces

For a long time I was a single monitor purist. While my co-workers monitors seemed to breed before them like rabbits (One guy in my office was using a six monitor system for a while), I insisted on a single monitor directly ahead of me. I've especially never liked my co-workers' dual monitor setups where they spend all day staring directly at the break between two monitors in front of them. These days several of them seem to be replacing their multi-monitor setups with a single monitor very wide monitor which curves around them and makes me nauseous just to look at.

Eventually however, with everyone around me discarding perfectly good monitors. I succumbed to the dual monitor disease myself. My setup is a bit idiosyncratic though, as I insist on a single primary monitor directly in front of me, and a secondary monitor off to the side, and perhaps a bit farther away. I still use only the single monitor to layout and interact with my windows, and leave the secondary monitor to things I'm not expecting to interact much with but just want to keep an eye on.

When working, I almost always have my secondary monitor showing my email client, so I can glance over and see if there's anything inbound I need to take a look at. The "Secure" email web client I'm forced to use is too secure for features like "Desktop Notifications". 'Secure' and 'Convenient ' tend to be opposing forces. At times I'll also have a Youtube video playing on that monitor that I'm sort of paying attention to as I work. Everything I drag to that monitor I set the 'All Desktops' flag in OpenBox, so the layout is the same on all workspaces. I tend to think of that monitor as being outside my workspace system as I change workspaces only on my primary monitor. Technically this isn't whats happening, but the end result is the same, and that's the mental model of it I have.

I've played with different numbers of workspaces, but seem to have settled on four, in a two by two grid. My primary workspace is named 'Main' and is in the upper-left. This is where I actually get programming, editing, and writing done. To the right, is 'Browsers', which funnily enough, is where I generally keep web browsers and documentation. My bottom left is 'Virtual Machines', and on the Bottom right, out of the way, is a desktop named 'Skype'. The workspace names describe what I usually run on each workspace, and correlate to what my hotkeys do (If I start up Skype from a hot key, it will start up on the Skype Workspace automatically). However I'm somewhat fluid on what I actually do with each. Sometimes its easier to bring a browser over to 'Main' or an XTerm to 'Browsers' if I'm following some directions or specification as I code, and sometimes will pick a non-main workspace to use for some temporary project. I tend to blog in emacs, for example, from the Browser workspace, along side the Browser showing previews of my Markdown. 

# Part 5: My Workflow

For work, my Hotkey Win-W (W for Work), spawns my two main XTerms on my 'Main' Workspace. The left one takes up most of the screen, ssh's into the main server I develop on, and attaches to my GNU Screen session I keep running their all the time. From there I have different windows in Screen that access my emacs editors, shells, and containers for actually getting work done. The servers my group uses are mostly Debian GNU/Linux, so regardless of what OS I run on my desktop, this is actually where I spend most of my time. On the right I have a narrower XTerm which does the same thing but connects me to an irssi running on the same server for my group's IRC channels. This is pretty much the same layout I used back with Notion before I switched away to Xfce, and it might as well be a tiled window manager, as their startup-scripts move and size these windows to these locations and I don't tend to move these windows around.

I really like having all my work stuff running on a server at work under GNU Screen. This means I can work one day in the office, and pick up the next at home, or from my laptop, and connect to the exact same editors, command prompts, etc exactly as I left them. Whenever I reboot my workstation, all I do when I get into X is run Win-V to connect the VPN, Win-W to open my work terminals, and I can continue right where I left off. I'll usually open Skype, Horizon, and Jira with three more hot keys and I've got everything I need ready to go. I've experimented with a single hot key to do all of this, but I actually prefer the granularity.

The scripts my hotkeys run all look more or less like this:
~~~
title='GNU Screen - Work Server'
echo -e '\033]2;'$title'\007'
wmctrl -r $title -e 0,1920,0,1411,1025
ssh -t <ip address> 'screen -r -x -p1 -A'
~~~

Xbindkeys would run it with 'uxterm -e ~/scripts/work.sh'. Basically open an XTerm, set the title, put it where it belongs, ssh in, and connect to my screen. Very comfortable. The versions I use on my laptop are a bit more complex, as they will detect if I have an external monitor attached, and arrange windows differently if I do. With my laptop, I use the external monitor as my 'primary' desktop and the laptop screen just for email, but If I'm using the laptop portably, the laptop becomes the primary monitor, and email goes with "Virtual Machines" on the lower left workspace.

# Conclusion

I'm always slowly tweaking things, often I'll try something out only to revert back as I find I don't really end up using it, as I have a few times with trying to get used to using tabbed with my xterms. I have a list of a few things I've wanted to set up and haven't yet taken the time to figure out. I want to be able to programmatically open a Window and set it to 'All Desktops' instead of just one workspace, but Openbox and wmctl don't seem to get along in this way. I also need to set up a hot key for enabling and disabling "Keep this Window on Top". For larger things, I may still decide to change our xfce4-panel for something else.

What I like most about the Linux / Unix desktop experience, is that people are always making new components, desktops, and themes, always adding new configuration options to their software, and even if no project exactly tickles my fancy, the ecosystem becomes a big smorgasbord to pick and choose from. Nothing I use is that far off the beaten path, there is a ton of software that works almost exactly how I want it to. Often I can configure in the changes I want, and if not, I can trivially swap a piece out for another project that does what I want. Add in a small amount of custom scripts I write myself, and I'm pretty much right where I want to be. Worse case scenario, everything I use is Open Source so if necessary I could crack it open and make the changes I want.

I read a lot of articles online that promote the idea of 'standardizing' the desktop experience to better compete with Apple or Microsoft. The argument seems to boil down to  "Everyone should just get on the same page and use the exact same thing exactly was some entity decides it should work and stop forking and configuring, and rewriting everything to 'scratch their own itches'". Fortunately anytime a movement appears that seems to have such a goal, it seems an equal-but-opposite movement occurs with an exodus back to freedom for which I'm grateful.

I like reading about what others are using and configuring for their own use. Hopefully someone out there didn't find this post to be a complete waste of time. Unlikely perhaps, but hopefully.













