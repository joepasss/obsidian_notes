The most popular protocol suite in use today is Transmission Control Protocol/Internet Protocol (TCP/IP).

## The TCP/IP Model

The TCP/IP model, also referred to as the TCP/IP Model, is a four-layer model similar in concept to the seven-layer OSI Reference Mode.

![[스크린샷 2025-04-21 09.45.04.png]]

but the TCP/IP model combines multiple layers of the OSI model, Several protocols direct how computers connect and communicate using TCP/IP within the TCP/IP protocol suite, and each protocol runs on different layers of the TCP/IP Model.

### Application Layer

=> (OSI) application, presentation, session layer combined

It is responsible for making network requests (sending computer) or servicing requests (receiving computer). So for example, when a user submits a request from a web browser, the web browser responsible for the submission of the request is running at the application layer. When that web request reaches the web server, the web server running at the application layer accepts that request.

Following are popular application layer protocols:
* Hypertext Transfer Protocol (HTTP)
* Hypertext Transfer Protocol Secure (HTTPS)
* Simple Mail Transfer Protocol (SMTP)
* Post Office Protocol (POP)
* File Transfer Protocol (FTP)
* Domain Name System (DNS)
* Remote Desktop Protocol (RDP)

### Transport Layer

responsible for both connection oriented communication (a session is established) and connectionless communication (a session is not established)

When the request comes down from the application layer, a transport protocol is chosen to handle it. The two transport protocols in TCP/IP are the Transmission Control Protocol (TCP) and the User Diagram Protocol (UDP). As data passes down the layers of the TCP/IP model, if connection-oriented communication is required, TCP is used as the transport layer protocol, but if connectionless communication is required, UDP is used.

### Internet Layer

responsible for logical addressing, routing, and converting the logical address to the physical Media Access Control (MAC) address.

* Internet Protocol (IP)
	* Handles all logical addressing (IP addresses) and routing functionality
* Address Resolution Protocol (ARP)
	* Converts the logical address to physical address (MAC)
* Internet Control Message Protocol (ICMP)
	* Reports the status and any errors in the TCP/IP protocol suite

### Network Access Layer

(network interface layer, link layer)

The network access layer corresponds to the data link and physical layers of the OSI model.

Examples of network access layer components are network architectures, physical addressing, cables, connectors, network media, and any other layer 2 and layer 1 network components.

### Transport Layer Protocols

*transport layer includes the TCP and UDP protocols*

### Transmission Control Protocol (TCP)

TCP is responsible for providing connection-oriented communication and for ensuring delivery of the data (reliable delivery).

Connection-oriented communication involves first establishing a connection between two systems and then ensuring that data sent across the connection reaches the destination.

TCP will make sure that the data reaches its destination by retransmitting any data that is lost or corrupt. TCP is used by applications that require a reliable transport, but this transport has one more overhead than a connectionless protocol because of the construction of the session and the monitoring and retransmission of any data across that session.

Another factor to remember about TCP is that the protocol requires that the recipient acknowledge the successful receipt of data. all the acknowledgments, or ACKs, generate additional traffic on the network, which redeuces the amount of data that can be passed within a given time frame. The extra overhead involved in the creation, monitoring, and ending of the TCP session is worth the certainty that TCP will ensure that the data will reach its destination.

TCP ensures that data is delivered by using sequence numbers and acknowledgment numbers. A sequence number is a number assigned to each piece of data that is sent. After a system receives a piece of data, it acknowledges that it has received the data by sending an acknowledgment message back to the sender, with the original sequence number being the acknowledgment number of the reply message.

### TCP three-way handshake

![[스크린샷 2025-04-21 23.46.24.png]]

Before a system can communicate over TCP, it must first establish a connection to the remote system. It does this through the TCP three-way handshake

1. SYN
	* The sending system sends a SYN message to the receiving system
	* Each packet sent is assigned a unique sequence number
	* The SYN message contains the initial sequence number (ISN). Which is the first sequence number to be used.
