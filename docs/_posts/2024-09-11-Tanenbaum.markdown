---
layout: post
title:  "Tanenbaum Networks"
date:   2024-09-10 19:29:05 +0100
categories: webdev
---

#### Client-server model 

Two processes (i.e. running programs) are involved, one on the client machine and one on the server machine. Communication takes the form of the client process sending a message over the network to the server process. The client process then waits for a reply message. When the server process gets a request, it performs the requested work or looks up the requested data and sends a reply. 

#### Broadcast links vs Point-to-point links

Two types of transmission technology in widespread use: broadcast links and point-to-point links. Point-to-point links connect individual pairs of machines. 

To go from the source to the destination on a network made up of point-to-point links, short messages, called packets, in certain contexts, may first have to visit one or more intermediate machines. Often multiple routes of different lengths are possible so finding good ones is important in point-to-point network. Point-to-point transmission with exactly one sender and exactly one receiver is sometimes called unicasting.

On a broadcast network the communication channel is shared by al the machines in the network; packets sent by any machine are received by all the others. An address field within each packet specifies the intended recipient. Upon receiving a packet, a machine checks the address field. If the packet is intended for the receiving machine, that machine processes the packet; if the packet is intended for some other machine, it’s just ignored. 

A wireless network is a common example of a broadcast link, with communication shared over a coverage region that depends on the wireless channel and the transmitting machine. As an analogy, consider someone standing in a meeting room and shouting “Watson come here”. Although he will be heard by many people only Watson will respond.

Broadcast systems usually also allow the possibility of addressing a packet to all destinations by using a special code in the address field. When a packet with this code is transmitted, it is received and processed by every machine on the network. This mode of operation is called broadcasting. Some broadcast systems also support transmission to a subset of the machines, which is known as multicasting.

#### Types of networks (PAN, LAN, MAN, WAN) and Internet definition

PAN (personal area networks) let devices communicate over the range of a person. Computer with attached monitor, mouse, keyboard, printer by Bluetooth. 

LAN (local area networks) privately owned network that operates within or nearby a single building like a home, office or factory. LANs are widely used to connect computers and shared printers and exchange information. In most cases, each computer talks to a device in the ceiling (wireless router or access point). It relays packets between the computers and also between them and the Internet. Being the AP is like being a popular kid at school because everyone wants to talk to you.
Standard for wireless LANs is called IEEE 802.11 popularly known as WiFi. The topology of many wired LANs is built from point-to-point links. IEEE 802.3 popularly called Ethernet is the most common type of wired LAN.

MAN (metropolitan area network) covers a city. The best known examples are cable television networks available in many cities. 

WAN (wide area network) spans a large geographical area, often a country or continent. In most WANs hosts are connected by transmission lines and switching elements. Transmission lines move bits between machines. They can be made of copper wire, optical fiber or radio links. Most companies lease the lines from a telecommunications company. Switching elements (switches) are specialised computers that connect two or more transmission lines. When data arrives on an incoming line, the switching element must choose an outgoing line on which to forward them. 
Unlike LAN, the routers here will usually connect different kinds of networking technology. The networks inside the offices may be switched Ethernet, while the long-distance transmission lines may be SONET links. Many WANs may be composite networks that are made up of more than one network. Rather than lease dedicated transmission lines, a company might connect its offices to the internet. It allows the connections to be made between the offices as virtual links that use the underlying capacity of the Internet. This arrangement is called VPN. The subnet may be run by a different company (internet service provider). The subnet operator will connect to other customers too, as long as they can pay and it can provide service. The subnet operator will also connect to other networks that are part of the internet. 
WANs can be satellite systems where each computer on the ground has an antenna through which it can send data to and receive data from to a satellite in orbit. All computers can hear the output from the satellite, and in some cases they can also hear the upward transmissions of their fellow computers to the satellite as well. Satellite networks are inherently broadcast.
The cellular telephone network is another example of a WAN that uses wireless technology. Each cellular base station covers a distance with a range measured in kilometres. The base stations are connected to each other by a backbone network that is usually wired. 

A collection of interconnected networks is called an internet. The internet uses ISP networks to connect enterprise networks, home networks and many other networks. When two networks are interconnected? Different organisations have paid to construct different parts of the network and each maintains its part. The underlying technology may be different in different parts (e.g. broadcast vs point-to-point and wired va wireless). Gateway is a machine that makes a connection between two or more networks and provides the necessary translation in terms of hardware and software. Gateways are dist inguished by the layer at which they operate in the protocol hierarchy.

#### Network models - general

**Connection-oriented service vs Connectionless service**

