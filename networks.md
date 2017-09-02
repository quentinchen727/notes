## Differences between hub, bridge, switch and router
1. hub is the simplest one. It cannot filter data so data packets are sent to all connected devices. It is used on small networks.
2. Bridge connects a LAN to another LAN that uses the same protocol, having a single incoming and outgoing ports and filtering traffic based on MAC.
3. A switch when compared to bridge has multiple ports. It can perform error-checking. It can support both layer 2 and layer 3 depending on the type of switch.
4. Router forwards packets based on address, which allow the network to go across different protocols. Routers forward packets based on software while a switch(Layer 3) od using hardware called ASIC(application specific integrated circuits). Router supports different WAN technologies but switches do not. Besides, wireless routes have access points built in.

## UDP and TCP socket
TCP server: socket->bind->listen->accept->send/recv
TCP client: socket->connect->send/recv
UDP server: socket->bind->send/recv
UDP client: socket->send/recv