2. SYN/ACK
	* This phase acknowledges the first message, but at the same time indicates its initial sequence number.
3. ACK
	* The final phase is the acknowledgment message that acknowledges that the pakcet sent in the second phase has been received.

> COMPUTER A -> 시퀀스 넘버(ISN)이 123인 SYN을 보냄
> COMPUTER B -> 시퀀스 넘버(ISN)이 124인 ACK를 보냄 (ISN 123인 패킷을 받았다는 의미)
> COMPUTER B -> 시퀀스 넘버(ISN)이 326인 SYN을 보냄
> COMPUTER A -> 시퀀스 넘버(ISN)이 327인 ACK를 보냄 (ISN 326인 패킷을 받았다는 의미)

### Disconnecting from a TCP session

Just as TCP uses a three-way handshake process to create connection between two systems that want to communicate, it also has a process for a participant to disconnect from the conversion

![[스크린샷 2025-04-22 00.21.49.png]]

*polite* way
> COMPUTER A -> FIN 플래그 SYN
> COMPUTER B -> FIN 에 해당하는 ACK, FIN 플래그 SYN
> COMPUTER A -> 받은 FIN 에 해당하는 ACK

*impolite* manner

You can end the conversation impolitely by hanging up without saying goodbye. => sending a TCP message with RST(reset) flag set.

### TCP Ports

When applications use TCP or UDP to communicate over the network, each application must be uniquely identified by a unique port number on the system.

A port is an address assigned to the application. When a client wants to commnuicate with one of these applications (also known as services), it must send the request to the appropriate port number on the system.

**Common TCP port**
![[스크린샷 2025-04-22 00.28.12.png]]

Both TCP and UDP support up to 65,536 port numbers that range from 0 to 65,535.

* Well-known port
	* 0 to 1023
	* assigned by the internet Assigned Number Authority (IANA)
	* HTTP, DNS, and SMTP
* Registered
	* 1024 to 49151
	* assigned by IANA for proprietary applications
	* MS SQL Server, Adobe Shockwave, Oracle, ....
* Dynamically assigned
	* 49152 to 65535
	* dynamically assigned by the system
	* typically used by client software such as web browser, FTP or telnet client

### TCP Flags

TCP uses TCP flags to identify important types of packets.

**common TCP flags**
* SYN
* ACK
* PSH
	* push flag is designed to force data on an application
* URG
	* urgent flag
* FIN
* RST

### TCP Header

Every packet that is sent using TCP has a TCP header assigned to it, which contains TCP-related information such as the source port, destination port, and the TCP flags.

**TCP Header**
![[스크린샷 2025-04-22 00.35.14.png]]

* Source Port
* Destination Port
* Sequence Number
* Acknowledgment Number
* Offset
	* This 4-bit field indicates where the data begins
* Reserved
	* This 6-bit field is always set to 0 and was designed for future use
* Flags
	* This 6-bit field where the TCP flags are stored. There is a 1-bit field for each of the TCP flags mentioned earlier in this section
* Window Size
	* determines the amount of information that can be sent before an acknowledgment is expected.
* Checksum
	* verify the integrity of the TCP header
* Urgent pointer
	* if URG flag is set and is a reference to the last pice of information that was urgent.
* Options
	* variable length field that specifies any additional settings that may be needed in the TCP header

### User Datagram Protocol (UDP)

UDP is used by applications that are not concerned with ensuring that the data reaches the destination system. UDP is used for connectionless communication (unreliable), which means that data sent to the destination and no effort is made to track the progress of the packet or determine whether it has reached the destination.

### UDP Ports

![[스크린샷 2025-04-22 00.42.55.png]]

### UDP Header

![[스크린샷 2025-04-22 00.44.08.png]]

* Source port
* Destination port
* length
	* specifies the size of the UDP header in bytes
* Checksum
	* used to verify the integrity of the UDP header

## Internet Layer Protocols

### Internet Protocol (IP)

