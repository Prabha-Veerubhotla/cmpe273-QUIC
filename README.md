# cmpe273-QUIC

Team Members:

* Vijaya Lakshmi Prabha Veerubhotla

* Caroline Chandraguptharajah

* Akshay Anil Pagar

* Dhruvil Jiteshbhai Patel


### QUIC, a multiplexed stream transport over UDP

QUIC is an experimental transport layer network protocol initially designed, implemented, and deployed by Google in 2012, and announced publicly in 2012 as experimentation broadened. QUIC is a new transport which reduces latency compared to that of TCP. On the surface, QUIC is very similar to TCP+TLS+HTTP/2 implemented on UDP. Because TCP is implemented in operating system kernels, and middlebox firmware, making significant changes to TCP is next to impossible. However, since QUIC is built on top of UDP, it suffers from no such limitations. 

QUIC's main goal is to improve perceived performance of connection-oriented web applications that are currently using TCP. It does this by establishing a number of multiplexed connections between two endpoints over User Datagram Protocol (UDP).