Layers can offer two different types of service to the layers above them: connection-oriented and connectionless. Connection-oriented service is modelled after the telephone system. To talk to someone, you pick up the phone, dial the number, talk, and then hang up. Similarly, to use a connection-oriented network service, the service user first establishes a connection, uses the connection, and then releases the connection. The connection acts like a tube: the sender pushes objects (bits) in at one end, and the receiver takes them out at the other end. In most cases the order is preserved so that the bits arrive in the order they were sent. In some cases, when a connection is established, the sender, receiver and subnet conduct a negotiation about the parameters to be used, such as maximum message size, quality of service required etc. Typically one side makes a proposal and the other side can accept it, reject it, or make a counter proposal. 

Connectionless service is modeled after the postal system. Each message (letter) carries the full destination address, and each one is routed through the intermediate nodes inside the system independent of all the subsequent messages. There are different names for messages in different contexts, a packet is a message at the network layer. It is possible that the first message sent can be delayed so that the second one arrives first. Some services are reliable - they never lose data. Usually, a reliable service is implemented by having the receiver a knowledge the receipt of each message so the sender is sure that it arrived. The acknowledgement process introduces overhead and delays, which are sometimes undesirable.

**Service definition and an example of connection-oriented service**

A service is specified by a set of operations available to user processes. If the protocol stack is located in the operating system, as it often is, the primitives are normally system calls. These calls cause a trap to kernel mode, which then turns control of the machine over to the operating system to send the necessary packets. 

An example of connection-oriented service. First, the server executes LISTEN to indicate that it’s prepared to accept incoming connections. A common way to implement listen is to make it a blocking system call. After executing this operation, the server process is blocked until a request for connection appears. Next, the client process executes CONNECT to establish the connection with the server. Next, the client process executes CONNECT to establish a connection with the server. The CONNECT call needs to specify who to connect to, so it might have a parameter giving the server’s address. The operating system then typically sends a packet to the peer asking it to connect, the client process is suspended until there is a response. When the packet arrives at the server, the operating system sees that the packet is requesting a connection. It checks to see if there is a listener and if so it unblocks the listener. The server process can then establish the connection with the ACCEPT call. This sends a response back to the client process to accept the connection. The arrival of this response then releases the client. At this point the client and the server are both running and they have a connection established. Analogy from real life - a customer calling a company customer service manager. At the start of the day, the service manager sits next to his telephone in case it rings. Later, a client places a call. When the manager picks up the phone, the connection is established. Next, the server executes RECEIVE to prepare to accept the first request. Normally, the server does this immediately upon being released from the LISTEN, before the acknowledgement can get back to the client. The RECEIVE call blocks the server. Then the client executes SEND to transmit its request followed by the execution of RECEIVE to get the reply. The arrival of the request packet at the server machine unblocks the server so it can handle the request. After it has done the work, the server uses SEND to return the answer to the client. The arrival of this packet unblocks the client, which can now inspect the answer. If the client has additional requests, it can make them now. Six packets are required to complete this protocol, why not use a connection less protocol instead. In the face of large messages on either direction, transmission errors and lost packets, a simple request-reply protocol over an unreliable network is not adequate.

A service is a set of operations that a layer provides to the layer above it. A service relates to an interface between two layers, with the lower layer being the service provider and the upper layer being the service user. A protocol is a set of rules governing the format and meaning of the packets, or messages that are exchanged by the peer entities within a layer. Entities use protocols to implement their service definitions. They are free to change their protocols, provided they do not change the service visible to their users.

**Layers**

The entities comprising the corresponding layers on different machines are called peers. The peers may be software processes, hardware devices or even human beings. Each layer passes data and and control information to the layer immediately below it, until the lowest level is reached. Layer 1 is the physical medium through which communication occurs. The lower levels are frequently implemented in hardware or firmware. Different hosts use different implementations of the same protocol (often written by different companies). The protocol itself can change in some layer without the layers above and below it even noticing. 

Design issues for the layers. Reliability is the design issue of making a network that operates correctly even though it is made up of a collection of components that are themselves unreliable. There is a chance that some bits of a packet travelling through the network will be received damaged (inverted) due to random wireless signals, hardware flaws, software bugs etc. One mechanism for finding errors in received information uses codes for error detection. Information that is incorrectly received can then be retransmitted until it is received correctly. More powerful codes allow for error correction, where the correct message is recovered from the possible incorrect bits that were originally received. Both of these mechanisms work by adding redundant information. They are used at low layers, to protect packets sent over individual links, and high layers, to check that the right contents were received. Another reliability issue is finding a working path through a network. Often there are multiple paths between a source and destination, and in a large network, there may be some links or routers that are broken. Suppose that the network is down in Germany. Packets sent from London to Rome via Germany won’t get through, but we could instead send packets from London to Rome via Paris. The network should automatically make this decision. This topic is called routing. Every layer needs a mechanism for identifying the senders and receivers that are involved in a particular message. This mechanism is called addressing or naming, in the low and high layers respectively.

#### ISO OSI (Open Systems Interconnection) 

**The Physical Layer**

