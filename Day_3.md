# Day 5

## Quick Recap from yesterday
Router is a host. Switch is not. Switch will take a packet and forward it to everybody.

Routers do not pass on ARP requests, as they are layer 3 devices. Only layer 2 passes on broadcasts, because they do not understand IP addresses. 

If you were pinging a device from another network separated by a router, the router would just return its MAC address. The router handles the requests to another network separately. With static routing, the router will check if it has a network ID associated with the target IP address in its database, and then pass the request that way if needed.

### DHCP - Dynamic Host Configuration Protocol

Helps to configure many hosts, where setting up hosts individually may not be practicable. If you then needed to change a config aspect, you would need to do it to each host individually. DHCP solves this by having hosts be issued with a configuration. An extension of Boot Protocol (BOOTP). Listens to Port number 67/udp for server side, 68/udp for client side.

Doesn’t really “give” the configuration to a host, but rather “leases” the configuration. Configuration is temporarily allocated. Changes are updated as the lease is renewed, and this can lead to downtime if for example a default gateway is changed.

These messages are used to set up or alter the configuration of a hostVarious message types - a few examples include:

- DHCPDECLINE is sent if an address is found to already be in use
- DHCPRELEASE - client telling the server that they no longer need the elase and relinquishing the IP address
 - DHCPDISCOVER is a broadcast to locate available server 

DHCP also has other options to offer, e.g. Subnet Mask, the name server, Domain name, and so on. This is a convenient way of settign up a series of hosts which have the same networking details.



### Example Case.
New host, needs an IP address, sends DHCP DISCOVER. DHCP servers say what IP and for how long they can have it. It accepts a particular setup. ACK sent by DHCP (acknowledge) to confirm lease. The host now has that IP address for the length of the lease.


#### Ipconfig

This command has DHCP options. /renew to renew configs. /release to release current lease. Renew is particularly useful if more details have been added onto a DHCP server (such as a DNS server)

## Setup

Instead of assigning a static IP address to our PCs, we set them to look for the DHCP server instead.  The DHCP server will need a default gateway (if needing to use a router), IP address and subnet mask, as well as instructions as to what IPs to assign,w hat the default gateway should be and how many users can be assigned. Note that this can make direct pinging difficult, need a dns (domain name system/server?) for this.


Normally, server only operates within it’s own broadcast domain, but this can be circumvented.


**APIPA** - automatic private IP addressing. This is what we use to have private addresses set up.

Handy Tip, a Router can be set up to also act as a DHCP server. This means it automatically assigns IPs.

DSHCP is an APPLICATION LAYER PROTOCOL, NOT NETWORK LAYER.  Whenever you know that a protocol uses a port, that means it is an application layer protocol. Added a DHCP server to the packet tracer simulation.

### HTML/HTTP

Markup language. Not a part of the application layer. Data Layer, is concerned with data nad hwo data is written. Transferred via http (hyper text transfer protocol). Http was designed for the communication between a web server and a web browser (client-server protocol). Client is the browser, web server is the server.

By default the http protocol takes the tcp port number 80. Adding a http server to nbetwokr 1 in our packet tracer simulation 

#### HTTPS

More secure version of http. By default, works on port 443. 


### FTP

File Transfer Protocol. The standard protocol for file transfers on the internet. Uses port 20 and 21 by default. Also a client-server protocol. Usually, if downloading a file from browser, not using ftp, but rather http or https. However, if wantign something efficient and designed to transfer files, ftp is the wya to go.


### DNS
Short for domain name systems. The idea is that IPs are not easily human-readable, and that DHCP can further mess with the IP addresses. Hierarchical system. Each domain might have child domains, for example edu might have colorado, or uk might have ac , which might have shef, leading to shef.ac.uk. By default, uses Port 53/UDP

### Setting up DNS in packet tracer:

Instead of accessing the web server using an IP address, want to access using a URL. Need to add a DNS server. Once added, we can start adding names for IP addresses. Will need to make sure that the DHCP server is updated to include the DNS server. This way, any new devices will automatically receive the new DNS server address, and they can be reset using ”ipconfig  /renew”. 

Host then recognises name rather than IP, contacts DNS server, DNS server just returns the IP associated with the URL and the host then communicates with the target IP address.

Several of these processes can also be set up for several of these services. A lot of this can actually be done just through the router, for example.

It is possible to access the DNS server from another network, but the IP address needs to be set up correctly. You can set up a DHCP server that points to a DNS server etc outside of the network. ONLY THE PROTOCOLS THAT DEPEND ON BROADCASTS NEED TO BE CONTAINED IN THE SAME NETWORK

## Firewall

A device, system or software. Idea is to control access policy between networks. We will study

- How the firewall enforces a policy
- How to write these policies.

SSL filter the access to the network or network resources. When we want to protect, limit or grant access to network resources, we generally write an access list (ACL). Main options are either GRANT or DENY. An access list is a collection of the rules that grant or deny access to a certain resources. Could be anything: a file, a printer, etc. These take a different format depending on what the access list covers.

A firewall is a system or group of systems, that enforce these access lists.

All firewalls share some common properties:
should be resistant to network attacks
Can filter traffic
Can allow or deny access based on the rules they have
They enforce their access list
Can be either hardware or software


If software,can be installed on the host, server, etc.

If it is hardware, then it is a computer enforcing an access list/policy.


Firewalls can be used both to protect the users from accessing unsafe content, and to prevent unauthorised access from outside of the network.

Firewall Policies


Either allow something to pass, or not.
Packet Filtering Firewall

A firewall that controls the network access by analysing outgoing and incoming packets, based on information in the network and transport layers. Will check IP address, TCP or UDP and ports. Based on this information, will either allow or deny the traffic.

 ** Intercept packet -> Check network and transport layer info -> if in allow policy, let through. **


## Application Gateway Firewall

 Will check the application data. Working on more than just layer 3 and layer 4 data. Can also check the application (e.g. whether working with http, etc etc) More capabilities than packet filtering firewall.

### Firewall Zones

Will always have an inside (private) zone, and a public (untrusted) zone. Usually, you would trust your own network, and connected public networks are untrusted. Also have DMZ (“demilitarised zone”) which can be accessed from the outside in order to use certain services (for example, a web server might be added to the DMZ), since we might want part of our network to be accessible from the outside. Would allow the DMZ to be accessed from the outside, and from the inside, however the network cannot be accessed from the DMZ. The idea is to isolate anything interacting with the outside world from the rest of the world. This wya, if there are any breaches, they are contained within the DMZ.You can have several DMZs, and it is good practice to do so. Generally, the more separation between the network and the public domain, the more secure and trusted the DMZ is considered. You should not allow access from a less trusted network onto a more trusted network normally.

#### Wildcard Mask

Can be considered as the opposite of a subnet mask. In a wildcard mask, blank is 255.255.255.255