IP provides packet delivery for protocols in higher layers of the model. It is a connectionless delivery system that makes a "best-effort" attempt to deliver the packets to the correct destination. IP does not guarantee delivery of the packets

The Internet Protocol is also responsible for the logical addressing and routing of TCP/IP and, therefore, is considered a layer 3 protocol of OSI model. The IP on the router is responsible for decrementing the TTL (time to live) of the packet to prevent it from running in a "network loop"

Once the TTL of the packet is decremented to 0, the router removes the packet from the network and sends a status message to the sender (using ICMP); this prevents loops. Windows os have a default TTL of 128, whereas most Linux OS have a TTL of 64 by default.

### IP Header

![[스크린샷 2025-04-22 00.51.10.png]]

* Version
	* IPv4, IPv6
* Header Length
* Type of Service
	* how the packet should be handled by the system
	* if the low delay option is specified here, it means that the system should deal with the packet right away.
* Total length
* Identification
	* Because networks can handle only packets of a specific maximum size (MTU, maximum transmission unit) the system may break the data being sent into multiple fragments
	* uniquely identifies the fragment
* IP Flags
	* MF
		* More Fragments flag
		* more fragment are to come
	* DF
		* Don't Fragment flag
* Fragment Offset
	* specifies the order in which the fragments are to be put back together when the packet is assembled.
* TTL (Time to live)
	* When the TTL reaches 0, the packet is discarded.
* Protocol
	* TCP, UDP
* Header Checksum
	* verifies the integrity of the IP header
* Source Address (IP)
* Destination Address (IP)
* IP options
	* any other settings in the IP header

### Internet Control Message Protocol (ICMP)

ICMP enables systems on a TCP/IP network to share status and error information. can troubleshoot(detect) network trouble using status information.

ICMP messages are encapsulated within IP datagrams so that they can be routed throughtout a network. Tow programs that use ICMP messages are ping and tracert.

You can use ping to send ICMp echo requests to and IP address and wait for ICMP echo responses.

Tracert traces the path taken a particular host. This utility can be useful in troubleshooting internetworks. Tracert sends ICMP echo requests to an IP address while it increments the TTL field in the IP header by a count of 1, after starting at 1, and then analyzing the ICMP errors that are returned. Each suceeding echo request should get one unit further into the network before the TTL field reaches 0 and an "ICMP time exeeded" error message is returned by the router ateempting to forward it

### ICMP Types and Codes

ICMP does not use port numbers, but instead uses ICMP types and codes to identify the different types of messages.

an echo request message that is used by the ping request used by the ping request uses ICMP type 8, while the ping reply comes back with an ICMP type 0 message.

Some of the ICMP types are broken down to finer levels with different codes in the type. For example, ICMP type 3 is "destination unreachable" message, but because there are many reasons why a destination is unreachable, the type is subdivided into different codes.

![[스크린샷 2025-04-22 01.19.53.png]]

### ICMP Header

![[스크린샷 2025-04-22 01.21.09.png]]

* Type
	* ICMP Type
* Code
	* ICMP code
* Checksum
	* verify the integrity of the ICMP header
* Other
	* stored any data within the ICMP header.

Internet Group Management Protocol (IGMP) is another Internet-layer protocol and is used for multicast applications.

### Address Resolution Protocol (ARP)

ARP provides IP address-to-physical address resolution on a TCP/IP network. To accomplish the feature, ARP sends out a broadcast message with an ARP request packet that contains the IP address of the system it is trying to find. All systems on the local network see the message, and the system that owns the IP address for which ARP is looking replies by sending its physical address to the originating system in an ARP reply packet. The physical/IP address combo is then stored in the ARP cache of the originating system for future use.

### Single-Segment ARP Example

everyone exists on a single LAN
![[스크린샷 2025-04-22 01.27.57.png]]

