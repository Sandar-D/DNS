The Internet Control Message Protocol (ICMP) is a network layer protocol used by network devices to diagnose network communication issues. ICMP is mainly used to determine whether or not data is reaching its intended destination in a timely manner. 

The primary purpose of ICMP is for error reporting. When two devices connect over the Internet, the ICMP generates errors to share with the sending device in the event that any of the data did not get to its intended destination.

A secondary use of ICMP protocol is to perform network diagnostics; the commonly used terminal utilities traceroute and ping both operate using ICMP:

- traceroute utility is used to display the [routing](https://www.cloudflare.com/learning/network-layer/what-is-routing/) path between two Internet devices. The routing path is the actual physical path of connected routers that a request must pass through before it reaches its destination. The journey between one router and another is known as a ‘hop,’ and a traceroute also reports the time required for each hop along the way.
- ping utility is a simplified version of traceroute. A ping will test the speed of the connection between two devices and report exactly how long it takes a packet of data to reach its destination and come back to the sender’s device. Although ping does not provide data about routing or hops, it is still a very useful metric for gauging the [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/) between two devices.

ICMP is not associated with a transport layer protocol such as [TCP](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/) or [UDP](https://www.cloudflare.com/learning/ddos/glossary/user-datagram-protocol-udp/). This makes ICMP a connectionless protocol: one device does not need to open a connection with another device before sending an ICMP message. Normal IP traffic is sent using TCP, which means any two devices that exchange data will first carry out a TCP handshake to ensure both devices are ready to receive data. ICMP does not open a connection in this way. The ICMP protocol also does not allow for targeting a specific port on a device.

