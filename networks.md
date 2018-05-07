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

## URL encoding (percent encoding)
Reserved characters used in unreservered purpose must be url encoded, using hexidemical bytes in ASCII preceded by %.
The application/x-www-form-urlcoded type: 
when data that has been entered into HTML forms is submitted, the form field names and values are encodeded and setn to the server in http request message using method GET or POST. The rncoding used by default is based on an early version of the general URI percent-encoding rules, with a number of modifications such as newline normalization and replace spaces with + instead of %20. The media type of data encoded this way is application/x-www-form-urlencoded, and it is currently defined in HTML and XForms specs. In addtion, the CGI specs contains rules for how web servers decode data of this type and make it available to applications.
When HTML form data is sent in an http get request, it is included in the query component of the request URI using the same syntax described above. When sent in an http post request or via email. the data is placed in body of the message, and application/x-www-form-urlencoded is included in the message's Content-type header.

## APIPA: automatic private ip addressing
If the client is unable to find DHCP information, it uses APIPA to automatically configure it self with an IP address. The range is 169.254.0.1 to 169.254.255.254. It also configures itself with a default class B subnet mask of 255.255.0.0. A client uses the self-configured IP address until a DHCP server becomes available.
