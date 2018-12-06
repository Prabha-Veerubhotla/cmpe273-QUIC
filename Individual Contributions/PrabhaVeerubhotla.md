**QUIC- Quick UDP Internet Connection**

**What is QUIC?**

QUIC is a protocol used for HTTP transport. It is a transport layer protocol, built in the application layer. 
It was first started in 2014 by Google. It is rapidly evolving.  If we open YouTube from chrome browser, there is more than 80 % chance, the connection is using QUIC. QUIC replaced a big chunk of Internet Stack. It replaces all of the TCP, TLS and most of the HTTP/2.0. QUIC is standardized at IETF, the organization which standardizes all the protocols like TCP. In the QUIC, the Google has introduced something new called QUIC Crypto Handshake, which combines the TCP 3-way handshake and TLS handshake. There have been several enhancements to TLS, and now we have TLS 1.3, which is almost similar to QUIC crypto handshake algorithm, but QUIC is still better. 

**Why QUIC?**

**TCP**:  

The connection is reliable, but very expensive. 
The number of connections which can be made are limited. 

**UDP**:  

It is connectionless, and not reliable. The sender will not even know, if the message has reached the receiver. 

**Why not build on top of TCP**? 

There were several projects like TCP fast open, that conducted experiments on building a new protocol, on top of TCP. But they were fruitless. It is very hard to change the middlebox, to be compatible with the changes built on top of TCP. The middlebox are the routers between 2 endpoints of the connection. One more hurdle is that the middleware can see the headers of the TCP packets, this sometimes cause the block in traffic. Also, it is very hard to deploy changes in the network using TCP.

**QUIC on top of UDP**: 

The team at Google has decided to explore the alternative, which is building a new protocol on top of UDP. Building on top of UDP gives them advantage of evolvability. It is also easy to deploy, when you are using UDP. The issue of NAT rebinding in UDP is solved by using a unique connection id for each connection of QUIC established. The headers of the packets are encrypted in QUIC, so the middleware cannot modify the traffic routes depending on the packet headers. As, mentioned previously, QUIC is built in the application layer. There are few reasons for this: 

1.	The application layer to allow Google to more quickly modify and deploy new transport-layer optimizations at scale.
2.	To avoid privacy violations as well as transparent proxying and content modification by middleboxes.
3.	To deploy on a massive scale quickly, without waiting for upgrading at OS level.
