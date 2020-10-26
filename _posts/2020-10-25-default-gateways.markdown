---
layout: post
title:  Default Gateways and Static Routes
date:   2020-10-25 16:47:00 -0400
---

When setting up networking on a computer, configuring multiple default gateways tells the system "To send traffic to an IP that doesn't match any of the configured subnets, any of these gateways will work, pick one". That's rarely what the user configuring the system means to express. Commonly they have multiple networks and for each network, traffic matching the netmask should be transmitted to the configured directly to the IP on that network, and traffic that doesn't match the netmask but "belongs" on that network should be sent to that network's gateway; but nothing about "default gateway"  has told the device what traffic "belongs on each network". Of course the right way to configure this would be to modify the routing table to tell the computer what network traffic to send to which gateway.

At $WORK we develop and support a Linux based device which has a web based configuration interface we've developed. One common support issue we run into is a complaint about the device not responding to certain network requests, and when we troubleshoot, we discover the user has configured multiple default gateways on different Ethernet devices. When the device needs to send to an IP that's not on one of its local subnets, it sends the traffic to one of the default gateways, which then has no idea what to do with that traffic, since it should have gone to the other default gateway.

The right way to configure this setup would be to set a static route for each gateway on each network interface, routing only the traffic that that gateway will be able to route to that gateway, and our web interface makes this possible by allowing the configuration of arbitary static routes. However many customer's use this feature untouched and instead just  populate a gateway per network interface, and leave it at that, never telling the device what traffic should go to each network-- just an IP, netmask, and a gateway. The netmask defines the local subnet of course, but says nothing about the full extent of the network reachable through the gateway.

Its easy to see why this happens, "Network Settings" generally consists of an IP, netmask, broadcast address, and a gateway. Whoever is installing the device gets the network settings from their network administrator-- adds them in the GUI and calls it a day. If they have a second network to add (possibly with a seconds network administrator) they get an IP, broadcast, netmask, and gateway for that one and add it in the GUI and call it another day. 
e. The post-it note they are given by the network administrator with the network settings for the device, doesn't mention anything about configuring a static route, just the IP, netmask, broadcast address, and gateway for the network. This information isn't wrong, its just that  network administrators tend to  consider their network the entire world, so these settings won't play nice with multiple networks.

Many years ago during a revamp of our product, I proposed adding a 'routemask' field to each network in the GUI -- the subnet mask telling what IPs could be reached directly on the network and the routemask telling what IPs could be reached via the gateway configured for the network. With this information, we could insert the correct routing statements on the back end and everything would work as expected. This was shot down though, because no one would understand the terminology, and if a user asked their network administrator for the 'routemask' of their network, they'd probably get little more than a funny look. Its too bad since in 90% of the cases, if there was a routemask field in the GUI for the network interface, and the network administrators considered 'routemask' something they scribbled down on the post-it when handing out network settings, it would be enough in 90% of our installs; with only a few requiring more complex setup then this.

I think the root cause of this is that for the average user, network settings are something they are used to, and understand, but routing tables are not, so they aren't likely to even think of routing tables when setting up their network. Its not part of the all important post-it.

I did finally implement a pop-up if the user tries to save a network configuration with multiple default gateways informing the user that 'this probably isn't what you want' and telling them how to configure static routes. Hopefully this helps.



