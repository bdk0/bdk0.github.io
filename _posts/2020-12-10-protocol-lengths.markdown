---
layout: post
title:  Protocol Designers, Give us Length Fields!
date:   2020-12-10 16:47:00 -0400
---

This is a bit of a rant, but at ${WORK} I am currently implementing a network protocol from a specification provided by another company. They designed a proprietary protocol for integrating to their device and implemented it. Our product is supposed to communicate with theirs to receive a real-time feed of data which we will then process and do stuff with, which requires us to implement the their protocol.

This is something I do a lot of, so perhaps I'm a bit of a connoisseur of network protocols and their documentation at this point, and this one has me ripping my hair out. Mainly I want to talk about one small change to their protocol that would have made it so much more ergonomic and less brittle to work with.

When sending data over a network connection, you need to provide some way for the receiver to figure out when one message ends and the next one starts. In UDP based protocols, this is often just reduced to "Each message is its own UDP packet", but for TCP, the receiving software can't see packet boundaries unless they are using raw sockets or libpcap. The receiver just sees a stream of bytes and doesn't know how the sender packetized it, so the protocol must define an  in-band method to provide that information. The two most commonly seen methods are a certain character sequence that marks the end or beginning of a packet, like two newlines in SIP, or a length field that prefixes every payload, as with RTP interleaved inside a RTSP stream (just to bring up two random examples).

Once the receiver is certain that they have received an entire packet of data, they need to extract the useful data out of it. If the data is simple enough it could just be sent as a bunch of fixed length fields in a preset order.  More complex data could be formatted as something like JSON or XML, or ASN.1, or something like ProtocolBuffers which also provides a solution to the previous issue of message boundaries as well as decoding the message.

In this case, the protocol designers decided to roll their own, each packet starts with a "Packet Type" byte, and then the payload format depends on what type the packet is, and is a combination of fixed length fields (like a 5 byte timestamp, or a 4 byte flag field), as well as variable length integers and  Pascal style strings. There are also arrays of further structures where the first byte tells how many entries are in the array, and then each array element itself starts with a 'sub-block type' which determines the (variable length) length of the sub-block.

Technically this works fine: My code can read the PacketType, then scan the message byte by byte and make sense of all the components. However one piece of missing information would have made this protocol so much easier and better: They should have included a Packet Length after the Packet Type. Yes, this isn't strictly necessary as we can figure out the length of each message as we parse it by knowing the fields that make it up and the rules for reading lengths of individual fields as we go. But this one oversight has several problematic consequences.

- There's no way to skip over a message and get the next one without implementing all the parsing code for it. In our case, only about half of the Packet Types and sub-block types have information relevant to us, but I have to implement parsing code for all of them, or one message we didn't know how to figure out the length of would throw off our synchronization ad we'd have no idea of which future byte represents the Packet Type for the next Message.

- Any parsing error is catastrophic, if we make any error in figuring out the length of a packet, it throws off synchronization, and we can't find the next Message in the stream. This has already been a problem for me as I've found at least one case so far where their implementation and their specification didn't match-- the Specification claimed a 16-bit GUID field in one of their messages that didn't actually exist on the wire in all cases. Not good. If they had a length field, it would be trivial for us to detect a problem like this and resynchonize and parse future messages properly.

- There is no backwards or forwards compatibility between protocol versions. If they add any new Packet Types in the future, our working implementation will silently break the first time we receive one of these new Packet Types, as we will have no ability to skip it and move on as we'd want to do. We are tied to only working with the exact protocol version we implemented against. This is even worse in this case as their protocol also doesn't provide a version field, so we won't even know anything is wrong until our parsing goes off the rails.

In summary, if you are designing a protocol, please be kind to your consumers and give them explicit length fields rather than make them calculate them.
















