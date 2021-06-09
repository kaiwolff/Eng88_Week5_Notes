Today, working with IP addresses.

The whole process is built on binary. Therefore, quick revision of binary.


## Layer 3 Addressing

Every computer has an IP address on the internet. For human-readable form, we put this in dotted decimal form. E.g 216.24.61.137

This is expressed in binary by machines, as a 32 bit unsigned integer, in 4 octets separated by dots.

Layer 3 addressing is divided into classes, see previously shown table for how these are split up. The more of the 32 bits is dedicated to the network address, the more networks are available, and the fewer hosts (as they have to share 32 bits).

### Calculating Network ID
 Can be done in several ways. Generally looking at first octet and totalling. Number ranges determine type of address. Can also look at starting binary numbers.0


### Subnet Mask

Subnet mask is a 32-bit address used to block or “mask” a part of the IP address to separate the network ID front eh host id.

Each host on a TCP/IP network requires a subnet mask.

Default subnet mask is used on TCP/|IP networks that are not divided into subnets

Bits that are part of the network address are set to 1, bits that are host ID are 0.

Always in two blocks, left block is 1, right block is 0

The “Broadcast IP” is the last available IP in the network. It cannot be allocated to a host. All the host bits will be 1 (network ID will be the same as before). Broadcast IP is used to send specific data to all the machines on the same network. Usually abbreviated to bcast IP.


Two sets of addresses are reserved. Anything starting with 10 is private

Loopback is 127.0.0.0 Net etc

Localhost is addressed as 127.0.0.1
This means that if you try to connect to 127.0.0.1, you will be immediately looped back to your own machine. This is the method for communication with your own machine.

Each machine considers itself to be 127.0.0.1

First UP all host as 0 except the last 1

Last IP all hsot id is 1 except last digit is 0

Broadcast - all host part is 1



The problem with classful allocation is that a lot of addresses are wasted for subnets with asmall number of hosts. We can either use IPv6 (which si a recent development, or use a subnet mask to render the entire setup “classless”


CISCO Packet Tracer - Running Notes

Cna be used to set up a simulation of a network, in preparation for setting up a virtualised network. Once we have got the hand of it, we will be given files and told to configure the network

Network Switch - A connection point. They understand mac addresses, but don’t work with IP addresses

Testing Connectivity, just ping via command prompt

Ping is a tool relying on a protocol called ICMP (Internet Control Message Protocol).

Whenever the switch has a packet to an address it doesn’t recognise, it will send to everybody and receive back the reply from the correct one. But, once received, it will learn the mac address, and remember it. Therefore, it will send a second ping only to the address it has identified as belonging to the correct MAC address.

ARP protocol

Stands for Address Resolution Protocol. ARP is used to find the mac address that belongs to an IP address. Basically asks device to answer with mac address if the IP address is correct. This allows the switch to identify the correct address. Other devices discard the received packet without replying. ARP cna also tell us if something has the same address on the network, in which case it stops the IP address from being assigned.

Notice that if two hosts are on different private networks by logic they will not be able to communicate, even if they are sharing a switch (as in, are physically connected)

IP Address Tools:

Ipconfig is a Microsoft utility which verifiues network connections and settings. On linux, it is not available, and the tool is ifconfig.

The option /all will show all the information

PIng OPtions

Ping -t: pings until stopped

-n count - number of requests to send


Usually get either:

Reply from - received by target host and answered

Timed out - sent but no reply received

Destination Host Unreachable

Route to system cannot be found.


Broadcast Domain vs. Collision Domain

Broadcast Domain is the region that a broadcast is heard on. When we send a broadcast on a network, it doesn’t usually reach the entire network, and will stop at the router instead. A BROADCAST CAN NEVER GO BEYOND A ROUTER, or generally, any layer 3 device. Therefore, if a router was dividing up parts of a network, the broadcast domain would not be the entire network. A switch will pass a broadcast signal on the other hand. NOTICE THAT A BROADCAST DOMAIN INCLUDES MACHINES THAT DON’T SHARE A NETWORK, BUT ARE ON THE SAME SWITCH FOR EXAMPLE.

Once we reach layer 3, the devices should understand IP addresses. Any device that “understands” IP addresses will not pass on a broadcast signal.



A general sending procedure inside a network. PC1 knows it’s own MAc and IP address, and the target address. It will need the target MAC address to send, and gets this either from its cache if it has already sent, or will attempt to use ARP protocol to resolve this issue. If it gets the MAC address, it can then send the package.

Devices cannot exchange traffic using layer 2 protocol unless their IP addresses are from teh same subnet (that is, if they are on the same network by logic)

On different IP subnets, we would need to exchange data through a Layer 3 device, such as a router.


Routers

 Layer 3 device that can be used to route traffic between subnets. A computer with two network cards can be turned into a router.  The use of the router is that as a layer 3 device, it “understands” IP addresses, and can therefore be used to communicate with devices outside of the subnet.

The Default Gateway

Teh device that will be used by the host to contact other networks. This is normally the router. The router will need and IP address on the network to be contactable. WE also have to make sure that the PC we are sending the data from knows where the default gateway is using the router’s IP address.

CANNOT ACCESS THE SAME DEFAULT GATEWAY FROM DIFFERENT BROADCAST DOMAIN DIRECTLY. 

A router can only have one IP per subnet. If subnet is split across broadcast domains, either change subnet of pc, or move the pc to be in the correct broadcast domain. Need to be LOGICALLY AND PHYSICALLY CONNECTED TO THE SAME BROADCAST DOMAIN.

Advice, keep default gateway IP should not be standardised. If the address has a pattern, then an attacker has an easier time finding the gateway, and exploring the network from there.



So, when sending between networks.

PC1 knows it’s mac, mask and IP, target mask and IP and gateway IP (same mask as its own). Will get gateway mac using arp or the cache, and the same. The target mac address would be obtained by the router using either the router’s cache or the arp protocol.

Recap on default gateway

The device that weill handle traffic between different IP networks.Devices have to be physically and logically connected under the 3rd layer in order to communicate on the same subnet.

Connecting between routers.

This exists as it’s own network. So the routers need IP addresses on this network. The routers will know onl ythe networks they are directly connected to initially (includign the router), so we need to set up static routing between the routers so that they know to pass any traffic headed for a particular network ID onto the other router. The other router’s IP address should be the “next hop” for a target network ID.
