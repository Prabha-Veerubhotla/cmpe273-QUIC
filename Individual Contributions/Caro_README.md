**A few of the drawbacks of QUIC:**

**1.** **Unfairness**

A transport protocol must be fair in terms of bandwidth consumption with respect to other transport protocols. But it is seen than QUIC dominates most of the bandwidth when competing with TCP. This happens even when QUIC is competing with more than 1 TCP connection, QUIC still happens to consume almost half the bandwidth. This happens because QUIC adjusts its congestion window more rapidly and with steep slopes with respect to change of window sizes.

**2.** **QUIC Performance – Object sizes and Object Number**

QUIC performs better when the object sizes are small, this is mainly because of the 0-RTT connection. But when the object sizes are large, the initial RTT connection time becomes an insignificant part of the whole req-res transportation time. Thus, we see little to no benefits in such cases.

When it comes to the number of objects being requested, TCP out performs QUIC. This is because of the Congestion Control algorithm, which is the same as the one used in TCP. But in case of QUIC, the protocol makes an early exit from the Hybrid slow start state, assuming the pathway to be congested due to the min RTT as observed by the sender.

**3.** **Packet – Reordering**

QUIC under performs TCP when it comes to packet-reordering. This seems to be because, QUIC uses a fixed NACK threshold. When packets are re-ordered deeper than the threshold value, it false detects it to be a case of packet loss. TCP on the other hand uses DSACK algorithm, which detects packet reordering and adjusts its NACK threshold accordingly.

**4.** **Performance on mobile devices**

It is seen that the benefits of QUIC are lost or diminished in mobile devices. On analyzing the state diagram transitions [1], for both Desktop and mobile, we see that QUIC spends a comparatively larger amount of time in the Application Limited state whilst operating on mobile devices. This happens because while the application now receives more packets, it is limited in resources to process these packets in time. Due to this it is unable to use the full benefits of QUIC protocol.