Transmitting raw bits over a communication channel. We should make sure that when one side sends a 1 bit it is received by the other side as a 1 bit, not as a 0 bit. Typical questions here are what electrical signals should be used to represent a 1 and a 0, how many nanoseconds a bit lasts, whether transmission may proceed simultaneously in both directions, how initial connection is established, how it is torn down when both sides are finished, how many pins the network connector has and what each pin is used for. 

**The Data Link Layer**

Transforms a raw transmission facility into a line that appears free of undetected transmission errors. It has the sender to break up the input data into data frames (typically a few hundred or a few thousand bytes) and transmit the frames sequentially. If the service is reliable, the receiver confirms the correct receipt of each frame by sending back an acknowledgement frame.

Another issues here and most of the higher layers as well: how to keep a fast transmitter from drowning a slow receiver in data. Some traffic regulation mechanism may be needed to let the transmitter know when the receiver can accept more data. 

Broadcast networks have an additional issue in this layer: how to control access to the shared channel. A special sublayer of the data link layer, the medium access control sublayer, deals with this problem. 

**The Network Layer**

Determines how packets are routed from source to destination. Routes can be based on static tables that are “wired into” the network and rarely changed or more often they can be updated automatically to avoid failed components. They can also be determined at the start of each conversation, for example, a terminal session, such as a login to the remote machine. Finally, they can be highly dynamic, being determined anew for each packet to reflect the current network load. 

If too many packets are present in the subnet at the same time, they will get in one another’s way, forming bottlenecks. Handling congestion is also a responsibility of the network layer, in conjunction with higher layers that adapt the load they place on the network. The quality of service provided (delay, transit time, jitter) is also a network layer issue. 

When a packet has to travel from one network to another to get to its destination,many problems can arise. The addressing used by second network can be different from that used by the first one. The second one may not accept the packet at all because it’s too large. The protocols may differ and so on. It’s up to the network layer to overcome these problems to allow heterogenous networks to be interconnected. 

In broadcast networks, the routing problem is simple so the network layer is often thin or even nonexistent. 

**The Transport Layer**

Transport layer accepts data from above it, splits it up into smaller units if need be, passes these to the network layer and ensures that all the pieces arrive correctly at the other end. All this must be done efficiently and in a way that isolates the upper layers from the inevitable changes in the hardware technology over the course of time. 

The transport layer also determines what type of service to provide to the session layer and to the users of the network. The most popular type of transport connection is an error-free point-to-point channel that delivers messages or bytes in the order in which they were sent. However, other possible kinds of transport service exist, such as the transporting of isolated messages with no guarantee about the order of delivery, and the broadcasting of messages to multiple destinations. The type of service is determined when the connection is established. By error-free channel people mean the error rate is low enough to ignore errors. The transport layer is a true end-to-end layer, it carries data all the way from the source to the destination. In other words, a program on the source machine carries on a conversation with a similar program on the destination machine, using the message headers and control messages. In the lower layers, each protocol is between a machine and its immediate neighbours, and not between the ultimate source and destination machines, which may be separated by many routers. 

**The Session Layer**

Allows users on different machines to establish sessions between them. Sessions offer various services, including dialog control (keeping track of whose turn it is to transmit), token management (preventing two parties from attempting the same critical operation simultaneously) and synchronization (check pointing long transmissions to allow them to pick up from where they left off on the event of a crash and subsequent recovery)

**The Presentation Layer**

Unlike the lower layers, which are mostly concerned with moving bits around, it is concerned with the syntax and semantics of the information transmitted. In order to make it possible for computers with different internal data representations to communicate, the data structures to be exchanged can be defined in an abstract way, along with a standard encoding to be used “on the wire”. The presentation layer manages these abstract data structures and allows higher level data structures (e.g. banking records) to be defined and exchanged. 

**The Application Layer**

It contains a variety of protocols, one widely used is HTTP (Hypertext Transfer Protocol). When a browser wants a web page, it sends the name of the page it wants to the server hosting the page using HTTP. The server then sends the page back. Other application protocols are used for file transfer, electronic mail, and network news.

#### History of internet

The grandparent of all WAN is ARPANET. It was a research network sponsored by the US Department of Defense. It connected hundreds of universities and government institutions, using leased telephone lines. When satellite and radio networks were added later, the existing protocols had trouble interworking with them, so a new reference architecture was needed. This architecture later became known as the TCP/IP reference model, named after its two primary protocols. Given the DOD worry that some of its precious hosts, routers and gateways might get blown to pieces at a moments notice by an attack from the Soviet Union, another major goal was that the network able to survive loss of subnet hardware, without existing conversations being broken off. The DOD wanted  connections to remain intact as long as the source and destination machines were functioning, even if some of the machines or transmission lines in between were suddenly put out of operation. 

#### The TCP/IP model

![TCP IP Model]({{ site.baseurl }}/assets/TCP_IP_Model_Protocols.jpg)

