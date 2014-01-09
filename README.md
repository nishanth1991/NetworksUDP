NetworksUDP
===========

User Datagram Protocol implementation for GINI , COMP-535 Mcgill 1st semester

For listening on a port

nc -l -p port_number
example- "nc -l -p 9050"

For sending pata to a port 

nc ip_adress -p destination_port -s source_port
example- "nc 192.168.2.3 -p 9050 -s 9051"
The source port is an optional field, if nothing is specified, it defaultly takes a random free port and sends data from it
example - "nc 192.168.2.3 -p 9050"


For closing a port

nc close port_number
example- "nc close 9050"

For traceroute
traceroute ip_address
example- "traceroute 192.168.2.3"



whenver an nc command is parsed from the terminal, either a udp message can be sent, or a port can be opened for listening or a port can be closed. If a port is opened and and a message is tried to be sent
on the same, a "port in use" error is thrown. 

sendCmd() function is called whenever nc is parsed and function are called accordingly. 

Once a message is sent , udpoutputprocessing() function creates the udp datagram, creates a pseudo header, calculates the checksum on the pseudo-header and then sends to IP for processing.

If a port is opened for listening, it checks whether the port is in use[checkforopenport(port)], if not creates a queue corresponding to the port and the queue is continously left for scanning for any additions, unless it is exclusively called for the port being closed. 

Once a IP layer receives a UDP packet, it calls the ProcessUDPpacket(), which creates a pseudo header and calculates the checksum, if the checksum is not zero, the packet is rejected. Else checks whether the destined port is not open, if not the DestinationUnreachable() function is called to send the Destination unreachable message. The getinputqueue(port) gets the queue corresponding to the port  and  enqueueudp(queue,udphdr->data,length) enqueues the packet data to the queue. 

Note: The functions used in the implementation are the first 3 functions in cli.c and the whole udp.c and the queue.c file and someparts of ip.c file, to by pass retrieving the pseudo header. All the functions in these files have been commented above the function to define their exact functionality. 