1. PC-A wants to send information directly to PC-B
2. PC-A knows PC-B's IP address (or DNS), it dosen't know PC-B's Ethernet MAC address
3. PC-A generates an ARP request message. (source IP: 10.1.1.1, dest IP: 255.255.255.255, broadcast IP, ARP Datagram: PC-B's IP address)
4. then placed on the Ethernet segment. Both PC-B and PC-C see this frame
5. Both device's network interface cards (NICs) notice the data link layer broadcast address and assume that this frame is for them, since the destination MAC address is broadcast, so they strip off the Ethernet frame and pass the IP datagram with the ARP request up to the Internet layer.
6. PC-B notices that this is an ARP request and that this is its own IP address in the query, and it therefore responds directly back to PC-A with PC-B's MAC address.
7. PC-C however, sees that this is not an ARP for its own MAC address and ignores the requested datagram broadcast

### Two-Segment ARP Example

![[스크린샷 2025-04-22 01.41.33.png]]

PC-A wants to connect to PC-B using IP.
The source IP address is 1.1.1.1, destination ip is 2.2.2.2
because two devices are on different networks, a router is used to communicate between the networks.

1. PC-A will determine whether the destination IP address is local network or a remote network
	* if destination IP address in remote network, PC-A need the MAC address of the default gateway router.
2. if the router's MAC address isn't already in its local ARP cache, PC-A will generate an ARP request for the default gateway's MAC address.
3. the router responds with the MAC address of its Ethernet interface connected to PC-A
4. PC-A creates an IP packet with the source and destination IP address (source: 1.1.1.1, dest: 2.2.2.2) -> encapsulates in Ethernet frame, source MAC: PC-A, dest MAC: router
5. PC-A sends the Ethernet frame to the router
6. router receives the Ethernet frame, the router compares the frame to the MAC address on its Ethernet interface, which it matches. The router strips off the Ethernet frame and makes a routing decision based on the destination address of 2.2.2.2
	* in this example, dest address 2.2.2.2 is directly connected to the router's second interface.
7. if router doesn't have PC-B's MAC address in its local ARP cache, the router ARPs for the MAC address of PC-B(2.2.2.2) and receives the response
8. The router then encapsulates the original IP packet in a new Ethernet frame
9. placing its second interface's MAC address, in the source MAC address field and PC-B's MAC address in the destination field.

next time PC-A needs to send someting to PC-B, the device will not have to ARP other intermediate devices again (just look through ARP cache)

---
## EXERCISE: Identifying TCP/IP Protocols

![[스크린샷 2025-04-22 01.56.08.png]]

A: ARP
B: IP
C: TCP
D: FTP
E: ICMP
F: SMTP
F: HTTPS
H: UDP

## Questions

1. Which layer of the OSI model does IP run at?
	* IP (Internet Layer) => (in OSI) network layer

2. Which of the following protocols are layer 4 protocols of the OSI model?
	* in OSI layer 4 protocol is transport layer (in TCP/IP it also transport layer)
	* uses TCP, UDP protocol

3. Which protocol is responsible for converting IP address to a MAC address?
	* ARP (in Internet layer, or network layer in OSI model)

4. Which protocol is responsible for connection-oriented communication?
	* connection-oriented = TCP, connectionless = UDP therefore TCP

5. Which protocol is responsible for error reporting and status information
	* ICMP

6. Which protocol is responsible for logical addressing and delivery of packets?
	* IP

7. What two items are used by TCP to guarantee delivery of data?
	* Sequence numbers, Acknowledgment numbers

8. What field in the IP header identifies the system that the packet came from?
	* Source IP address

9. What field in the TCP header is used to specify the target application that the packet is for?
	* Destination IP address (x)
	* TCP -> 포트 사용, Destination Port addr 사용해서 target application 지정함


![[스크린샷 2025-04-22 02.22.47.png]]

* Application Layer
	* SMTP, FTP, HTTP
* Transport Layer
	* TCP, UDP
* Internet Layer
	* ARP, ICMP, IP
* Network Access Layer
	* Ethernet

![[스크린샷 2025-04-22 02.25.11.png]]

**3 way handshake**

1. SYN
2. SYN/ACK
3. ACK