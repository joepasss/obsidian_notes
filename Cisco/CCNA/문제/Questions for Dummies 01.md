1. Your manager asks you which service is responsible for translating the source IP address of a  packet to the IP address of the public interface on the router.

NAT: Network Address Translation

2. You are troubleshooting a communication problem. You seem to be able to communicate with Glen’s website by IP address, but not by the fully qualified domain name (www.gleneclarke.com). What  is most likely the problem?

DNS

3. Which network service can be configured on your router that is responsible for assigning IP  addresses to systems on the network?

DHCP

4. What service on the network is responsible for converting the FQDN to an IP address?

DNS server

5. What service on the network is responsible for verifying username and passwords when the user attempts to log on?

Authentication server

6. You are monitoring network traffic and you notice a number of DHCP discover messages on the network. Which of the following is the destination address of the DHCP discover message?

DHCP uses broadcast address (FF-FF-FF-FF-FF-FF)

7. When a client system boots up and requests an IP address, it first must send out which message?

DHCP discover -> DHCP response -> DHCP request -> DHCP ACK

8. What device is responsible for regenerating the signal so that the signal can travel a greater distance?

Repeater (or hub. hub is set of repeaters)

9. Which device filters traffic by looking at the destination address of the frame and then forwards the frame to the port that the destination system resides on?

Switch

10. Which of the following is a layer-3 device?

Layer 1 devices: Bridge, Repeater, Hub
Layer 2 devices: Switch
Layer 3 devices: Layer 3 switch, Router

11. A device that can send and receive information, but not at the same time, is said to be ""

Half-duplex

12. A message that is sent out on the network and is destined for all systems is known as a "" message

broadcast message

* Unicast: send to single destination
* Multicast: send to group of devices

13. A message that is sent out on the network and is destined for a group of systems is known as a "" message.

Unicast

14. A group of systems that can receive one another’s broadcast messages is known as a ""

Broadcast domain

* Collision domain
	* Collision can occur
	* hub with five device = one collision domain

15. You are monitoring network traffic and notice that there is a large number of broadcast  messages sent across the wire. You would like to separate your network into multiple broadcast domains. How can you do this? (Select two.)

Using VLANs and router. just using switch or hub is create a single broadcast domain

16. A group of systems that can have their data collide with one another is known as a ""

Collision domain

17. How many broadcast domains and collision domains are there in the diagram below?

![[Pasted image 20251220164932.png]]

I assume that switch dosen't have any VLAN configuration or trunk port, so it has two broadcast domains (created by Router). and each port on a switch or bridge creates a separate collision domain.

2 broadcast domains and 5 collision domains

18. Sue is having trouble understanding some network concepts and asks you to help identify address types. Which of the following is considered a layer-2 address?

D 00-AB-0F-2B-3C-4E

layer 2 uses MAC address. layer 3 uses IP address.

19. You are troubleshooting communication to a network by looking at the link light on the switch. What layer of the OSI model are you troubleshooting when looking at a link light?

Data link layer (OSI layer 2)

20. What layer of the OSI model is responsible for breaking the data into smaller segments?

Transport layer

21. Which of the following is considered a layer-3 address?

192.168.2.200

22. What layer of the OSI model is responsible for routing and logical addressing?

Network (layer 3)

#### OSI Layers

| Layer | Desc         |
| ----- | ------------ |
| 7     | Application  |
| 6     | Presentation |
| 5     | Session      |
| 4     | Transport    |
| 3     | Network      |
| 2     | Data link    |
| 1     | Physical     |

Physical layer is mediums (cables, hubs, repeater ...)
Data link layer is contains switches (using MAC address)
Network layer is continas routers (using IP address)

23. Which of the following are considered layer-2 devices? (Choose two.)

Switches, bridges, NICs.

24. Which of the following are considered layer-1 devices? (Choose two.)

Cable, hubs, Repeaters

25. What is the application layer protocol for delivering e-mail across the Internet?

POP3: Used to read or download e-mail
SMTP: Used to send e-mail
IMAP: Used to read e-mail (newer version)

**TCP**

| Port | Service     | Desc                                                   |
| ---- | ----------- | ------------------------------------------------------ |
| 20   | FTP Data    | Used by FTP to send dat to a client                    |
| 21   | FTP Control | Used by FTP commands sent to the server                |
| 22   | SSH         |                                                        |
| 23   | Telent      |                                                        |
| 25   | SMTP        | Used to send e-mail                                    |
| 53   | DNS         | Used for DNS zone transfers                            |
| 80   | HTTP        |                                                        |
| 110  | POP3        | Used by POP3 to read e-mail                            |
| 143  | IMAP        | Used by IMAP, a newer Internet protocol to read e-mail |
| 443  | HTTPS       |                                                        |
| 3389 | RDP         | Remote Desktop Protocol                                |

**UDP**

| port     | service | desc                                                             |
| -------- | ------- | ---------------------------------------------------------------- |
| 53       | DNS     | Used for DNS queries                                             |
| 67, 68   | DHCP    | 67 is used by the DHCP service and 68 is used by client requests |
| 69       | TFTP    |                                                                  |
| 137, 138 | NetBIOS |                                                                  |
| 161      | SNMP    | Simple Netowork Management Protocol                              |

26. Which layer of the OSI model works with packets?

| PDU     | Layers                                       |
| ------- | -------------------------------------------- |
| Data    | Application, presentation, and session layer |
| Segment | Transport layer                              |
| Packet  | Network layer                                |
| Frame   | Data link layer                              |
| Bits    | Physical layer                               |

27. What is the application layer protocol for receiving e-mail over the Internet?

To send e-mail, using SMTP. To read e-mail, using POP3 and IMAP.

28. Which Gigabit Ethernet standard uses UTP cabling to reach 1000 Mbps?

| Ethernet type | Distance Limit | Cable Type                   |
| ------------- | -------------- | ---------------------------- |
| 1000BaseCX    | 25m            | STP copper                   |
| 1000BaseLX    | 3-10km         | SMF                          |
| 1000BaseSX    | 275m           | MMF                          |
| 1000BaseT     | 100m           | CAT 5E and CAT 6 UTP (RJ-45) |
| 1000BaseZX    | 100m           | SMF                          |

1000BaseCX and 1000BaseT

29. Which 10 Gigabit Ethernet standard uses multimode fiber-optic cabling?

| Ethernet type | Distance Limit | Cable Type           |
| ------------- | -------------- | -------------------- |
| 10GBaseSR     | up to 400m     | Short range MMF      |
| 10GBaseLR     | up to 10km     | long range SMF       |
| 10GBaseER     | up to 40km     | extra-long range SMF |
| 10GBaseT      | up to 100m     | CAT 6A UTP           |

10GBaseLR and 10GBaseER uses multimode fiber. 10GBaseT is uses UTP cable.

10GBaseSR uses multimode fiber.

30. Which of the following addresses does a router use to determine where a packet needs to be delivered? (Choose two.)

router uses layer 3 address (24.56.78.10, IP address).

31. Which layer of the OSI model works with frames?

Data link layer.

| PDU     | Layers                                       |
| ------- | -------------------------------------------- |
| Data    | Application, presentation, and session layer |
| Segment | Transport layer                              |
| Packet  | Network layer                                |
| Frame   | Data link layer                              |
| Bits    | Physical layer                               |

32. What type of cable would you use if you wanted to connect a system to an RJ-45 port on a switch?

Straight-through

33. You wish to network two systems by connecting a computer directly to another computer. Which type of cable would you use?

computer to computer (simmiler device) => Crossover

34. You need to create a crossover cable. What wires would you cross on one of the ends?

568B is used for both straight-through cables and one end of a crossover cable.

**568B**

| Pin | Color                      |
| --- | -------------------------- |
| 1   | White wire / orange stripe |
| 2   | Orange wire                |
| 3   | White wire / green stripe  |
| 4   | Blue wire                  |
| 5   | White wire / blue stripe   |
| 6   | Green wire                 |
| 7   | White wire / brown stripe  |
| 8   | Brown wire                 |

A straight-through Ethernet UTP cable has pin 1 on one end of the cable connected to pin 1 on the other end of the cable, pin 2 to pin 2, and so on.
A crossover UTP Etherent cable crosses over two sets of wires: pin 1 on one side connected to pin 3 on the other side, and pin 2 is connected to pin 6. Crossover cable is used to connect similar devices together.

1 - 3
2 - 6

1 and 2 with 3 and 6

35. See figure below. You are trying to ping computer B from computer A and are unsuccessful. What is the problem?

![[Pasted image 20251221202303.png]]

cable connect between switch and pc should be crossover.

the cable type between the computers and switches is incorrect.

36. You are testing communication to a router and have decided to connect your workstation to the Fast Ethernet port of the router. What type of cable would you use?

router to pc uses straight-through cable (simmilar devices)

37. You have a UTP cable that has been configured at both ends with the 568B standard. What type of cable is it?

both ends are 568b standard = straight through
single ends are 568b standard = crossover

38. You wish to create a crossover cable and have wired one end of the cable with the 568A standard, What standard should you use to wire the opposite end of the cable?

568B

![[Pasted image 20251221202717.png]]

568A and 568B standard is different between green and green/white cable.

cross over cable is pin 1 to pin 3, and pin 2 to pin 6.

39. Which of the following are considered class A addresses? (Select all that apply.)

Class A: 00000001 - 01111111 (1 - 127)
Class B: 10000000 - 10111111 (128 - 191)
Class C: 11000000 - 11011111 (192 - 223)

B. 10.35.87.5
E. 121.59.87.32

40. Sue is reviewing IP addressing basics and asks you which of the following is considered a class A private address.

Forbidden Addresses:

*loopback addr*: 127.0.0.1

*private addr*:
* 10.0.0.0 - 10.255.255.255
* 172.16.0.0 - 172.31.255.255
* 192.168.0.0 - 192.168.255.255

*APIPA (Automatic Private IP Addressing)*: 169.254.x.y

Class A private address: 10.0.0.0 - 10.255.255.255

D. 10.55.67.99

41. What is the default subnet mask of a system with the IP address of 189.34.5.67?

This address is class B address (range 128-191), so default subnet mask is 255.255.0.0 (/16)

B. 255.255.0.0

42. What is the default subnet mask of a class C address?

255.255.255.0 (/24)

43. Which of the following is a class B private address?

Class B Private address is 172.16.0.0 - 172.31.255.255

C. 172.16.45.10

B. 192.168.0.5 is class C private address
D. 10.55.67.99 is class A private address

44. Are the systems of 201.45.3.56 and 201.45.5.20 on the same network?

201.x.x.x is class C address (range 192 - 223), so default subnet mask is 255.255.255.0.
Because subnet mask is 255.255.255.0, network id is 201.45.3.0 and 201.45.5.0 so this two address is different network.

B. no

45. Which of the following addresses are considered invalid addresses to assign to a host on the network? (Select all that apply.)

A. 10.254.255.255: Class A private address
C. 190.34.255.255: Class B broadcast address
E. 202.45.6.0: Class C network ID

46. The decimal number of 137 is which of the following in binary?

A. 10001001: 137
B. 10101001: 153
C. 11001001: 193
D. 10000101: 133

47. Are the systems with the IP addresses of 121.56.78.10 and 121.45.6.88 on the same network?

121.x.x.x is in class A address (range 1 - 127), so default subnet mask is 255.0.0.0 because this two of addresses is in same network.

A. Yes

48. What is the default subnet mask of 130.56.78.10?

130.x.x.x is in class B address (range 128 - 191), so default subnet mask is 255.255.0.0.

B. 255.255.0.0

49. Your system has an IP address of 131.107.45.10. Which of the following systems is on the same network as you?

131.x.x.x is in class B address => default subnet mask 255.255.0.0 so network ID is 131.107.0.0

C. 131.107.22.15

50. Which of the following addresses are considered invalid addresses to assign to a host on the network? (Select all that apply.)

A. 216.83.11.255: Class C broadcast address
B. 12.34.0.0: Class A network ID
D. 131.107.0.0: Class B network ID
E. 127.15.34.10: reserved for loopback

C. 10.34.15.22
F. 189.56.78.10

51. The binary number of 01101101 is which of the following decimal values?

B. 109

A. 101: 01100101
C. 135: 10000111
D. 143: 10001111

52. Which class address always has the value of the first bits in the IP address set to 110?

Class A: 00000000 - 01111111
Class B: 10000000 - 10111111
Class C: 11000000 - 11011111

C. Class C

53. Which class address always has the first two bits of the IP address set to 10?

B. Class B

54. Which IP address class always has the first bit in the address set to 0?

A. Class A

55. Which protocol is responsible for converting the logical address to a physical address?

D. ARP

A. TCP: reliable trasnport
B. IP: routing, logical addressing
C. ICMP: Internet Control Messege Protocol

56. The ARP request is sent to which of the following layer-2 destination addresses?

broadcast address (FF-FF-FF-FF-FF-FF)

A. FF-FF-FF-FF-FF-FF

57. What are the three phases of the TCP three-way handshake?

B. SYN, SYN/ACK, ACK

![[Pasted image 20251125053603.png]]


1. **SYN**
	* The sending system sends a SYN message to the receiving system.
	* The SYN message contains the *initial sequence number (ISN)*, which is the first sequence number to be used.
2. **SYN/ACK**
	* This phase acknowledges the first message, but at the same time indicates its initial sequence number.
3. **ACK**
	* Acknowledges that the packet sent in the second phase has been received.

4. Which of the following does TCP use to guarantee delivery?

C. Sequence numbers and acknowlegements

59. What TCP/IP protocol is responsible for logical addressing and routing functions?

B. IP

60. Which transport layer protocol is responsible for unreliable delivery?

D. UDP

61. Which TCP/IP protocol is responsible for error and status reporting?

C. ICMP

62. The router looks at which field in the IP header to decide where to send the packet?

B. Destination IP address

63. What flags are set on the second phase of the three-way handshake?

C. ACK/SYN

64. Which flag is set in a TCP packet that indicates a previous packet was received?

C. ACK

65. You wish to allow echo request messages to pass through the firewall. What ICMP type is used in an echo request message?

| Type                 | Code | Desc                             |
| -------------------- | ---- | -------------------------------- |
| 0 - Echo Reply       | 0    | Echo reply message               |
| 3 - Dest Unreachable | 0    | Destination network              |
|                      | 1    | Destination host unreachable     |
|                      | 2    | Destination protocol unreachable |
|                      | 3    | Destination port unreachable     |
| 8 - Echo request     | 0    | Echo request message             |

B. 8

66. Which of the following are fields found in the IP header? (Select all that apply.)

![[Pasted image 20251125063405.png]]

Version, Header length, Type of service, Total length, Identification, IP flags, Fragment offset, TTL, Protocol, Header checksum, Source address, Destination address, IP options.

There is no sequence number (used by TCP), and SYN flag (used by TCP)

67. Which of the following are considered application layer protocols?

C. FTP

TCP, UDP = transport layer
ICMP, IP = network layer

68. Which transport layer protocol is responsible for reliable delivery?

D. TCP

69. What ICMP type is used by the echo reply message?

A. 0

3 is used "destination unreachable", and 8 used by echo request

70. The security administrator for your network has asked that you block ping messages from entering your network. What protocol would you block?

ping uses ICMP

A. ICMP

71. Which TCP flag is responsible for dropping a connection at any point in time?

D. RST

FIN used for "polite end" it requres addtional ACK

![[Pasted image 20251125053848.png]]

If Computer A wants to disconnect from a TCP session, it must send a FIN flag to Computer B to signal that it wants to end the converstaion.
When Computer B receives the FIN message, it replies with an acknowledgement and then sends its own FIN message back to Computer A.
As a final step to this process, Computer A must acknowledge that it received the FIN message from Computer B.

Also you can "hang up" by sending a TCP message with the RST (reset) flag set.

72. Which of the following is the IPv6 equivalent to 127.0.0.1?

| Address     | Value     | Desc                                                                                                                                                                                                                                        |
| ----------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Global      | 2000::/3  | Global unicast addresses are IPv6 public IP addresses that are routable on the Internet, and which are assigned by the Internet Assigned Numbers Authority (IANA). They are equivalent to IPv4 public IP addresses.                         |
| Reserved    | (range)   | Reserved addresses are used for specific types of anycast as well as for future use.                                                                                                                                                        |
| Private     | FE80::/10 | Like IPv4, IPv6 originally supported private addressing, which is used by devices that don't need to access a public network. The first two digits are FE, and the third digit can range from 8 to F.                                       |
| Loopback    | ::1       | Like the 127.0.0.1 address in IPv4, 0:0:0:0:0:0:0:1, or ::1, is used for local testing functions; unlike IPv4, which dedicates a complete Class A block of addresses for local testing, only one address is used for local testing in IPv6. |
| Unspecified | ::        | 0.0.0.0 in IPv4 means "unknown" address. In IPv6, this is represented by :: and is typically used in the source address field of the packet when an interface doesn't have an address and is trying to acquire one dynamically.             |

C. ::1

73. Your manager has been hearing a lot about IPv6 addressing, and asks you which of the ­following statements are true about IPv6 unicast addresses? (Select two.)

* **Site-local/unique-local**
	* A site-local address (aka unique-local address) is considered a private IP address, similar to the 10.0.0.0/8 private IP with IPv4. This address is used for local communication within a site.
	* A site-local address always starts with FEC:: through FFF::.
* **Link-local**
	* A link-local address is similar to an APIPA address in IPv4 and is used for communication on the network link.
	* A link-local address starts with FE80::/10 (FE8:: through FEB::).

A. A link-local address starts with FEC:: through FFF:: not FE00
**B. A global address starts with 2000**
C. The loopback address is ::1 not 127::1.
D. When an interface is assigned a global address, it is allowed only one IP address
**E. The loopback address is ::1**

74. Which of the following is an example of an IPv6 link-local address?

B. FE80:F407:622c:a0ce:90cc

75. Your manager has asked you what some of the benefits of transitioning from IPv4 to IPv6 are. (Select two.)

B. No broadcast messages
C. A 64-bit address scheme

76. our manager is evaluating IPv6 transitioning technologies and is wondering which IPv6 tunneling method encapsulates the IPv6 data into an IPv4 user datagram to travel over the Internet and can pass through NAT devices.

A. 6-to-4

77. You are evaluation how you are going to slowly transition to IPv6. What three techniques can be used to allow migrating from IPv4 to IPv6? (Choose three.)

B. Translation between IPv6 and IPv4
D. Use of dual-stack routing
E. Use tunneling protocols

78. What IPv6 address type is similar to an IPv4 private IP address and starts with FC00?

D. Unique local (not sure, i think unique local address start with FEC:: through FFF.)

79. SMTP uses which of the following ports?

**TCP Ports**

| Port | Service     | Desc                                                   |
| ---- | ----------- | ------------------------------------------------------ |
| 20   | FTP Data    | Used by FTP to send dat to a client                    |
| 21   | FTP Control | Used by FTP commands sent to the server                |
| 22   | SSH         |                                                        |
| 23   | Telent      |                                                        |
| 25   | SMTP        | Used to send e-mail                                    |
| 53   | DNS         | Used for DNS zone transfers                            |
| 80   | HTTP        |                                                        |
| 110  | POP3        | Used by POP3 to read e-mail                            |
| 143  | IMAP        | Used by IMAP, a newer Internet protocol to read e-mail |
| 443  | HTTPS       |                                                        |
| 3389 | RDP         | Remote Desktop Protocol                                |

**UDP ports**

| port     | service | desc                                                             |
| -------- | ------- | ---------------------------------------------------------------- |
| 53       | DNS     | Used for DNS queries                                             |
| 67, 68   | DHCP    | 67 is used by the DHCP service and 68 is used by client requests |
| 69       | TFTP    |                                                                  |
| 137, 138 | NetBIOS |                                                                  |
| 161      | SNMP    | Simple Network Management Protocol                               |

D. TCP 25

80. FTP uses which of the following ports?

FTP Data: TCP 20
FTP Control: TCP 21

C. 21

81. Which of the following represents the port used by POP3?

B. 110 (TCP)

82. Which of the following are true statements about TCP ports? (Select all that apply.)

A. FTP uses TCP ports 21 (control) and 20 (data)
D. HTTP uses TCP port 80
F. SMTP uses TCP port 25

83. Which of the following are true statements about ports? (Select all that apply.)

A. DHCP uses UDP 67/68
C. SNMP uses UDP 161
F. DNS uses UDP 53

84. Sue is unable to surf the Internet and calls you over to her desk to troubleshoot. You use ­ipconfig on the system and notice the following IP configuration. What is the problem?

```
Local Area Network Connection:
IPv4 Address... . ... . : 169.254.34.56
Subnet Mask... ... . . : 255.255.0.0
Default gateway... ...:
```

169.254.x.x => APIPA address (cannot get IP address)

D. The DHCP server is down

86. You are designing the IPv4 address scheme for the network. How do you calculate the broadcast address of a subnet?

A. set all host bits to 0

87. Your manager would like you to subnet the 129.65.0.0 network into six different networks. What is your new subnet mask?

129.x.x.x is class B network, so default subnet mask is 255.255.0.0. And this needs divided into at least 6 subnets so, 3 bits of host bits are masked (11111111.11111111.11100000.00000000, 255.255.192.0)

A. 255.255.192.0

88. What is the subnet mask of 135.44.33.22/20?

Subnet mask: 11111111.11111111.11110000.0000
           : 255.255.224.0

C. 255.255.224.0

89. You are designing the IP address scheme for your network and you need to subnet the 142.65.0.0 network into 12 different networks. What is your new subnet mask?

142.x.x.x is class B network, so default subnet mask is 255.255.0.0. And this needs divide into at least 12 subnets, so, 4 bits of host bits are masked. (11111111.11111111.11110000.00000000, 255.255.224.0)

C. 255.255.224.0

90. How many host bits are needed in order to support 1,010 systems on the network?

B. 10 (2^10 = 1024)

91. How many masked bits are needed in order to create at least 20 subnets?

C. 5

92. You are designing your IPv4 address scheme. How do you calculate the network ID of a subnet?

A. Set all host bits to 0.

93. How many hosts can exist on a network with the address of 180.45.10.20/20?

host bits is 12 so 2^12 - 2 = 4094

C. 4094

94. How many subnet bits do you need to create 6 networks?

A. 3

95. Which option on the Cisco router indicates that you can use the first and last subnets of a ­subnetted network?

B. ip subnet-zero

VLSM = variable length subnet mask
RIP = Routing Information Protocol

96. Which of the following subnet masks matched to the CIDR notation is correct?

255.255.255.224 = 11111111.11111111.11111111.11100000 = /27
255.255.192.0   = 11111111.11111111.11100000.00000000 = /19
255.255.240.0   = 11111111.11111111.11111000.11110000 = /28
255.255.255.192 = 11111111.11111111.11111111.11000000 = /26

A. 255.255.255.224

97. You are designing a network with subnets of variable sizes. In the figure below, identify two problems with the design.

![[Pasted image 20251223013128.png]]

202.x.x.x is class C network. So default subnet mask is 255.255.255.0 (/24).

```
2^host_bits - 2 = # of hosts supported
```

1. Subnet C
	* 2^5 - 2 = 30
	* Subnet mask: 11111111.11111111.11111111.11100000 (/27)
	* Range: 202.45.67.0 - 202.45.67.32
2. Subnet B
	* 2^6 - 2 = 62
	* Subnet mask: 11111111.11111111.11111111.11000000 (/26)
	* Range: 202.45.67.32 - 202.45.67.96
3. Subnet A
	* 2^7 - 2 = 126
	* Subnet mask: 11111111.11111111.11111111.10000000 (/25)
	* Range: 202.45.67.96 - 202.45.67.224

C. Subnet C does not have enough address
E. Computer B is assigned an incorrect address

98. Your system uses the IP address of 145.68.23.45/25. How many systems are supported on your network?

number of host bits are 7. so 2^7 - 2 = 126

A. 126

99. The Fast Ethernet port on a router is assigned the IP address of 131.107.16.1/20. How many hosts can exist on the subnet?

Subnet mask: 11111111.11111111.11110000.00000000

2^12 - 2 = 4094

B. 4094

100. You want to assign your serial interface the third valid IP address in the second subnet of 192.168.2.0/26. Which IP address would you use?

192.x.x.x is class C network. so default subnet mask is /24. so this network is subnetted into 4 subnets.

| Octet 1 | Octet 2 | Octet 3 | Octet 4 (binary) | Network ID    |
| ------- | ------- | ------- | ---------------- | ------------- |
| 192     | 168     | 2       | 00000000         | 192.168.2.0   |
| 192     | 168     | 2       | 01000000         | 192.168.2.64  |
| 192     | 168     | 2       | 10000000         | 192.168.2.128 |
| 192     | 168     | 2       | 11000000         | 192.168.2.192 |

| Network ID    | First Valid Address | Broadcast Address | Last Valid Address |
| ------------- | ------------------- | ----------------- | ------------------ |
| 192.168.2.0   | 192.168.2.1         | 192.168.2.63      | 192.168.2.62       |
| 192.168.2.64  | 192.168.2.65        | 192.168.2.127     | 192.168.2.126      |
| 192.168.2.128 | 192.168.2.129       | 192.168.2.191     | 192.168.2.190      |
| 192.168.2.192 | 192.168.2.193       | 192.168.2.255     | 192.168.2.254      |

D. 192.168.2.67

101. You are designing the IP address scheme for a branch office that needs to support 92 systems on the network. What should your subnet mask be?

```
2^host_bits - 2 = # of hosts supported
```

2^7 - 2 = 126

11111111.11111111.11111111.10000000 (255.255.255.128)

B. 255.255.255.128

102. What is the minimum number of host bits needed to support 510 hosts on the network?

2^9 - 2 = 510

C. 9

103. You wish to subnet 137.15.0.0 in order to support 8,190 hosts on each network. You wish to use the smallest subnet size to support this number of hosts. What will be the new subnet mask?

2^13 - 2 = 8190

11111111.11111111.1100000.00000000 = 255.255.192.0

B. 255.255.192.0

104. How many hosts can exist on a network with the address of 201.10.20.30/27?

Subnet mask = 11111111.11111111.11111111.11100000

2^5 - 2 = 30

B. 30

105. With the goal of preserving addresses, which of the following subnet masks would you use to create a subnet of up to 60 hosts?

255.255.255.224 = 11111111.11111111.11111111.11100000, #host: 30
255.255.255.192 = 11111111.11111111.11111111.11000000, #host: 62
255.255.255.252 = 11111111.11111111.11111111.11111100, #host: 2
255.255.255.248 = 11111111.11111111.11111111.11111000, #host: 6

B. 255.255.255.192

106. You have a system whose IP address is correctly configured for 198.45.6.87/27. You are now responsible for assigning an IP address to the router. Which of the following IP addresses should be assigned to the router?

198.x.x.x is class C network. so default subnet mask is /24. so 198.45.6.87/27 address is subnetted into 8 subnets. (mask 3 host bits)

| Octet 4  | Network ID   | First Valid  | Brd          | Last Valid   |
| -------- | ------------ | ------------ | ------------ | ------------ |
| 00000000 | 198.45.6.0   | 198.45.6.1   | 198.45.6.31  | 198.45.6.30  |
| 00100000 | 198.45.6.32  | 198.45.6.33  | 198.45.6.63  | 198.45.6.62  |
| 01000000 | 198.45.6.64  | 198.45.6.65  | 198.45.6.95  | 198.45.6.94  |
| 01100000 | 198.45.6.96  | 198.45.6.97  | 198.45.6.127 | 198.45.6.126 |
| 10000000 | 198.45.6.128 | 198.45.6.129 | 198.45.6.159 | 198.45.6.158 |
| 10100000 | 198.45.6.160 | 198.45.6.161 | 198.45.6.191 | 198.45.6.190 |
| 11000000 | 198.45.6.192 | 198.45.6.193 | 198.45.6.223 | 198.45.6.222 |
| 11100000 | 198.45.6.224 | 198.45.6.225 | 198.45.6.255 | 198.45.6.254 |

198.45.6.87 is third subnet (range from 198.45.6.64 - 198.45.6.95)

198.45.6.95 is broadcast address.
198.45.6.62 is second subnet
198.45.6.64 is Network ID

D. 198.45.6.94

107. What is the network ID for the third subnet of 220.55.66.0/27?

* Network ID
	* 220.55.66.0
	* 220.55.66.32
	* 220.55.66.64
	* 220.55.66.96
	* 220.55.66.128
	* 220.55.66.160
	* 220.55.66.192
	* 220.55.66.224

D. 220.55.66.64

108. The 24.60.32.20/11 address is located in which of the following subnets?

24.x.x.x is class A address, default subnet mask is 255.0.0.0 (/8) so this address is divided by 8 subnets.

24.60.32.20 is second subnet (24.32.0.0 - 24.63.255.255)

A. 24.32.0.0

109. You need to assign the fourth valid IP address of 107.53.100.10/12 to the Fast Ethernet port on the router. What address would you use?

107.x.x.x is class A, default subnet mask is /8 so this address is divided by 16 subnets. (increments are 16).

* Network ID
	* 107.0.0.0
	* 107.16.0.0
	* 107.32.0.0
	* 107.48.0.0
	* 107.64.0.0
	* 107.80.0.0
	* 107.96.0.0
	* 107.112.0.0
	* 107.128.0.0
	* 107.144.0.0
	* 107.160.0.0
	* 107.176.0.0
	* 107.192.0.0
	* 107.208.0.0
	* 107.224.0.0
	* 107.240.0.0

107.53.100.10 is in 107.48.0.0 network.

D. 107.48.0.4

110. A network administrator is working with the 195.34.56.0 network, which has been subnetted into 8 networks. Which of the following two addresses can be assigned to hosts on the same subnet? (Select two.)

195.x.x.x is class C network (default subnet is /24)

* Network ID
	* 195.34.56.0
	* 195.34.56.32
	* 195.34.56.64
	* 195.34.56.96
	* 195.34.56.128
	* 195.34.56.160
	* 195.34.56.192
	* 195.34.56.224

195.34.56.35 : in second subnet
195.34.56.14 : in first subnet
195.34.56.76 : in thrid subnet
195.34.56.58 : in second subnet
195.34.56.98 : in fourth subnet
195.34.56.129: in fifth subnet

A and D

111. Using the network diagram shown in the figure below and having the goal of making the best use of your IP address space, which of the following represents the address scheme you would apply to the Toronto office?

![[Pasted image 20251223030936.png]]

```
2^host_bits - 2 = # of hosts supported
```

2^5 - 2 = 30, so toronto office needs at least 5 host bits. (/27)

C. 199.11.33.128/27

112. What is the network ID of 190.53.159.150/22?

Subnet mask: 11111111.11111111.11111100.00000000
Default subnet mask is /16. so this address is divided by 64 subnets (2^6). increments are 4

159 is 4 x 39 + 3, so network id is 4 x 39 which is 156

D. 190.53.156.0

113. Which of the following statements are true of 205.56.34.53/28? (Choose three.)

205.x.x.x is class C network. Default subnet mask is /24. so this network is subnetted by 2^4 = 16. increments are 16.

* Network ID
	* 205.56.34.0
	* 205.56.34.16
	* 205.56.34.32
	* 205.56.34.48
	* 205.56.34.64
	* 205.56.34.80
	* 205.56.34.96
	* 205.56.34.112
	* 205.56.34.128
	* 205.56.34.144
	* 205.56.34.160
	* 205.56.34.176
	* 205.56.34.192
	* 205.56.34.208
	* 205.56.34.224
	* 205.56.34.240

205.56.34.53 is fourth subnet (range: 205.56.34.48 - 205.56.34.63)

The network is subnetted.
The first valid address of the subnet is 205.56.34.49
The last valid address of the subnet is 205.56.34.62

C. The network ID of the subnet is 205.56.34.48
F. The network is subnetted
H. The broadcast address of the subnet is 205.56.34.63

114. What is the broadcast address of 107.50.100.10/12?

107.x.x.x is class A address (default subnet mask is /8). so this address is subnetted by 16 subnets (2^4), increments are 16.

50 is 16 x 3 + 2, so network id is 107.48.0.0 brd is 107.63.255.255

A. 107.63.255.255

115. You need to assign the last valid IP address of 199.45.67.139/26 to the Serial port on your router. What address would you use?

199.x.x.x is class C address, default subnet mask is /24 so this address is subnetted by 4 subnets. Increments are 64.

199.45.67.139 address is in 199.45.67.128 subnet (range: 199.45.67.128 - 199.45.67.191)

199.45.67.128 is Network ID
199.45.67.126 is in different subnet
199.45.67.191 is Broadcast address

D. 199.45.67.190

116. What is the last valid address for the subnet of 220.55.66.64/27?

220.x.x.x is class C address, so default subnet mask is /24. Because that this network is subnetted by 8 subnets. Increments are 32.

220.55.66.64 is in 220.55.66.64 network (range: 220.55.66.64 - 95). in this subnet, last valid address is 220.55.66.94.

B. 220.55.66.94

117. Which of the following addresses exists on the same network, knowing that all systems have a subnet mask of 255.255.255.240? (Select two.)

class c default subnet mask: /24
255.255.255.240: /28 -> Subnetted by 2^4 (16), increments are 16.

195.34.56.14: 195.34.56.0
195.34.56.30: 195.34.56.16
195.34.56.38: 195.34.56.32
195.34.56.55: 195.34.56.48
195.34.56.17: 195.34.56.16
195.34.56.69: 195.34.56.64

B and E

118. Which of the following statements are true of 190.88.122.45/19? (Choose three.)

190.x.x.x is class B address, class B has /16 as default subnet mask. so this network is subnetted by 8 subnets. (increments are 32)

190.88.122.45 is in 190.88.96.0 subnet. In this subnet first valid address is 190.88.96.1, broadcast address is 190.88.127.255, and last valid address is 190.88.127.254

D. The first valid addreess of the subnet is 190.88.96.1
E. The last valid address of the subnet is 190.88.96.254
F. The network is subnetted

119. Which of the following subnet masks would you typically use on a WAN interface to make the best use of address space?

A WAN link only needs two IP addresses available

D. 255.255.255.252

120. Which IP feature allows you to use different subnet masks on different subnets within the network?

C. VLSM

121. You are connecting two of your office locations together over a WAN link. Each office has a router with an IP address assigned. What should your subnet mask be in order to leverage the best use of address space?

WAN link needs 2 subnet (255.255.255.11111100. 255.255.255.252)

D. 255.255.255.252

122. Your organization network diagram is shown in the figure below. Your company has the class C address range of 199.11.33.0. You need to subnet the address into three subnets and make the best use of the available address space. Which of the following represents the address scheme you would apply to the New York office?

![[Pasted image 20251225040705.png]]

```
HOST BIT calculate
2^host_bits - 2 = number of hosts per subnet
```

**WAN LINK**

Host#: 2

Subnet Mask    : 11111111.11111111.11111111.11111100
Network ID     : 199.11.33.0
Broadcast Addr : 199.11.33.3

**Toronto Office**

Host#: 27

Subnet Mask    : 11111111.11111111.11111111.11100000
Network ID     : 199.11.33.4
Broadcast Addr : 199.11.33.35

**New York Office

Host#: 125

Subnet Mask    : 11111111.11111111.11111111.10000000
Network ID     : 199.11.33.36
Broadcast Addr : 199.11.33.163

B.199.11.33.0/25

123. Using the network diagram shown in the figure below and having the goal of making the best use of your IP address space, which of the following represents the address scheme you would apply to the WAN link?

![[Pasted image 20251225041626.png]]


E. 199.11.33.160/30

124. Which of the following represents a purpose for the serial port on a Cisco router?

C. To connect to the WAN

The serial port is typically used to connect to the WAN environment with an external CSU/DSU. You could also use the serial port to connect directly to the serial port of another router for a back-to-back serial link.

125. Which of the following port identifiers are used to reference a Fast Ethernet port? (Select two.)

B. f0/0
E. fa0/0

126. What type of cable is used to connect to the console port?

D. Rollover

Crossover = connect between simmilar devices (switch-switch, pc-pc, pc-router ...)
Straight-through = connect between dissimilar devices (switch-router, switch-pc ...)

127. Your manager notices an AUX port on the back of the Cisco router and asks what it is used for. How would you reply?

D. To connect a modem

128. You are performing a security audit on the network devices and have verified that the console port has a password configured. What other port should you verify has a password?

C. AUX

129. You are configuring the DCE end of a point-to-point serial link between two routers. What extra command is typed on the DCE end of the link?

A. `clock rate 64000`

130. The external CSU/DSU connects to which port on the router?

D. Serial0/0/0

131. What type of module can be purchased to install an internal CSU/DSU into the router?

C. WIC (WAN Interface Card)

132. You are trying to assign an IP address to the LAN port on the router and get the following message on the screen. 

```
Router(config)#interface e0
^
% Invalid input detected at '^' marker.
```

You use the show version command to try to identify the problem. The output is displayed below. What is the problem?

```
Cisco 2811 (MPC860) processor (revision 0x200) with 60416K/5120K bytes 
of memory
Processor board ID JAD05190MTZ (4292891495)
M860 processor: part number 0, mask 49
2 FastEthernet/IEEE 802.3 interface(s)
4 Low-speed serial(sync/async) network interface(s)
239K bytes of NVRAM
62720K bytes of processor board System flash (Read/Write)
Configuration register is 0x2102
Router>
```

B. The interface type specified does not exist.

133. Where is the running configuration stored?

B. VRAM

134. Which of the following are stored in ROM on the Cisco device?

B. POST
C. Bootstrap
E. ROMMON

Startup config is stored in NVRAM
IOS is stored in flash
Running config is stored in VRAM (Volatile ram, not video ram)

135. What type of memory is shown here?

D. Flash

136. Which commands can you use to save the running configuration to the startup configuration? (Select two.)

D. `copy running-config startup-config`
E. `write`

137. When the Cisco router starts up, where does it find the Cisco IOS?

D. Flash

138. When you first boot up the Cisco router, where does it locate the startup configuration?

A. NVRAM

139. You are connected to your switch and wish to remotely administer the Cisco router. Which command would you use?

A. telnet

140. You are at the global configuration prompt and wish to configure a communication protocol for remote access to the router. What command would you type?

A. `line vty 0 4`

`line ssh` is does not exist.
`line telnet` is does not exist.

141. What command would you use to navigate from priv exec mode to user exec mode?

D. `exit`

`shutdown` command is used for disable interface
`enable` command is used for navigate from user exec mode to priv exec mode

142. You wish to get help on which parameters exist on the ping command. How would you get a list of parameters?

B. `ping ?`

143. What is the command to move to priv exec mode from user exec?

A. `router>enable`

144. Which of the following prompts represents global configuration mode?

B. `router(config)#`

145. What is the default router name if a name is not assigned to the router during initial configuration?

C. `router`

146. What command would you use to navigate to global configuration mode?

D. `#config terminal`

147. What types of actions can an administrator perform at this IOS prompt? (Select two.)

```
R1(config-if)#
```

B. Disable the interface
E. Assign an IP address

`show running-config` is can executed from priv exec mode
Show ios version (`show version`) is priv exec mode command
Change the hostname is can done via `hostname` command which is global configuration command.

148. You wish to run the initial configuration dialog. What command would you use?

C. `setup`

149. Under what conditions would the initial configuration dialog appear?

B. When there is no startup-config
D. When you type setup

150. You have connected a computer to the console port of a brand new router. What will the router display on the screen after it is powered on?

C. The option to enter initial configuration

151. What is the purpose of the POST?

C. Verify that hardware is functioning

152. You have rebooted your Cisco switch and it displays an amber light on the system LED during POST. What does this indicate?

A. A failure during POST

153. What happens when a router starts up and it does not have a startup configuration file?

B. The router prompts to enter setup mode

154. Which of the following best describes ROMMON?

A. A limited operating system used to troubleshoot startup and password recovery

Storage area to hold the startup configuration = NVRAM
Used to store the Cisco IOS = flash
Storage area to hold the running configuration = VRAM

155. Which of the following best identifies the boot order of a Cisco device?

POST -> bootstrap program loaded and execute -> configuration register check -> IOS image load -> find a configuration file

D. POST; bootstrap loads IOS from flash; apply startup configuration to running config

156. What keystroke is used to access the ROMMON prompt in order to bypass the password?

D. Ctrl-Break

157. Using this output of the show version command, how many Fast Ethernet ports exist on the router?

```
Cisco 2811 (MPC860) processor  
(revision 0x200) with 60416K/5120K bytes of memory
Processor board ID JAD05190MTZ (4292891495)
M860 processor: part number 0, mask 49
2 FastEthernet/IEEE 802.3 interface(s)
4 Low-speed serial(sync/async) network interface(s)
239K bytes of NVRAM
62720K bytes of processor board System flash (Read/Write)
Configuration register is 0x2102
Router>
```

2 fast ethernet port and 4 low-speed serial.

B. 2

158. Using the output below, what version of the Cisco IOS is running?

```
Router>show version
Cisco IOS Software, 2800 Software (C2800NM-ADVIPSERVICESK9-M), Version 
12.4(15)T1, RELEASE SOFTWARE (fc2)
Technical Support:http://www.cisco.com/techsupport
Copyright (c) 1986-2007 by Cisco Systems, Inc.
Compiled Wed 18-Jul-07 06:21 by pt_rel_team
```

B. 12.4

159. Your manager is wondering how many Fast Ethernet ports are on the router. What command would you use to find that out?

D. `show version`


160. You wish to verify the version of the Cisco IOS you are running. What command would you use?

C. `show version`

161. You wish to view the current value of the configuration register. What command would you use?

B. `show verison`


162. You wish to disable the Fast Ethernet interface on your router. What command would you use?

A. `router(config-if)#shutdown`

163. Which of the following are encapsulation protocols that can be used on a serial link for a Cisco router? (Select all that apply.)

B. PPP
D. HDLC

PPTP = Point-to-Point Tunneling Protocol, method for implementing VPN.
RIP = Routing Information protocol, used for sharing routing information between rotuers.

164. You would like to configure the Fast Ethernet port on your router for full duplex. What commands would you use?

A.
```
R1#config term
R1(config)#interface f0/0
R1(config-if)#duplex full
```

165. You wish to enable the Fast Ethernet port on the router. What command would you use?

C. `R1(config-if)#no shutdown`

166. Which of the following layer-2 protocols are supported encapsulation protocols on the serial link of a Cisco device? (Select two.)


A. PPP
D. HDLC

167. Which of the following commands would you use to assign an IP address to the Fast Ethernet interface on the router?

C. `router(config-if)#ip address 10.0.0.1 255.0.0.0`


168. When looking at the interface fastethernet 0/1 command, what is the port number for the interface?

interface_name slot/port

B. 1

169. In a point-to-point serial link between two routers, which router sets the clock rate?

D.DCE

DCE (data communication equipment), DTE (data terminal equipment) DCE is responsible for set clock rate.

170. Which of the following are additional commands needed to configure a serial interface over what is needed when configuring an Ethernet interface? You are configuring the DCE device on a serial link. (Select all that apply.)

DCE is responsible for set clock rate

A. `clock rate 64000`
C. `encapsulation hdlc`

171. After disabling the serial interface using the shutdown command, which of the following represents the output displayed on the console as a result of your action?

C. `serial0/0 is administratively down, line protocol is down`

Because the serial port has been shut down by the administrator, the first part of the status will display as `serial0/0 is administrativly down`. The second part, which is associated with the layer 2 protocol on the link, will say the line protocol down because if the link is not up, the protocol cannot be up.

172. What command do you use on the interface to assign an IP address to the interface automatically via DHCP?


D. `R1(config-if)#ip address dhcp`

173. Which of the following commands would you use to change the hostname on the router?


D. `router(config)# hostname R1`

174. Which of the following password types are encrypted in the configuration by default?

C. `enable secret`

175. You are at the following prompt. What command would you type first to change the name of the router?

```
Router(config-if)#
```

B. exit

```
Router(config-if)#exit
Router(config)#hostname MY_ROUTER
```

177. You are looking to create a username for Tom to log on to the router. What command would you use?

A. `r1(config)#username tom password P@assw0rd`

178. You wish to view a list of commands recently used on the router, what ­command would you use?

A. `show history`

179. You would like to encrypt all passwords configured on the router. What command would you use?

B. `service password-encryption`

180. You have made changes to your Cisco router and would like to verify the changes before saving them. What command would you use?

C. `show running-config`

181. You are administering a Cisco router. When you have a typo in the command name, you have to wait for the router to do a DNS lookup before getting control of the console again — an annoying problem. What command would fix it?

B. `no ip domain lookup`


182. You would like the console to time out after a minute and 45 seconds of inactivity. What command would you use?

A. `exec-timeout 1 45`

183. What command can be used to modify the history size on the Cisco router?

A. `R1#terminal history size 30`

184. After creating a user named Bob, you would like to ensure that the console port prompts for username and password. What command would you use?

C. `r1(config-line)#login local`

185. Which of the following banner types is the first to be displayed?

B. MOTD

MOTD -> login banner -> user login -> exec banner

186. You would like to delete the configuration on your router and start with a clean configuration. Which two commands would you use? (Select two.)

B. `reload`
D. `erase starup-config`

after erase startup config, you should reload (restart device)

187. A network technician is having trouble ­connecting two routers across the serial ports. One router is a Cisco router, whereas the other router is from a different ­manufacturer. You have loaded HDLC on both routers. What should you do?

C. Use PPP as the serial link protocol

188. You would like to verify the status of one of your interfaces. What command would you use?

A. `show interfaces`

189. You are having trouble communicating with networks that are connected to your FastEthernet0/0 port. You use the ­command shown below to view the status of the links. What can you do to solve the problem?

```
R1>show ip interface brief
Interface          IP-Address   OK?  Method  Status
FastEthernet0/0    23.0.0.1     YES  manual  administratively down
FastEthernet0/1    unassigned   YES  manual  up
Serial0/2/0        24.0.0.1     YES  manual  up
Serial0/3/0        unassigned   YES  manual  administratively down
```

D. Enable the interface

190. Which of the following commands is used to display a table (output shown below) indicating the IP assigned to each interface and the status of the interface?

```
Interface          IP-Address   OK?  Method  Status
FastEthernet0/0    23.0.0.1     YES  manual  administratively down
FastEthernet0/1    unassigned   YES  manual  up
Serial0/2/0        24.0.0.1     YES  manual  up
Serial0/3/0        unassigned   YES  manual  administratively down
```

C. `show ip interface brief`

191. Which of the following commands display information on the IOS version? (Select three.)

A. `show version`
D. `show flash`
E. `show running-config`

192. You are troubleshooting communication issues between router R1 and R2. The physical cabling is connected as shown in the figure below, as well as the IP address range. You use the show command displayed in the figure to identify the problem. Why are the routers not communicating?

![[Pasted image 20251226042308.png]]

R1 has DTE cable in picture, but in terminal it shows it configured in DCE.

C. The serial port has the wrong cable connected.

193. You need to find out if the serial port on your router is acting as the DCE device in the  point-to-point link with another router. What command do you need to use?

D. `show controllers serial0/0`

194. You are troubleshooting communication across a serial port and use the show interfaces command to get the output displayed below. What is the problem?

```
Serial0/2/0 is down, line protocol is down
```

C. Check the physical connection

195. Sue, a network administrator in your office, has configured the router with the commands shown below. What is the order of the passwords that would need to be supplied if she connects to the console port on the router to make changes?

![[Pasted image 20251226042630.png]]

A. P@ssw0rd

196. You use the show interfaces command and get the status below on your serial port. Which layer of the OSI model has a problem?

```
Serial0/2/0 is up, line protocol is down
```

B. layer-2

197. Jeff configures the router with the commands shown below. What is the order of the passwords that he would need to use when connecting to the console port of the router to make changes?

![[Pasted image 20251226042809.png]]

D. conpass/P@ssw0rd

Tom would first need to type the console password and then, when entering priv exec mode, he would need to input the secret password of P@ssw0rd

198. Using the output below from the show interfaces command, what is most likely the problem?

```
Serial0/2/0 is up, line protocol is down
```

B. A protocol or clock rate has not been set.

199. Tom configures the router with the ­commands shown below. What is the order of the passwords that he would need to enter when connecting to the console port of the router to make changes?

![[Pasted image 20251226043010.png]]

D. compass/P@ssw0rd

200. Which of the following connectionless application layer protocols is used to transfer Cisco IOS images and configuration files to a backup location?

A. TFTP

201. Your coworker would like to back up the router configuration to a TFTP server. What command would you use to back up your running configuration to a TFTP server?

B. `copy running-config tftp`

202. A few weeks ago, you backed up your router configuration to a TFTP and now you wish to restore the configuration back to your router. What command would you use?

D. `copy tftp running-config`

203. Susan types the preceding command on her company router. What is the command's purpose?

?????????? what preceding command

204. Which of the following command associations are true? (Select two.)

C. Replace the IOS: `copy tftp flash`
E. backup the IOS: `copy flash tftp`

205. In the figure below, what is the purpose of address 192.168.1.3?

![[Pasted image 20251226044910.png]]

tftp server ip address

C. It is the address of the TFTP server to copy the running configuration to.

206. Which of the following commands allows you to view the filename of your IOS?

B. `show flash`


207. What happens if you are copying a new IOS image to flash memory and there is not enough available space due to space used by your existing IOS file?

A. The old IOS file is deleted to make room

208. You are looking to create a backup copy of your Cisco IOS to a TFTP server. What command would you use?

D. `copy flash tftp`

209. What port does TFTP use?

B. UDP 69

**Popular TCP ports**

| Port | Service     | Desc                                                   |
| ---- | ----------- | ------------------------------------------------------ |
| 20   | FTP Data    | Used by FTP to send dat to a client                    |
| 21   | FTP Control | Used by FTP commands sent to the server                |
| 22   | SSH         |                                                        |
| 23   | Telent      |                                                        |
| 25   | SMTP        | Used to send e-mail                                    |
| 53   | DNS         | Used for DNS zone transfers                            |
| 80   | HTTP        |                                                        |
| 110  | POP3        | Used by POP3 to read e-mail                            |
| 143  | IMAP        | Used by IMAP, a newer Internet protocol to read e-mail |
| 443  | HTTPS       |                                                        |
| 3389 | RDP         | Remote Desktop Protocol                                |

**Popular UDP ports**

| port     | service | desc                                                             |
| -------- | ------- | ---------------------------------------------------------------- |
| 53       | DNS     | Used for DNS queries                                             |
| 67, 68   | DHCP    | 67 is used by the DHCP service and 68 is used by client requests |
| 69       | TFTP    |                                                                  |
| 137, 138 | NetBIOS |                                                                  |
| 161      | SNMP    |                                                                  |

210. You have three IOS image files in flash memory. What command would you use to control which IOS is used when the router starts up?

C. `boot system`

211. You are having issues with your router, which currently does not have network access or an IOS. What protocol could you use to transfer an IOS image to the router?

A. xmodem

tftp, ftp, snmp (simple network mangement protocol) needs network connection

212. What command would you use to ensure that the Cisco router loads the IOS image file of c2800nm-advipservicesk9-mz.124-15.T1.bin from flash memory?

C. `boot system flash ~`

213. You need to identify other Cisco devices on the network; which Cisco protocol do you use?

C. Cisco Discovery Protocol 


214. You are connected to the console port of router R3 shown in the figure below. When you use CDP to display other network devices that have been discovered, what devices will you see?

![[Pasted image 20251227082632.png]]

CDP only discovers Cisco devices, and you will only see devices that are directly connected. (neighbors)

D. R1, R2, SW3

215. Which of the following statements are true in regards to CDP? (Select three.)

A. CDP is a Cisco proprietary protocol
D. CDP runs at the data link layer (layer 2)
F. CDP discovers directly connected devices.

216. You are the network administrator for the network shown in the figure below. Your manager would like to ensure that CDP advertisements are not sent out to the Internet from router HO1, but they should be sent to the Toronto and Boston routers. What should you do?

![[Pasted image 20251227090217.png]]

D. Disable CDP on the interface connected to the Internet.


217. You are the network administrator for a small network. You are connected to the console port on a switch and would like to telnet into the router on the network, but you forget the IP address of the router. What command could you use to determine the IP address of the router?

B. `show cdp neighbors detail`

218. There is a switch on the network called NY-SW1. What command would you use on the router to determine the model number of that switch?

A. `show cdp entry NV-SW1`

219. The frequency at which CDP messages are sent out is known as the `_________`.


D. CDP timer

220. By default, how frequently does a Cisco device send out CDP advertisements?

B. 60 seconds

default value for holdtime is 180seconds.

221. You wish to view only the IP addresses of neighboring devices; what command do you use?

A. `show cdp entry * protocol`


`show cdp neighbors detail` command shows addtional information.

222. By default, when a Cisco device receives a CDP message, how long does it store the CDP information locally?

default time for holddown value is 180 seconds

D. 180 seconds

223. You would like to alter the amount of time that CDP information is stored on a Cisco device. What command should you use?

C. `cdp holdtime`

224. What command do you use to disable CDP on the serial interface?

D. `no cdp enable`

225. You would like to alter the frequency at which CDP sends out advertisements to every 90 seconds. What command would you use?

D. `R1(config)#cdp timer 90`

226. Your manager is concerned that hackers can discover information about the network via CDP and asks you to disable CDP on your router. What command would you use?

B. `no cdp run`

`no cdp enable` command used interface-by-interface basis

227. You have telnetted into a switch on the network from your router and now wish to suspend your telnet session. How do you do this?

C `Ctrl-Shift-6, then X`

228. Which of the following IOS commands is used to connect to VTY ports on a Cisco device?

A. `telnet`

229. What port does Telnet use?

A. TCP 23

TCP 21 = FTP control
TCP 22 = SSH
TCP 25 = SNMP

230. Which of the following secure protocols should be used instead of Telnet for remote administration of the Cisco device?

D. SSH

231. What port does SSH use?

F. TCP 22

232. Which of the following represent characteristics of telnet with Cisco devices? (Select two.)

B. Traffic is unecrypted
E. Requires configuration on the estination device

By default, you cannot telnet into a device until you configure an IP address and passwords on the telnet ports (VTY ports).

233. Your network has been up and running for a few months. Two days ago, Bob, the network administrator, assigned an IP address to the switch. Why would an IP address be assigned to the switch?

A. For remote management

234. You have suspended a Telnet session and wish to reconnect to that session again; what command do you use?

B. `resume <Device ID>`

235. What command do you use to determine if you have any suspended Telnet sessions?

A. `show sessions`

236. Your manager would like you to monitor who is remotely connected to the router via Telnet. What command would you use?

C. `show users`

237. You are having trouble telnetting into one of your Boston switches from the New York office. You have no problem telnetting into the switch when you are in the Boston office. What is most likely the solution?

C. Configure a default gateway on the switch.

238. You try to ping the Boston router and get the error shown in the figure below. What should you do to allow the ping to work?

```
NY-R1>ping BOS-R1
Translating "BOS-R1"...domain server (255.255.255.255)
% unrecognized host or address or protocol not running.
```

B. Add the entry to the hostname table

239. What command displays the hostname table on a router?

A. `show hosts`

240. To configure your router to query a DNS server, what command do you use?

A. `ip name-server 23.0.0.10`

241. You have decided to remove the entry for the Boston router from your hostname table. What command would you use?

A. `NY-R1(config)#no ip host BOS-R1`

242. What command would you use to resolve the BOS-R1 router to the IP address of 15.10.0.5?

D. `ip host BOS-R1 15.10.0.5`


243. Which command has created the output shown in the figure below?

![[Pasted image 20251227093658.png]]

B. `show hosts`

244. When administering your Cisco router, it tries to do a DNS query every time you have a typo in a command. How can you disable this?

B. `no ip domain lookup`

245. You have configured your router for a name server, but you still find that names are not being resolved. What command would you execute?

A. `ip domain-lookup`


246. Which of the following DHCP commands would you use to specify that the address pool is to configure the default gateway on client computers?

B. `NY-R1(dhcp-config)#default-router 23.0.0.1`

247. Clients are no longer receiving IP addresses from the DHCP server running on your Cisco router. You suspect the DHCP service may have been shut down. How can you start it?

D. `service dhcp`

248. You have configured the DHCP service on your router as a backup DHCP service. You wish to stop the DHCP service so that addresses are not being handed out till you need the service. What command would you use to stop the service?

B. `no service dhcp`

249. You want to create configure DHCP on your router to give out IP addresses for the second subnet of 192.168.3.0/27. The addresses should be leased to clients for 7 days. What commands would you use?

```
NY-R1(config)#ip dhcp pool NY_Network
NY-R1(dhcp-config)#network 192.168.3.32 255.255.255.224
NY-R1(dhcp-config)#lease 7 0 0
```

250. You are monitoring DHCP usage on your router. What command would you use to see how many DHCP related messages the router has received?

B. `show ip dhcp server statistics`

251. You are administering the DHCP service on your Cisco router and would like to look at the DHCP leases, What command would you use?

C. `show ip dhcp binding`

252. To view a list of IP addresses given to clients on the network by your Cisco router DHCP service, what command do you use?

B. `show ip dhcp binding`


253. Which of the following implements NAT overload?

C. PAT

254. Your manager has asked what would be a good use of static NAT.

D. For publishing an internal system to the Internet

255. You wish to create an access list to be used for NAT as the inside addresses that will be permitted to use NAT. What command would you use to create the access list?

A. `NY-R1(config)#Access-list 16 permit 10.0.0.0 0.255.255.255`

256. Your serial port is connected to the WAN environment, with your Fast Ethernet port connected to the LAN. When configuring NAT, how would you configure the serial port?

D. `NY-R1(config-if)#ip nat outside`


257. What mechanism does NAT overloading use to allow one public IP address to be mapped to multiple systems?

D. PAT

258. You have created access list 1 that lists the IP addresses for NAT usage. What command would you use to configure the router so that those addresses can use NAT?

C. `NY-R1(config)#ip nat inside source list 1 interface serial 0/0 overload`

overload parameter = PAT

259. What type of NAT maps a single public address to all internal addresses?

A. Overloading (PAT)

260. Your serial port is connected to the WAN environment, with your Fast Ethernet port connected to the LAN. When configuring NAT, how would you configure the Fast Ethernet port?

Serial port -> outside, FE -> inside

D. `NY-R1(config-if)#ip nat inside`

261. What command can you use to view the NAT translation table?

view translation table: `show ip nat translations`
view nat statistics   : `show ip nat statistics`

D. `show ip nat translations`

262. Looking at the figure below, which of the following would be used to configure NAT on the router? Assume the IP addresses are already assigned to the interfaces.

![[Pasted image 20251227115510.png]]

S0/0 -> outside
F0/1 -> inside

```
NY-R1(config)#access-list 1 permit 192.168.4.64 0.0.0.31
NY-R1(config)#ip nat inside source list 1 interface serial 0/0 overload
NY-R1(config)#interface Serial0/0
NY-R1(config-if)#ip nat outside
NY-R1(config-if)#interface FastEthernet0/1
NY-R1(config-if)#ip nat inside
```

**Configure What inside address will translated**
```
Router(config)# ip nat inside source
					list STANDARD_IP_ACL_NAME_OR_#
					pool NAT_POOL_NAME
```

**Create NAT pool**
```
Router(config)# ip nat pool NAT_POOL_NAME
					beginning_inside_global_ip_addr
					ending_inside_global_ip_addr
					netmask subnet_mask_of_address
```

**Assign interface**
```
Router(config-if)#ip nat {inside | outside}
```

263. What keystroke interrupts the boot sequence on a Cisco router in order to implement password-recovery procedures?

B. `Ctrl-Break`


264. What is the default configuration register on most Cisco routers?

C. 0x2102

265. You wish to view the current configuration register value on a Cisco router, what command would you use? 

C. `show version`

267. When you need to recover a password, what bit number do you manipulate in the configuration register?

| Bit Number | Explanation                                          |
| ---------- | ---------------------------------------------------- |
| 0-3        | Known as the boot fields discussed in next section   |
| 6          | Ignore the contents of NVRAM                         |
| 7          | Disable the display of boot messages                 |
| 8          | Break is disabled                                    |
| 10         | IP broadcast with all zeros                          |
| 13         | Loads the default ROM software if network boot fails |

**Boot fields**

* **0**
	* If the boot field equals 0 (no bits enabled), the Cisco device will boot the ROMMON mode.
	* This boot mode is typically used when troubleshooting startup issues or needing to download or install a new IOS image file.
	* When booted to ROMMON mode, the configuraiton register value would be 2100.
* **1**
	* If the boot field has a decimal value of 1, the device boots to a Mini IOS that is located on ROM and known as RXBOOT.
	* When a device is booted into RXBOOT, the prompt is router (boot)>.
	* The configuration register in this case would be set to 2101.
* **2 to F**
	* Having the boot field set to a value from 2 to F indicates the IOS image that should be loaded from flash memory.
	* To boot an IOS image from flash memory, the configuration register would have a value of 2102 through 210F.

1 = RXBOOT
6 = Ignore the contents of NVRAM (ignore startup config)
8 = Break disabled

You need to ignore startup-config

C. 6

268. What is the new configuration register value after you configure it to bypass the loading of NVRAM?

A. 0x2142 (enable 6 bit, start from 0)

2102: 0010 0001 0000 0010
2142: 0010 0001 0100 0010

269. Extended access lists use what range for access list numbers?

| Type        | ACL Numbers        |
| ----------- | ------------------ |
| IP Standard | 1-99, 1300-1999    |
| IP Extended | 100-199, 2000-2699 |

C. 100-199

270. Your manager has asked you to block traffic from the system with the IP address of 192.168.5.100. You have configured an access list using the commands show in the figure below, but now no traffic from any system can pass through the interface. Why?

![[Pasted image 20251227121421.png]]

A. Access lists have an implied deny all at the bottom

271. What type of access control list only allows you to filter traffic by the source IP address?

B. Standard

272. What type of access control list allows you to filter by source and destination IP address, source and destination port, and protocol?

C. Extended

273. You would like to ensure that only systems on the 216.83.11.64/26 subnet can telnet into your router. What commands would you use?

```
### enable ACL in vty line
Router(config)#line vty 0 4
Router(config-line)#acess-class STANDARD_ACL_# in|out
```

```
access-list 20 permit 216.83.11.64 0.0.0.63
line vty 0 4
access-class 20 in
```

274. You have created a standard access list # 20 that permits a group of IP addresses. You would like to use this access list to control who can telnet into the router. What command would you use on the telnet ports?

A. `access-class 20 in`

275. What is the result of the access list in the figure below?

![[Pasted image 20251227121832.png]]

**`access-list 100 deny ip host 192.168.10.50 192.168.2.0 0.0.0.255`**
source addr : 192.168.10.50
dest addr   : 192.168.2.0/24

**`access-list 100 deny tcp host 192.168.10.50 host 3.3.3.3 80`**

A. It denies the 192.168.10.50 system from accessing the 192.168.2.0 network, denies the system of 192.168.10.50 from accessing the website on 3.3.3.3, and permits all others.

276. What type of memory stores the routing table on Cisco routers?

B. RAM (VRAM)

277. Computer A is sending data to computer B on a remote network. Looking at the ­location of the packet in transit in the figure below, which of the following ­statements are true? (Select as many as apply.)

![[Pasted image 20251230132615.png]]

**When PC-A sends**
DEST IP   : Computer B
DEST MAC  : Router R1
Source IP : Computer A
Source MAC: Computer A


**When PC-B recives**
DEST IP   : Computer B
DEST MAC  : Computer B
Source IP : Computer A
Source MAC: R1

A. The destination IP address is that of Computer B
E. The source MAC address is that of router R1

278. When a router receives a packet what does it do? (Select two.)

Determine dest ip (if ip is in same network, send packet if it is not, check routing table)

A. Determines if a destination route exists in the routing table and what the next hop is
C. Looks at the destination IP address in the packet


279. What routes exist by default on your router?

C. Connected routes

280. Which of the following represents one of the downfalls of static routes?

C. They are manually configured by the router administrator

281. Which of the following represents an advantage of static routing?

B. No network bandwidth is being used by protocols sharing routing tables

282. You are the administrator for router R1 and have configured a static route to the 216.83.11.0 network. Your company has loaded RIPv1 on all the routers on the network, and router R2 shares knowledge of a route to the 216.83.11.0 network with a RIP update. Which route will your router use?

B. The static route (administrative distance is 1)

283. Which of the following two actions must a router do with an incoming packet in order to send it to its destination? (Choose two.)

A. Determine if a route exists in the routing table
D. Look at the destination IP address of the packet

284. What is the process called that the IP ­protocol uses to determine if the system it is trying to communicate with is on a ­different network?

A. Routing

285. What is the command to delete a static route?

A. `no ip route`

286. Your manager has disabled routing on the router by mistake. What command could you use to enable routing again?

A. `ip routing`

287. What is the result of the following commands being entered on the route?

```
RouterA>enable
RouterA#config term
RouterA(config)#ip route 217.56.33.48 255.255.255.240 26.10.20.2
```

Add static route. If destination IP address is in 217.56.33.48/28 network, forwarded 20.10.20.2.

B. Any data destined for the 217.56.33.48/28 network is forwarded to the IP address of 26.10.20.2

288. You have configured static routing on your router and would like to remove an entry from the routing table. What command would you use?

C. `no ip route 27.0.0.0 255.0.0.0 26.0.0.2`

289. What command adds a static route?

D. `ip route 35.0.0.0 255.0.0.0 22.0.0.1`

290. You type the following command into the router. Which of the following statements is true as a result of the command?

```
Ip route 200.45.7.0 255.255.255.224 22.202.33.10 10
```

200.45.7.0/27, first subnet (Network ID: 200.45.7.0, Brd: 200.45.7.31/27)

D. The administrative distance to the destination network is 10

291. You are the administrator for a small network made up of two routers. What is a quick method you can use to configure routing between all networks?

B. Configure the GWLR on both routers to point to one another

292. Your routers are running the RIP routing protocol and you type the following command. What is the outcome of typing this command?

```
Ip route 0.0.0.0 0.0.0.0 55.12.4.38
```

A. if there is no matching destination network in the routing table, the router will send the packet to 55.12.4.38

293. You are the administrator for router R1. Looking at the figure below, what command would you use to add a static route the missing network?

![[Pasted image 20251230135444.png]]

D. `ip route 13.0.0.0 255.0.0.0 12.0.0.2`

294. Tom, one of the network administrators in your office, types the following command into the router. Which of the following statements is true as a result of the command?

```
### add static route and set administrative distance to 10
Ip route 200.45.7.64 255.255.255.224 22.202.33.10 10
```

200.45.7.64 is thrid subnet (Network ID: 200.45.7.64, Brd: 200.45.7.95)

C. Packets destined for 200.45.7.89 will be forwarded to 22.202.33.10.

295. You are considering configuring default routes on your routers. Which of the following are benefits of default routes? (Select two.)

A. They allow communication to networks not appearing in the routing table
D. the routing table size is kept to a minimum

When you configure a default route, you are essentially telling the router where to send the data if the router does not know how to reach a certain network.

296. Which of the following describes the gateway of last resort?

B. The address that your router will forward a packet to when it does not have a route for that packet.

297. Your manager asks you what the purpose of a default route is on the router. What would you say?

C. The default route is used when there is no other route to the destination.

298. You would like to configure the GWLR to forward traffic to your ISP_s router, which uses the IP address of 145.66.77.99. What command would you use?

D. `ip route 0.0.0.0 0.0.0.0 145.66.77.99`


299. What command is used to view your ­routing table?

B. `show ip route`

300. What does the following entry in the routing table signify?
 
 ```
 S* 0.0.0.0 [1/0] via 56.0.0.1
 ```

B. Gateway of last resort

301. Using the following output, how will data be sent to 26.13.45.222?

```
ROUTER87#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, …
(Additional codes omitted for briefness)
Gateway of last resort is not set
S 29.0.0.0 [1/0] via 26.0.0.2
C 26.0.0.0/8 is directly connected, Serial0/0/0
C 25.0.0.0/8 is directly connected, FastEthernet0/1
```

C. It will be sent out Serial0/0/0

302. You are configuring router on a stick. Which of the following commands would create a sub-interface on the router?

C. `interface fa0/0.20`


303. Using the following output, how will data be sent to 29.66.84.2?

```
ROUTER87#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, …
(Additional codes omitted for briefness)
Gateway of last resort is not set
S 29.0.0.0 [1/0] via 26.0.0.2
C 26.0.0.0/8 is directly connected, Serial0/0/0
C 25.0.0.0/8 is directly connected, FastEthernet0/1
```

D. Data will be sent to 26.0.0.2.

304. Using the following output, how will data be sent to 25.33.200.2?

```
ROUTER87#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, …
(Additional codes omitted for briefness)
Gateway of last resort is not set
S 29.0.0.0 [1/0] via 26.0.0.2
C 26.0.0.0/8 is directly connected, Serial0/0/0
C 25.0.0.0/8 is directly connected, FastEthernet0/1
```

A. Data will be sent out FastEthernet0/1

305. Using the output shown below, which resulted from the show ip route command, what is the hop count to reach the 29.0.0.0 network?

```
Static, IP, administrative distance/hop count via dest ip
S 29.0.0.0 [1/5] via 26.0.0.2
```

B. 5

306. Which of the following statements are true in order to get traffic to route between the two VLANs in the figure below? (Select two.)

![[Pasted image 20251230165959.png]]

B. Configure F0/12 on SW1 for trunk mode
E. Configure sub-interfaces on R1

In order to configure routing between two VLANs with a single router (router on a stick), you must create sub-interfaces on the router for the single interface connected to the switch.
You also must configure the port on the switch that the router is connected to trunk mode so it carries all VLAN traffic across the link.

307. You are configuring router on the stick so the router will route traffic between VLAN 10 and VLAN 20. You configure the sub-interfaces with the following commands. During testing, you notice that the router is not routing between the VLANs. What is missing?

```
interface fa0/0.20
ip address 192.168.20.1 255.255.255.0
interface fa0/0.10
ip address 192.168.10.1 255.255.255.0
```

C. You need to enable dot1q and specify the VLAN on each interface

When setting up each of the sub-interfaes, you need to specify the encapsulation protocol to use for VLAN tagging and the VLAN ID that the interface is part of

```
interface fa0/0.20
encap dot1q 20
ip address 192.168.20.1 255.255.255.0

interface fa0/0.10
encap dot1q 10
ip address 192.168.10.1 255.255.255.0
```

308. The router will use the route that has the *lowest* administrative distance.

309. Your router receives knowledge of a network via RIPv2 that it already has a static route to. Which pathway will be used?

A. The static route entry

310. What is the administrative distance of RIPv1?

C. 120

311. What is the administrative distance of a connected route?

D. 0

312. Which routing feature has an administrative distance of 120?

C. RIP

313. What is the administrative distance of a static route?

A. 1

314. When a router receives a routing table update for a route it does not have in its routing table, what does it do?

C. Adds the route to the routing table

315. Which of the following protocols are distance vector routing protocols supported by multiple vendors and not just Cisco?

B. RIP

Cisco proprietary = IGRP, EIGRP (hybrid)

Distance vector protocol = RIP, IGRP
Link state protocol      = OSPF, IS-IS
Hybrid                   = EIGRP, BGP

316. Which of the following protocols are link state routing protocols? (Select all that apply.)

A. EIGRP
D. OSPF

317. Which routing protocol is limited to 15 hops?

C. RIP

255 hops = IGRP, EIGRP

318. A `______` routing protocol is a routing protocol that knows only about how many hops away a network is

A. Distance vector

A distance vector routing protocol only knows how many hops (routers) away a network is; it does not know if a link is available or how much bandwidth the link has.

319. Dynamic routing protocols run at what layer of the OSI model?

C. Layer 3

320. Which of the following is true of distance vector routing protocols? (Select two.)

B. Routers share routing table with neighboring routers
E. Sends entire routing table as an update

RIP  = 30 seconds 
IGRP = 90 seconds

Distance vector protocol has single routing table.

321. Which of the following are true statements about RIPv2? (Select three.)

A. It is a classless routing protocol
B. Maximum hop count of 15

**RIPv1**
* doesn't support classless

**RIPv2**
* Support classless

322. (True/False): RIPv1 is an example of a classless routing protocol.

B. False (But RIPv2 is)

323. How frequently does RIPv2 send out ­routing updates?

C. Every 30 seconds.

IGRP = 90 seconds

324. Which of the following is true of a distance vector routing protocol? (Select two.)

B. Sends periodic updates
C. Updates routing table based on updates received from neghboring routers

325. OSPF is an example of a `____________` routing protocol.

C. Link state

326. Which of the following is true regarding link state routing protocols? (Select two.)

A. Routers share routing table information with all other routers on the network
D. Maintains multiple tables in memory

327. Your router receives an update for a route from both RIP and OSP (F) Which route will the router add to its routing table?

B. OSPF

Because OSPF has a lower administrative distance than RIP.

| Value | Route Type                                                       |
| ----- | ---------------------------------------------------------------- |
| 0     | Connected interface route                                        |
| 1     | Static route                                                     |
| 90    | Internal EIGRP router (within the same AS)                       |
| 110   | OSPF route                                                       |
| 120   | RIPv1 and v2 route                                               |
| 170   | External EIGRP (from another AS)                                 |
| 255   | Unknown route (considered an invalid route and will not be used) |

328. What does the term “ routing by rumor ” refer to?

D. Sharing routes learned from one neighbor to other neighboring routers

329. Which of the following represents a downfall of distance vector routing protocols over link state?

A. Convergence time

Because distance vector routing protocols only share the routing table with neighboring routers, it could take a long time for all routers to know about changes to the routing table.

330. Your manager is thinking about upgrading from RIPv1 to RIPv2 and asks about the benefit of RIPv2. What would you say?

D. Supports VLSM (classless routing)

RIPv2 has a few benefits. It sends routing updates out via multicast traffic (RIPv1 uses broadcast). Also RIPv2 is classless and suppots variable length subnet masks (VLSM)

331. Which routing protocol sends the entire routing table to neighboring routers every 30 seconds?

B. RIP

IGRP = 90 seconds.

332. You wish to see how RIP has been configured on the router. What command would you use?


B. `RouterA#show protocols`

333. When looking at the routing table, how do you know which entries have been learned through RIP?

A. The entry with code R

334. What version of RIP supports VLSM?

B. RIPv2

335. What command enables RIP debugging?

C. `debug ip rip`

336. You need to configure RIPv2 on your router. Which of the following commands would do this?

```
R1(config)#router rip
R1(config-router)#network 25.0.0.0
R1(config-router)#network 26.0.0.0
R1(config-router)#version 2
```

337. What is the default administrative distance for RIP?

C. 120

| Value | Route Type                                                       |
| ----- | ---------------------------------------------------------------- |
| 0     | Connected interface route                                        |
| 1     | Static route                                                     |
| 90    | Internal EIGRP router (within the same AS)                       |
| 110   | OSPF route                                                       |
| 120   | RIPv1 and v2 route                                               |
| 170   | External EIGRP (from another AS)                                 |
| 255   | Unknown route (considered an invalid route and will not be used) |

338. You have three interfaces on the router: one configured for the 27.0.0.0/8, while the other two interfaces are configured with 29.1.0.0/16 and 29.2.0.0/16. If you want to use RIPv2, which of the following represents the least number of commands you would need to use?

```
R1(config)#router rip
R1(config-router)#network 27.0.0.0
R1(config-router)#network 29.0.0.0
R1(config-router)#version 2
```

When specifying the networks for the RIP routing protocol, you need to specify only the classful addresses, even when the address space has been subnetted.

339. Which of the following is true about RIPng?

#### RIPng

RIPng is defined in RFC 2080. It is actually similar to RIP for IPv4, with these characteristics:

* It's distance vector protocol
* The hop-count limit is 15.
* Split horizon and poison reverse are used to prevent routing loops.
* It is based on RIPv2

These are enhancements in RIPng:

* An IPv6 packet is used to transport the routing update
* The all RIP routers multicast address (FF02::9) is used as the destination address in routing advertisements and is delivered to UDP port 521.
* Routing updates contain the IPv6 prefix of the router and the next-hop IPv6 address.

RIPng is the next generation version of RIP for IPv6 and requires you to enable RIPng on each interface.

A. You need to enable RIPng on each interface.

RIPng uses broadcast -> using multicast (FF02::9)
RIPng is classful -> based on RIPv2 (classless)
RIPng supports up to 30 hops -> hop count limit is 15.


340. You want to disable sending RIP messages on interface serial 0/1. Which of the following  commands would you use?

B. `RouterA(config-router)#passive-interface serial 0/1`

341. When RIP has multiple routes to a destination with the same hop count, what will it do?

B. RIP will load-balance the links to that destination

342. What is the administrative distance of OSPF?

A. 110

343. Which of the following would be used as the OSPF router ID if all were configured?

A. Highest IP address assigned to a loopback interface

With OSPF, the router ID is chosen by the highest IP address used by a loopback interface. If there are no loopback interfaces, then the router ID will be that of the highest IP address of the physical interface.

344. What type of OSPF router connects one or more areas to the backbone network?

D. ABR (area border router)

345. You are troubleshooting OSPF configuration. What command would you use to verify your configuration?

B. `show ip ospf`

346. When configuring OSPF on your router, you have specified an interface to be part of area 0. What is the area known as?

A. Backbone

347. You need to configure OSPF on your router. Which of the following most accurately depicts the commands used?

```
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
```


When configuring OSPF, you must first use the `router ospf <ID>` command in global configuration, where the ID is the process ID you want to assign to the OSPF process.
You then specify the network interfaces to run OSPF on by using the `network` command and the wildcard mask while specifying the area.

348. You want to configure your router for OSPF and run it on interfaces that are on the 192.168.1.0  network. Which of the following commands would you use?

```
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
```

349. When troubleshooting OSPF, which of the following commands would you use? (Select two.)

B. `show ip protocols`
E. `show ip ospf neighbor`

350. Which of the following commands enables an OSPFv3 process?

D. `ipv6 router ospf 5`

OSPFv3 is OSPF for the IPv6 protocol.

351. Which of the following commands would you use to change the router ID of an OSPF router?

```
Router ospf 1
Router-id 8.8.8.8
```

When using OSPF, you can change the router ID by using `router ospf 1` command to navigate to the OSPF process, and then type `router-id <ip_address>` to signify the ID.

352. Bob calls complaining that he cannot access the network. When you look at the switch, you notice the port he is connected to is displaying an amber light. What does this mean?

D. The port has been disabled

353. After powering on the Cisco switch you notice that the System LED is green. What does this indicate?

A. The system started without problems and is operational

354. The switch has the display mode set to Status and port 4 is blinking green. What does this indicate?

D. Data is being transmitted

355. After powering on the Cisco switch, you notice that the System LED is amber. What does this indicate?

D. There were POST errors

356. You wish to verify which ports on the switch are running in full duplex mode. How would you do this?

A. Switch the display mode to duplex and watch for a solid green light on the different ports.

When you toggle the display mode of the switch to duplex, the port LEDs display a green solid light to indicate they are running in full duplex mode.
If the port is running in a half duplex, no light appears on the port LED.

357. When your switch is set to the display mode of speed, what does the LED for a 1 Gbps port  look like?

D. Flashing green

This could change per switch, but typically, if the port is running at 1 Gbps it displays a flashing green light when the switch display mode is toggled to speed mode.

358. The switch display mode is set to duplex and you have a computer connected to port number 4 with no light displaying on the port. What is the problem?

C. The port is running in half duplex

359. You have the display mode set to speed on your 10/100/1000 switch. Three of the ports are not displaying a light. What does this indicate?

C. Ports are running at 10 Mbps

360. A switch is an example of a layer-___ device.

B. 2

361. When a switch receives a frame, what address does it look to in order to determine where to forward the frame?

A. The destination MAC address

363. Which of the following descriptions matches a collision domain?

A. A group of systems that can have their data collide with one another

* broadcast domain
	* A group of systems that receive one another's broadcast messages

364. Which of the following are considered core services offered by a switch? (Select three.)

A. Filtering and forwarding
B. Loop avoidance
E. Address learning

365. Which of the following descriptions matches a broadcast domain?

A. A group of systems that receive one another's broadcast messages

366. Looking at the figure below, what does the switch do when it receives a frame d­estined for the MAC address of 00d0.bc8a.2766?

![[Pasted image 20251231162137.png]]

D. It forwards the frame to port 12.

367. What does a switch do with a frame it receives that is destined for a MAC address that is not stored in the MAC address table?

B. Floods the frame

368. Your manager is wondering what the benefit of switches over hubs are. What would you say?

B. Switches filter traffic and only send the traffic to the destination port, while a hub sends all traffic to all ports.

369. How many collision domains exist on a 24 port hub versus a 24 port switch?

A. Hub - 1 collision domain/switch - 24 collision domains

370. Which of the following does the switch use to populate the MAC address table on the switch?

A. The source MAC address of the frame

In order to dynamically learn which port each system is connected to, the switch uses the source MAC address of a frame as it recives it.

Switche uses the source MAC address to populate the MAC address table (CAM table), but uses the destination MAC address of the frame to determine where to send a frame

371. Looking at the figure below, how many collision domains and broadcast domains are there?

![[Pasted image 20251231162541.png]]

A. 2 brd, 5 collision domain

Each interface off the router will create a separate broadcast domain (2), while each interface is creating a separate network segment (collision domain, 5)

372. You have three switches connected together with crossover cables. How many broadcast domains exist?

B. 1

373. You have linked two switches together with a straight-through cable but cannot seem to communicate between the two switches. What is the solution?

C. Use a crossover cable

straight-through cable is used for dissimilar devices (pc - switch, switch-router), crossover cable is used for connect between simmilar devices (pc-router, switch-switch, router-router ...)

374. Which switch operation mode waits to receive the first 64 bytes of the frame before forwarding the frame on to its destination?

D. Fragment-free

* Store-and-Forward
	* recive entire frame
* Cut-Through
	* read the destination MAC address and forwarding
* Fragment-Free (modified cut-through, runtless)
	* reads first 64 bits of frame

#### Store-and-Forward Switching

*Store-and-Forward* swithcing is the most basic form of switching.

With store-and-forward switching, the layer 2 device must receive the entire frame into the buffer of the inbound port and will check the frame for errors (the FCS checksum field on the frame) before the switch will perform any addtional processing of the frame.

When checking the FCS, commonly called a *cyclic redundancy check (CRC)*, the switch will calculate a CRC value, just as the source did, and compare this value to what was included in the FCS of the frame.
If they are same, the frame is considered good, and the switch can forward the frame out the correct destination port of the switch.
If the FCS value in the frame and computed CRC value are different, the switch will drop the frame.

The advantage of store-and-forward is that the switch can check the frame for errors before forwarding it on.
The downfall is the wait time for the switch to read the entire frame, which is longer than that of the other methods.

#### Cut-Through Swithcing

Cut-Through switches were designed to improve the performance of a switch as it relates to processing frames.

With cut-through switching, the switch only waits to read the destination MAC address before it begins forwarding the frame.
Cut-through switching is faster than store-and-forward switching.

its biggest problem, though, is that the switch may be forwarding bad frames, through the header may be legible, the rest of the frame could be corrupted from a late collision.

#### Fragment-Free Switching

*Fragment-free* switching is a modified form of cut-through switching.

Whereas cut-through switching reads up to the destination MAC address field in the frame before making a switching decision, fragment-free switching makes sure that the frame is at least 64 bytes long before switching it (64 bytes is the minimum legal size of an Ethernet frame).

The goal of fragment-free switching is to reduce the number of Ethernet *runt frames* (frames smaller than 64 bytes) that are being switched.

Fragment-free switching is sometimes called *modified cut-through* or *runtless* switching.
With fragment-free switching, a switch could still be switching corrupt frames, because the switch is check only the first 64 bytes.

375. What is the general term for data received and processed by a switch?

A. Frame

**PDU**

| TERM    | LAYER                                         |
| ------- | --------------------------------------------- |
| Data    | Application, presentation, and session layers |
| Segment | Transport                                     |
| Packet  | Network                                       |
| Frame   | Data link                                     |
| Bits    | Physical                                      |

376. Which switch operation mode waits until the entire frame is received before forwarding the frame on to its destination?

A. store-and-forward

377. Looking at the figure below, what does the switch do when it receives a frame destined for the MAC address of 00d0.bc8a.2788?

![[Pasted image 20251231172844.png]]

C. It will flood the frame

378. Which switch operation mode starts forwarding the frame off to its destination as soon as the destination MAC address is received?

D. Cut-through

379. When a packet is sent from a system on your network to a system on another network, which of the following is true of the packet as it is passed to your router from your system?

B. Destination IP Address: the_remote_system / Destination MAC Address: your_router

380. Computer A is sending a message to ­computer B. Looking at the figure below, which of the f­ollowing are true of the ­message as it travels between R2 and SW2? (Select three.)

![[Pasted image 20251231173130.png]]

SOURCE IP: PC-A
DEST IP  : PC-B

SOURCE MAC: R2
DEST MAC  : PC-B

B. The destination MAC address is that of Computer B
E. The destination IP address is that of Computer B
G. The source MAC is that of R2

381. Computer A is sending a message to ­computer B. Looking at the figure below, which of the following are true of the ­message as it travels between R1 and R2? (Select three.)

![[Pasted image 20251231173337.png]]

Source IP : PC-A
DEST IP   : PC-B

Source MAC : R1
DEST MAC   : R2

B. The source IP address is that of Computer A
C. The source MAC address is that of router R1
E. The destination IP address is that of Computer B

382. Which of the following commands is used to change the name of the switch?

B. `Switch(config)#hostname NY-SW1`

383. You wish to display your MAC address table. What command do you use?

D. `Switch# show mac-address-table`

384. our manager has asked you to telnet into one of the switches in the Las Vegas office, but you cannot seem to connect. You can telnet into the switch when you are in the Las Vegas office, but you cannot seem to telnet when you are on a remote network. What could be the problem?

B. No default gateway address assigned on your switch

385. Which of the following commands are used to assign an IP address to the switch?

```
NY-SW1(config)#interface vlan1
NY-SW1(config-if)#ip address 23.0.0.25 255.0.0.0
```

386. Which of the following commands assigns a default gateway address to your switch?

C. `Switch(config)#ip default-gateway 24.0.0.1`


387. How long does a dynamic entry stay in the MAC address table?

D. 300 seconds from last time used

A dynamic entry in the MAC address table is one that is learned automatically by the switch based off traffic it has received and stays in the MAC address table for 300 seconds since last time it was used (by default).

388. While looking at the figure below, what does the type of dynamic mean?

C. The switch has learned the address based off traffic received

389. You would like to change the amount of time an entry stays in the MAC address table on the switch to 400. What command would you use?

D. `mac-address-table aging-time 400`

390. Your manager would like a port description configured to port number 5 on the switch labeling the port as being used by the file server. What set of commands would you use?

```
Switch#config term
Switch(config)#interface f0/5
Switch(config-if)#description File Server Port
```

391. Jeff has disabled all of the unused ports on the switch. You would like to enable port 12.  What command would you use?

C. `no shutdown`

392. You wish to configure port 8 on the switch to negotiate its speed with the connecting system. What command would you use?

A. `speed auto`


393. You wish to configure port number 8 for full duplex. What command would you use?

D. `duplex full`

394. You would like to configure all 24 ports on the switch for 100 Mbps. Which of the following ­represents the best way to do this?

```
Switch(config)#interface range f0/1 - 24
Switch(config-if-range)speed 100
```

395. Which command sets the port speed to 100 Mbps?

D. `switch(config-if)#speed 100`

396. What command would you use first to configure a setting on multiple ports on the switch?

D. Use the `interface range` command

397. You need to disable ports 6 to 12 on your switch. What commands would you use?

```
Switch(config)#interface range f0/6 - 12
Switch(config-if-range)#shutdown
```

398. What command shows you MAC addresses associated with each port on the switch?

D. `show mac-address-table`

399. Looking at the figure below, what is the duplex setting of the port set to?

![[Pasted image 20251231174514.png]]

B. Half duplex

400. What command was used to create the output displayed in the figure below?

![[Pasted image 20251231174538.png]]

D. `show interface f0/8`

401. You wish to view the speed that port number 5 is using. What command would you use?

C. `show interface f0/5`

402. Looking at the figure below, what is the speed of the port set to?

![[Pasted image 20251231180710.png]]

C. 100 Mbps

403. You are administering a 24-port switch that is divided into 4 VLANs. How many collision domains exist?

B. 24

Each port on a switch creates a separate collision domain.

404. You are administering a 24-port switch that is divided into 4 VLANs. How many broadcast domains exist?

D. 4

405. Which of the following commands would you use to enable a port on the switch?

C. `no shutdown`

406. You are having trouble connecting to port 8 on the switch. You view the status of the port (shown in the figure below). What command would allow the port to function properly?

![[Pasted image 20251231180928.png]]

A. `no shutdown`

407. Looking at the figure below, what is the bandwidth of the port set to?

![[Pasted image 20251231181046.png]]

C. 100000 Kbps (BW)
 
408. When you connect the workstation to port 6, you can ping three other systems on the network, but you cannot seem to connect to the file server. You verify that others can connect to the file server, but you cannot connect to those other systems as well. What could be the problem?

D. You are in a different VLAN than the file server

409. You have a workstation connected to port 10 on the switch, but for some reason you cannot ping any other system on the network. You view the configuration of the port and get the output display in the figure below. What is likely the problem?

![[Pasted image 20251231181342.png]]

B. The port is disabled

410. What Cisco switch feature will allow you to control which systems can connect to a port on the switch?

B. Port Security

411. What option allows you to configure a static MAC address on the switch by using the MAC of the connected system?

C. sticky

The sticky option allows you to configure a port to allow traffic only from the current or first MAC address (assuming you set the maximum addresses to 1)

412. Why would a network administrator configure Port Security on the switch?

D. To prevent unauthorized access to the network

413. Which of the following violation modes would block traffic not coming from the correct MAC address, but allow traffic from the specified MAC address?

B. restrict

414. Which of the following actions disables the port when an address violation occurs?

B. shutdown

415. What command was used to create the output displayed in the figure below?

![[Pasted image 20251231181715.png]]

B. `show port-security interface f0/6`

416. What mode must you place the interface into before you are able to configure Port Security on the interface?

A. Access

417. You are having trouble with a system connecting to port 5 on the switch. You want to see if the port has been configured for Port Security. What command do you use?

C. `show port-security interface f0/5`

418. What command was used to create the output displayed in the figure below?

![[Pasted image 20251231181834.png]]

B. `show port-security address`

419. What are the three actions that you can configure when an address violation occurs? (Select three.)

A. restrict
C. shutdown
E. protect

* **Protect**
	* This command sends an alert to the administrator when an unauthorized device is connected to the port.
* **Restrict**
	* The `restrict` setting results in the port being able to forward frames for the authorized device only.
	* If an unauthorized device connects to the port, the switch will drop those frames.
* **Shutdown**
	* With this command, the port is disabled the second an authorized device connects to the port.
	* The port is disabled until the administrator issues the `no shutdown` command.
	* This is the default violation mode if you don't specify the mode.

420. Your manager has heard a lot about the VLAN feature available on switches and is wondering what the benefit is. What would your response be?

A. To create communication boundaries

421. Which of the following switch features would you use to create multiple broadcast domains?

D. VLANs

422. You wish to view the VLAN configuration on the switch. What command do you use?

B. `show vlan`

423. You are troubleshooting communication problems and suspect that a port is in the wrong VLAN. What command can you use to verify the VLANs that exist and what ports exist in each VLAN?

A. `show vlan`

424. Which of the following protocols are used to carry VLAN traffic between switches? (Choose two.)

C. 802.1q (dot1q)
D. ISL

Cisco supports two Ethernet trunking methods:
* Cisco's proprietary InterSwitch Link (ISL) protocol for Ethernet
* IEEE 802.1Q, commony referred to as *dot1q* for Ethernet

425. What command would you use on a port to specify that it is allowed to carry all VLAN traffic across the port?

A. `switchport mode trunk`

426. Which of the following commands is used to create a VLAN name Floor1?

D. `vlan 2 name Floor1`

427. Which of the following commands is used to place port 6 in VLAN 2?

```
Switch(config)#interface f0/6
Switch(config-if)#switchport access vlan 2
```

428. You are troubleshooting communication to another device. What is the result of the command being executed in the figure below?

![[Pasted image 20260101181525.png]]

B. The pings were successful

429. What are the results of the command executing in the figure below?

![[Pasted image 20260101181602.png]]

A. The pings were unsuccessful

430. You are troubleshooting communication problems on a network on the other side of the ­country. What command do you use to identify at what point in the communication pathway there is a failure?

D. `traceroute ip_address`

431. What command would you use to test VTY port configuration?

D. `telnet`

432. You are testing connectivity to a system on a remote network. Using the output shown in the figure below, what is the default gateway setting of this workstation?

![[Pasted image 20260101181645.png]]

A. 23.0.0.1

433. You are connecting a switch to a router. What type of cable should you use?

A. Straight-through

434. You have your workstation connected to port 6 on the switch, but are unable to communicate on the network. What should you do first?

D. Verify your cable is connected properly

435. You are connecting two switches together. What type of cable should you use?

D. Crossover

436. You receive the following output when looking at the show interfaces command. What should you check?

```
Serial0/2/0 is down, line protocol is ­down
```

A. Check the physical aspects of the link, such as the cable

437. You are the network administrator for a small network running three VLANs on a switch. You have connected a workstation to port 8 on the switch and assigned it a valid IP address for that network. You try to communicate with another system on the network, but are unable to. What could be the problem?

D. You are connected to a port in a different VLAN than the other system

438. Looking at the figure below, Computer A is initiating communication to Computer B and sends out an ARP request. Which device responds with the ARP reply message?

![[Pasted image 20260101182120.png]]

B. R1

The arp request would be sent to router R1 because that is the device Computer A needs to send the data to in order to have the data leave the network.

439. You are the network administrator for a company that has users located in different offices. The network configuration is shown in the figure below. You have connected the switches to the routers with a crossover cable, and the computers to the switches with straight-through cables. Computer A cannot ping Computer B. What should you do?

Switches to the routers -> straight-through cable
computers to the switches -> stright-through cable

D. Use stright-through cables to connect switches to routers

440. Having a green link light indicates that which of the following conditions have been met? (Select two.)

B. The network media is attached at both ends of the link
D. a layer-2 protocol has been loaded and configured at either end of the link

If the link light on an interface displays green, it indicates that a physical connection is esatblished between the two ports, and a data link protocol is configured at either end.

441. You are the network administrator for a company that has users located in different offices. The network configuration is shown in the figure below. You have connected the switches to the routers with a straight-through cable, and the computers to the switches with straight-through cables. Computer A cannot ping Computer B. What should you do?

![[Pasted image 20260101182611.png]]

216.x.x.x => class C network, default subnet mask = 255.255.255.0 (/24)

216.83.11.0/27
216.83.11.32/27
216.83.11.64/27
216.83.11.96/27
216.83.11.128/27
216.83.11.160/27
216.83.11.192/27
216.83.11.224/27

PC A is in 216.83.11.96 network

C. Change the IP address on Computer A

442. You use the following commands to create an access control list on the FastEthernet port of the router. Users are now reporting that they cannot communicate with the intranet site. What is the problem?

```
R1(config)# access-list 102 permit tcp any any eq 23
R1(config)# access-list 102 permit tcp any any eq 25
R1(config)# interface fastethernet0/1
R1(config-if)# ip access-group 102 in
```

C. There is an implicit deny all traffic at the end of the ACL.

443. You have a switch using multiple VLANs that is connected to a router using a straight-through cable. You have configured the router as a router on the stick. You have configured the sub-interfaces on the router, but communication is not occurring between the VLANs. How do you fix the problem?

B. Enable trunking on the port connecting the switch to the router


444. You are the network administrator for a company that has users located in different offices. The network configuration is shown in the figure below. You have connected the switches to the routers with straight-through cables, and the computers to the switches with straight-through cables. Computer A cannot ping Computer B. Why not?

![[Pasted image 20260101183216.png]]

A. Computer B's default gateway is invalid (broadcast address)

445. Looking at the output of the show interfaces command seen in the figure below, what is the MAC of the interface using the IP address 23.0.0.1?

![[Pasted image 20260101183353.png]]

D. 0010.11d9.d001

446. What command was used to create the output shown in the figure below?

![[Pasted image 20260101183434.png]]

B. `show ip arp`

447. Which of the following commands can you use to figure out the IP address of a neighboring router?

B. `show cdp neighbors detail`

448. Looking at the output of the show interfaces command seen in the figure below, what is the encapsulation protocol being used by the serial port?

![[Pasted image 20260101183525.png]]

D. HDLC

449. Looking at the output in the figure below, what end of the point-to-point link is Serial0/2/0?

![[Pasted image 20260101183556.png]]

A. DCE (need to set clock rate)

450. If an interface shows as administratively down, what command do you use to bring the interface up?

C. `no shutdown`

451. How do you view your ARP cache on a Cisco router? (Select two.)

A. `show ip arp`
D. `show arp`

452. Looking at the output in the figure below, what is the clock rate of serial0/2/0?

![[Pasted image 20260102152345.png]]

B. 64 Kbps

453. You receive the following output from the show interfaces command. What should you check?

```
Serial0/2/0 is up, line protocol is down
```

C. Check that the data link protocol is configured (HDLC, PPP protocol)

454. Looking at the output of the show interfaces command seen in the figure below, which of the following statements is true?

![[Pasted image 20260102152504.png]]

Serial port is using HDLC

D. FastEthernet0/1 has not been configured

455. You are having trouble communicating with a remote network. You have checked the status of the interfaces: Each interface is up, and the IP addresses look fine. What command do you use next?


A. `show ip route`

456. Which of the following commands would you use to troubleshoot issues at layer 3? (Select three.)

A. `show ip interface brief`
C. `show protocols`
E. `show ip route`

Any commands that relate to routing or the IP protocol would be layer-3 troubleshooting commands.

457. What command do you use to view whether your serial port is the DCE or DTE device?

B. `show controllers serial 0/0`

458. Which of the following commands would you use to check for issues at layers 1 and 2? (Select two.)

B. `show interfaces`
D. `show controllers`

459. Which of the following could be an indication of excessive collisions or bad packets submitted by a malfunctioning network card? (Select two.)

A. Runt
C. CRC

The runt and CRC count can be viewed on an interface to determine the number of small packets recived and the number of packets recieved that do not have the CRC information matching the caculated checksum. Both of these are an indication of collisions on thenetwork or a bad network card sending grabage packets out on the network

460. Looking at the output in the figure below, how many packets have been received that were less than 64 bytes?

![[Pasted image 20260102153205.png]]

6 runts

B. 6

461. You would like to see the number of runt packets received. What command would you use?

D. `show interfaces`

462. You are troubleshooting performance issues on the network and suspect that a high number of packets are being retransmitted due to collisions. You use the show interface command to view the status on the interface. Looking at the output in the figure below, how many packets have been retransmitted due to collisions?

![[Pasted image 20260102153315.png]]

6 runts, 87 collisions

C. 87

463. Which of the following commands is recommended to disable debugging?

D. `no debug all`

464. You are troubleshooting problems with RIP and suspect that your router is no longer receiving RIP updates. What command would you use to help diagnose the problem?

A. `debug ip rip`

465. You would like to view the details of all packets sent or received by the router. What command would you use?

D. `debug ip packet`

466. What type of attack involves the hacker overloading a system and causing the system to crash or not perform its job role?

B. DoS attack

467. You would like to control communication between the accounting and marketing groups. What feature of a Cisco switch will allow you to do this?

C. VLANs

468.  What network technology could be used to encrypt communication from point A to point B over an untrusted network?

A. VPN

469. What type of firewall can filter traffic by analyzing the payload of a packet?

C. Application layer firewall

An application layer firewall, also known as a proxy server, can filter traffic not only by IP address and port information, but also by analyzing the application data and determining if the packet is allowed through based on the contents of the data.

470. What type of firewall filters traffic based on the context of the conversation and the layer-3 and layer-4 header?

D. Stateful packet inspection firewall

A stateful packet inspection firewall is more advanced that a simple packet filtering firewall in the sense that it understands the timing of the pcaket and whether or not that packet is suited for that port of the communication

471. What type of attack involves the hacker altering the source IP address of a packet in order to bypass your packet filtering rules on the router?

B. Spoofing

472. Your manager has asked you make sure that the Cisco router configuration files are protected from the intruders on the Internet. Which actions would you ­recommend? (Select two.)

B. User a firewall to control access to the device from the Internet
D. Use SSH to remotely configure the device

473. You are helping the security team create a network security plan for the business. Which of the following should you include in the plan?

B. Lock network equipment in a secure location

474. You have been asked to review the running-configuration of a customer’s router and identify potential security issues related to the configuration. Review the output of the `show  running-config` command, shown below. Which of the following are potential security issues with this configuration? (Select two.)

```
ROUTERB#show running-config
Building configuration…
(output omitted for briefness)
service timestamps log datetime msec
no service password-encryption
!
hostname ROUTERB
!
ip subnet-zero
!
interface FastEthernet0/1
ip address 27.0.0.1 255.0.0.0
no ip directed-broadcast
!
interface Serial0/0/0
ip address 26.0.0.2 255.0.0.0
no ip directed-broadcast
!
router rip
network 26.0.0.0
network 27.0.0.0
!
banner motd
Welcome to Glen’s router! You have connected to 192.168.0.1!
!
line con 0
password myc0nP@ss
login
line aux 0
line vty 0 4
password telnet
login
end
```

B. Passwords are not encrypted
D. The banner is using inappropriate text

475. The junior IT technician has configured the router for usernames and passwords, but the router is not prompting for usernames and passwords when connecting to the console port. You review the configuration, shown below. What is the problem?

```
ROUTERB#show running-config
Building configuration…
(output omitted for briefness)
no service password-encryption
!
hostname ROUTERB
!
ip subnet-zero
!
Username bob password P@ssw0rd
Username sue password Pa$$w0rd
!
interface FastEthernet0/1
ip address 27.0.0.1 255.0.0.0
no ip directed-broadcast
!
interface Serial0/0/0
ip address 26.0.0.2 255.0.0.0
no ip directed-broadcast
!
line con 0
login
line aux 0
line vty 0 4
password telnet
login
end
```

D. The login local command should be used

476. Sue has configured the console port with a password, but it does not seem to work. You review the configuration, shown below. What is causing the problem?

```
ROUTERB#show running-config
Building configuration…
(output omitted for briefness)
!
hostname ROUTERB
!
ip subnet-zero
!
interface FastEthernet0/1
ip address 27.0.0.1 255.0.0.0
no ip directed-broadcast
!
interface Serial0/0/0
ip address 26.0.0.2 255.0.0.0
no ip directed-broadcast
!
line con 0
password Con$0le1
line aux 0
line vty 0 4
end
```

C. The `login` command is missing

477. For security reasons, you have decided to disable the built-in web server on the Cisco device. Which command would you use?

A. `R1(config)#no ip http server`

478. You would like to create a user named Bob that has privilege level 3. What command would you use?

B. `R1(config)#username Bob privilege 3 password P@ssw0rd`

479. How would you view your current privilege level on a Cisco device?

D. `show privilege`

480. What command is used to disable a port on the switch?

B. `switch(config-if)#shutdown`

481. You have been asked to configure port number 4 to only allow the system that is currently connected to the port access to the network via that port. What Cisco switch feature would you use?

D. Sticky (or manually add device mac address)

```
### Manual
VAN-SW1(config-if)#switchport port-security mac-address 0050.7966.6800

### Sticky
VAN-SW1(config-if)#switchport port-security mac-address sticky
```

482. Which violation mode would you use to disable a port when an unauthorized device connects to the port, and have the port disabled until the administrator enables it?

B. Shutdown

**Port security violation**

* **Protect**
	* This command sends an alert to the administrator when an unauthorized device is connected to the port.
* **Restrict**
	* The `restrict` setting results in the port being able to forward frames for the authorized device only.
	* If an unauthorized device connects to the port, the switch will drop those frames.
* **Shutdown**
	* With this command, the port is disabled the second an authorized device connects to the port.
	* The port is disabled until the administrator issues the `no shutdown` command.
	* This is the default violation mode if you don't specify the mode.

483. Which feature allows you to control which workstations can connect to a specific port on the switch?

C. Port security

484. You are trying to connect a system to a port on the switch, but having no luck. You use the `show interface f0/5` ­command to view the status of the port and receive the output shown below. What should you do to allow the system to ­connect via the port?

```
SW1#show interface f0/5
FastEthernet0/5 is administratively down, line protocol is down (disabled)
Hardware is Lance, address is 0060.2fba.e405 (bia 0060.2fba.e405)
BW 100000 Kbit, DLY 1000 usec,
reliability 255/255, txload 1/255, rxload 1/255
Encapsulation ARPA, loopback not set
Keepalive set (10 sec)
Half-duplex, 100Mb/s
```

D. Enable the port

485. You wish to limit which device can connect to each port on the switch by configuring port security with the MAC address of each system currently connected to the switch. What command would you use?

A. `SW1(config-if)#switchport port-security mac-address sticky`


486. Which of the following port modes supports the port security feature?

C. access

487. You would like to configure port 5 on your switch to allow only one system to connect to the port that uses the MAC address of 1111.2222.3333. Which of the following configurations would you use?

```
SW1(config)#interface f0/5
SW1(config-if)#switchport mode access
SW1(config-if)#switchport port-security
SW1(config-if)#switchport port-security mac-address 1111.2222.3333
SW1(config-if)#switchport port-security maximum 1

### optional, default is shutdown
SW1(config-if)#switchport port-security violation shutdown
```

488. Your manager would like to use a single system on the network for monitoring network traffic. You have connected the monitoring system to port 12 on the switch, but you are unable to monitor network traffic. What do you need to do? (Select two.)

A. Connect the monitoring system to port 1 on the switch
D. Configure port mirroring on the switch

In order to monitor traffic on the network, you will connect your monitoring system to the port on the switch and configure the port mirror to send all traffic to that port.

489. You wish to have the Cisco device prompt you for a username and password. What command would you use?

B. `router(config-line)#login local`

490. Your manager would like all passwords encrypted in the configuration files on the Cisco routers and switches. What command is used to encrypt all passwords in the configuration files?

D.`router(config)#service password-encryption`

491. Which of the following commands are used to configure a password on your console port and ensure that the router prompts for a password anytime someone connects to that port?

```
ROUTERA(config)#line con 0
ROUTERA(config-line)#password consolepass
ROUTERA(config-line)#login
```

492. You wish to create a username `Rebecca` with a password of `mypass`, what command would  you use?

D. `router(config)#username rebecca password mypass`

493. You wish to configure a password for priv exec mode but have the password encrypted in the configuration file. What command would you use?

A. `enable secret P@ssw0rd`

494. What is the difference between the login command and the login local command?

D. Login prompts for a password, whereas login local prompts for a username and password

495. You are configuring banners on your Cisco router. Which of the following banners is displayed last?

MOTD -> login -> exec

A. The exec banner

496. Which of the following commands configures a proper message of the day banner?

```
R1(config)#banner motd #
R1(config)#This device is for authorized idividuals only.
#
```

497. Which of the following banner type displays before the user is asked to log in, but after the MOTD banner?

D. login

enter the router -> MOTD, and then login banner -> user logon and enters exec mode -> exec banner

498. Your manager would like to ensure that remote access to the router is only allowed using a secure communication protocol. Which command would you use?

B. `router(config-line)#trasnport input ssh`

499. You are responsible for configuring Telnet access to the router. Which of the following commands would you use to ensure a username and password are required for Telnet access?

```
Router(config)#username bob password pass
Rotuer(config)#line vty 0 4
Router(config-line)#login local
```

500. You want to remotely manage your Cisco devices. What protocol should you use?

B. SSH

501. You are having trouble telnetting into a router from your client system. You can successfully ping the IP address of the router. What may be the problem?

B. There is no Telnet password

A part of the default security of a Cisco device is if you do not have proper passwords set, you are unable to remotely connect to the device

502. Which of the following commands are used to configure the router to only ­support SSH for remote management?

```
R1(config)#crypto key generate rsa
R1(config)#line vty 0 4
R1(config-line)#login local
R1(config-line)#transport input ssh
```

503. What command would you use to configure the Telnet ports to support both SSH and Telnet as protocols?

C. `transport input ssh telnet`

504. What type of wireless network involves two wireless clients connecting to one another?

D. Ad-hoc

505. Which of the following is considered the name of the wireless network?

B. SSID

506. Which agency is responsible for defining the wireless standards?

D. IEEE

507. In the figure shown below, what type of wireless network exists?

![[Pasted image 20260103144335.png]]

A. ESS

An ESS wireless network covers a large area by having multiple access points using the same SSID and different channels.

508. Which of the following devices could cause interference with your wireless network? (Select two.)

B. Cordless phone
C. Microwave

509. You are installing a wireless network for a small office environment. The manager is worried that someone in the parking lot can connect to the wireless network. What could you do to help minimize the chances of that happening?

D. Change the power levels on the access point

510. In the figure shown below what type of wireless network exists?

![[Pasted image 20260103144452.png]]

C. BSS (Basic Service Set)

BSS is the nam uesd for a wireless network involving clients connecting to an access point to gain network access.

511. You are experiencing intermittent problems with your wireless network and you suspect it is interference from other wireless devices. What can you do to solve the problem?

D. Change the channel on the wireless access point

512. What type of wireless network involves the wireless clients connecting to a wireless access point to access the network?

B. Infrastructure

Wireless networks are running in infrastructure mode when connecting the a wireless access point to gain wireless network access.
You can also configure wireless networks in ad-hoc mode with one wireless device connecting to another wireless device.

513. Which of the following facts are true of the WLAN diagram displayed below? (Select two.)

![[Pasted image 20260103144825.png]]

B. The diagram represents an ESS network with both access points using the same SSID
D. Each access point is using a different channel

An ESS wireless network is one that has multiple access points using the same SSID, with each access point using a different channel.
An ESS is designed to allow users to roam throughout the building and still have wireless access.

514. Which of the following are requirements for an ESS network? (Select three.)

B. Use multiple access points with the same SSIDs.
C. Each access point uses a different frequency range
E. Access point must overlap by 10 percent or greater

515. Which agency is responsible for regulating the use of wireless and wireless frequencies?

D. FCC

The Federal Communications Commision (FCC) is responsible for regulating the use of wireless and wireless frequencies. It is the governing body that legalized publid use of the 900 MHz 2.4 GHz, and 5 GHz frequencies.

516. Which of the following is another term for an ad-hoc wireless network?

B. IBSS

An independent basic service set (IBSS) is another term for an ad-hoc wireless network.

517. You are the administrator for a small office and have configured four access points to create the WLAN. What type of WLAN is created?

B. ESS

four access points -> ESS
single access poings -> BSS

518. Which type of wireless network uses a single wireless access point with an SSID assigned?

A. BSS

519. You have manufactured your own wireless network card and would like it tested and certified for Wi-Fi compliance. What organization would do this?

A. Wi-Fi Alliance

The Wi-Fi alliance would test and certify components for Wi-Fi compliance.

520. You have installed a single access point to create a WLAN for a small office. What type of WLAN did you create?

A. BSS

521. Which wireless LAN design ensures that a roaming user does not loose connectivity when moving from one wireless access point to another?

D. Ensure that the area of coverage by both access points by at least 10%.

522. Which agency is responsible for ensuring compatibility with wireless networking components?

A. Wi-Fi Alliance

523. Which three wireless channels do not overlap with one another?

B. 1, 6, 11

524. Which type of wireless network supports roaming users by having multiple access points share the same SSID?

B. ESS (Extended Service Set)

525. Which of the following should be done with the SSID on a wireless network in order to follow good security practices? (Select two.)

A. Change the SSID
D. Disable SSID broadcasting

526. Which of the following represent good security practices for wireless networks? (Select three.)

B. Enable WPA2
D. Enable MAC filtering
F. Disable SSID braodcasting

527. Which of the following wireless encryption protocols is an older protocol using RC4?

C. WEP

528. Which of the following can be used to restrict which devices can connect to your wireless network?

B. MAC Filtering

529. You are installing a wireless access point for a small office. At a minimum, which wireless parameter must be configured in order for clients to connect to the wireless network?

C. SSID

530. You are configuring a wireless network for a customer. What reason would you give the customer as to why WPA should be used instead of WEP for the encryption?

A. WPA rotates the key with each packet

*Wi-Fi Protected Access (WPA)* was designed to improve upon wireless security and fix some of the flaws in WEP.
WPA uses a 128-bit key and the Temporal Key Integrity Protocol (TKIP), which is used to change the encryption keys for every packet that is sent. This will make it far more difficult for hackers to crack the key.
WPA uses RC4 as the symmetric encryption algorithm, which is why WPA is sometimes referred to as TKIP-RC4.

WAP has a number of other improvements over WEP; for example, it has improved integrity checking, and it supports authentication using the Extensible Authentication Protocol (EAP), a very secure authentication protocol that supports a number of authentication methods such as Kerberos, token cards, certificates, and smartcards.

* Improved integrity checking
* supports authentication (using EAP)
* 128-bit key and Temporal key Integrity protocol (TKIP)
	* change the encryption keys for every packet that is sent
* Uses RC4 as encryption algorithm

531. Which WPA2 mode involves using a pre-shared key as the method of security?

* **WPA Personal**
	* With WPA Personal, aka WPA-PSK (WPA preshared key), you can configure the AP with a starting key value, known as the preshared key, which is then used to encrypt the traffic.
	* This mode is used most by home users and small businesses.
* **WPA Enterprise**
	* WPA-802.1X, is a WPA implemenation that uses a central authentication server such as a RADIUS server for authentication and auditing features.
	* WAP Enterprise is used by larger organizations so that they can use their existing authentiation server to control who has access to the wireless network and to log network access.
* **Open**
	* An open wireless network does not require any password to connect and does not use any form of encryption to keep the wireless data secret from prying eyes.
	* Naturally, it is not recommend to leave your wireless network open or to connect your client system to an open network that you are not familiar with.

A. Personal

532. Which wireless encryption protocol uses the TKIP protocol?

TKIP-RC4 (WPA)

C. WPA

533. Which WPA mode uses a RADIUS server for authentication?

B. Enterprise

534. Which wireless encryption protocol uses AES for encryption?

B. WPA2

WPA -> RC4
WPA2 -> AES
WPA3 -> GCMP256

535. What method of encryption does WPA2 use?

C. AES-CCMP

536. Which wireless standard delivers data at 11 Mbps within the 2.4 GHz frequency range?

**Wireless standards**

| Standard | Freq      | Transfer Rate  | Compatibility |
| -------- | --------- | -------------- | ------------- |
| 802.11a  | 5 GHz     | 54 Mbps        | 802.11a       |
| 802.11b  | 2.4 GHz   | 11 Mbps        | 802.11g/n     |
| 802.11g  | 2.4 GHz   | 54 Mbps        | 802.11b/n     |
| 802.11n  | 5/2.4 GHz | up to 600 Mbps | 802.11a/b/g   |
| 802.11ac | 5 GHz     | 1 Gbps         | 802.11a/b/g/n |

11Mbps, 2.4 GHz -> 802.11b

B. 802.11b

537. Which wireless standard is capable of transferring data over 150 Mbps and runs at both the 2.4 GHz and 5 GHz frequencies?

D. 802.11n

538. Which wireless standard runs at 54 Mbps and is compatible with 802.11b?

compatable with 802.11b -> ac, n, g
runs at 54 Mbps -> g

C. 802.11g

539. Which wireless network standard runs at 54 Mbps and uses the 5 GHz frequency?

A. 802.11a

540. Which IEEE standard defines wireless networking?

D. 802.11

541. Which wireless standard is compatible with 802.11a?

802.11a, 802.11n, 802.11ac

D. 802.11n

542. What is the bandwidth of a T1 WAN link?

A. 1544 Mbps

543. Which of the following represents a dedicated WAN link that you pay a monthly subscription fee for?

A. Leased time

A leased line is a dedicated WAN link that you pay a monthly fee for.

544. Which WAN technology needs to first establish a connection between two points before sending the data and has all the data travel the same pathway?

C. Circuit switched

545. What is the bandwidth of a BRI subscription?

D. 128 Kbps

A BRI ISDN subscription uses two 64 Kbps data channels, giving you 128 Kbps

546. What is the bandwidth of a T3 link?

T1 = 1544 Mbps
T3 = 44736 Mbps

B. 44736Mbps

547. Which of the following technologies are dedicated leased lines?

A. T1

A T1 or T3 link is considered a dedicated leased line because the connection is always available and dedicated to your company

548. What is the bandwidth of a PRI subscription?

A PRI ISDN subscription has a bandwidth of 1544 Mbps

A. 1544 Mbps

549. Which type of WAN technology dedicates the bandwidth to the communication until the connection is terminated?

C. Circuit switch

550. Which of the following is considered a not-so-permanent WAN link?

C. Circuit switched

551. Which of the following is a circuit switching technology?

C. X.25; frame relay

X.25 and frame relay are examples of packet switching technologies where each packet can take a different route.

circuit switched: Analog dialup and ISDN
Cell switched: ATM
Packet switched: X.25, frame relay

552. Which of the following WAN technologies are packet switching technologies? (Select all that apply.)

C. X.25
D. Frame relay

### Wide Area Networks

Wide area networks (WANs) are used to connect mulitple LANs together.

For example, suppose you have an office in Boston (a LAN) and an office in Vancouver (another LAN); a WAN is used to connecte these two LANs together so that users can communicate with other used and systems on both LANs.

Typically, WANs are used when the LANs that must be connected are separated by a large distance. Whereas a corporation provides its own infrastructure for a LAN, WANs are leased from carrier networks, such as telephone companies and Internet service providers (ISPs).

Four basic types of connections, or circuits, are used in WAN services: circuit-switched, cell-switched, packet-switched, and dedicated connections.

A wide array of WAN services available to connect different office locations, including analog dialup, Asynchronus Transfer Mode (ATM), dedicated circuits, cable, digital subscriber line (DSL), Frame Relay, Integrated Service Digital Network (ISDN), and X.25.

Analog dialup and ISDN are examples of circuit-switched services; ATM is an example of a cell-swiched service; and Frame Relay and X.25 are examples of packet-swiched services.

*Circuit-swiched services* provide a temporary connection across a phone circuit.
In networking, these services are typically used for backup of primary circuits and for temporary bosts of bandwidth.
More commonly today, celluar wireless services (3G and 4G) are used for dial-on-demand applications or backup connections.

A *dedicated circuit* is permanent connection between two sites in which the bandwidth is dedicated to that company's use.
These circuits are common when a variety of services, such as a voice, video, and data, must traverse the connection and you want guaranteed bandwidth available.

*Cell-switched* services can provide the same features that dedicated circuits offer, such as having a dedicated link, but one of its advantages over dedicated circuits is that it enables a single device to connect to multiple devices on the same interface.
The downside of these services is that they are not available at all locations, they are difficult to set up and troubleshoot, and the equipment is expensive when compared to equipment used for dedicated circuits.

*Packet-switched* services are smiliar to cell-swiched services, except cell-swiched services switch *fixed-length* packets called cells and packet-switched services switch *variable-length* packets.
This feature makes packet-swiched services better suited for data services, but they can nonetheless provide some of the QoS features that cell-swiched services provide.

Two other WAN services that are very popular technologies for high-speed Internet connections are digital subscriber-lines (DSLs) and cable.

DSL provides speeds up to a few megabits per second (Mbps) and costs much less than a typicall WAN circuit from the carrier.
It supports both voice and video and doesn't require a dialup connection (it's always enabled).
The main disadvantage of DSL is that coverage is limited to about 18,000 feet, and the service is not available in all areas.

Cable supports higher data rates than DSL, but like DSK, it provides a full-time connection. However, cable has two major drawbacks: it is shared service and functions in a logical bus topology much like Ethernet, so the more customers in an area that connect via cable, the less bandwidth each customer has; also, because many people are sharing the medium, it is more susceptible to security risks such as eavesdropping on other subscribers' traffic.

553. Which device on a point-to-point link needs to have the clock rate set?

C. The DCE device

554. Which of the following ports would you use to connect to a T1 connection?

C. Serial 0/0

To connect a T1 line to your router, you could connect an external CSU/DSU to the serial port on the router.

555. What is the name of the DTE/DCE cable that is used to connect two routers together via their serial ports?

C. Back-to-back serial cable

A back-to-back serial cable has a DCE end of the cable and DTE end, and is used to connect two routers together by their serial ports to emulate a WAN link.

556. You are troubleshooting your WAN connection. What layers of the OSI model are you trouble-shooting? (Select two.)

B. Physical
E. Data link

When troubleshooting WAN connection issues, you are troubleshooting layer 1 (Physical) and layer 2 (data link).

557. Which of the following devices would typically connect the serial port on your router to a T1 line?

A. External CSU/DSU

558. Which of the following statements is true of a serial interface?

A. The clock rate must be set on the DCE device.

559. What is the default encapsulation protocol on a serial link for a Cisco router?

D. HDLC

560. You are connecting your Cisco router to a non-Cisco router over the serial link. Which of the following commands will you use?

D. `encapsulation ppp`

561. You have disabled the serial 0/0 interface with the `no shutdown` command. How will the show interfaces command display the status of the interface?

D. `Serial 0/0 is up, line protocol is down`

???? no shutdown command 로 disable 을 어케함 말이안되네

562. You have configured a test lab and you want to allow communication between two routers using PPP. Review the following code and identify any reasons why the two routers cannot communicate over the serial link.

```
MTL-R1(config)#interface s0/0
MTL-R1(config-if)#ip address 24.0.0.1 255.0.0.0
```

A. The encapsulation protocol needs to be set (default is hdlc)

563. You are configuring the DCE end of a point-to-point serial link. What command would you type that is not typed on the DTE device?

B. `clock rate 64000`

564. Looking at the output below, what is the protocol configured on the serial port?

![[Pasted image 20260103210936.png]]

C. HDLC

565. Looking at the output shown below, what type of device is router R1?

![[Pasted image 20260103211007.png]]

D. DCE

566. Which of the following encapsulation protocols will allow communication with non-Cisco devices?

A. PPP

567. Which packet switched technology uses a fixed length packet size known as a cell?

Packet switched: X.25, frame relay

A. ATM

ATM networks have a fixed length packet size, known as a cell, which is 53 bytes.

568. If you want to load a Cisco proprietary encapsulation protocol over the serial link, what command do you use?

B. `encapsulation hdlc`

569. Your router is unable to communicate over the WAN link that is connected to serial 0/0. What commands would you use to verify the status of the port? (Select two.)

B. `show interfaces`
E. `show ip interface brief`

570. What are the requirements to configure PPP authentication between NY-R1 and TOR-R1? (Choose three.)

B. Set the hostname on each router to a unique name
D. Create a username on each router that matches the hostname of the other router
E. Enable PPP authentication on both routers and specify either PAP or CHAP as the authentication protocol

To configure PPP authentication, you set hostname on each router to a unique name and then create a username on each router that is the hostname of the other router. (Set the passwords to the same value.) Then you enable PPP authentication and specify the authentication protocol of either CHAP or PAP.

571. What command would you use to verify if a serial port is a DCE or DTE device?

D. `show controllers serial 0/0`

572. Your manager has asked you about the HDLC protocol on Cisco devices. Which of the following statements are true? (Select two.)

B. It is the default encapsulation protoocl for serial links on Cisco routers
E. It is a Cisco proprietary implementation

573. You have a branch office connected to your head office using a WAN link over the serial ports. You have recently upgraded the router and are unable to communicate across the WAN link. Using the output below, what should you do to get the link up and running?

![[Pasted image 20260103211623.png]]

B. Change the encapsulation protocol

574. What encapsulation protocol would you use on the serial port in order to support authentication?

D. PPP


575. The router administrator is troubleshooting the WAN link and wondering what the meaning of line protocol up means. (Select two.)

A. The link is up
C. Communication over the layer-2 protocol is working.

576. You are the network administrator who manages a router in the Toronto office named TOR-R1 and the New York office name NY-R1. You would like to configure PPP as the encapsulation protocol over the WAN link with PPP authentication between the two routers. Which of the following commands represent the configuration on the Toronto router?

```
TOR-R1(config)#username NY-R1 password mypass
TOR-R1(config)#interface s0/0
TOR-R1(config-if)#encapsulation ppp
TOR-R1(config-if)#ppp authentication chap
```

577. Which authentication protocol with PPP is considered the most secure?

D. CHAP

CHAP is the more secure PPP authentication protocol because the password is not passed in clear text across the network.

578. Which of the following authentication protocols supported by PPP should be used?

D. CHAP

PAP sends username and password in clear text

579. Which of the following commands would you use on the serial interface to configure CHAP authentication?

A. `ppp authentication chap`

580. Which of the following statements are true when discussing routers and switches? (Select two.)

B. Routers increase the number of broadcast domains
C. Switches increase the number of collision domains

581. What is the effect of the command copy tftp flash?

A. Initiate a copy of a file from a TFTP server to flash memory

582. Which OSI model header contains the source and destination fields of the packet?

C. Network

The network layer header contains the source and destination address information for the packet.

583. You have connected the switches to the routers using crossover cables, and the client systems to the straight-through cables. You have configured Network A with the 192.168.3.0/24 IP range, and Network B with the 192.168.4.0/24 range. The systems cannot communicate off the network. What should you do?

Switch - router = straight-through
switch - client = straight-through

D. Change the crossover to straight through

584. Which of the following devices are considered layer-2 devices? (Select two.)

A. Bridge
C. Switch

Hub = a lot of repeater (layer 1)

585. Which of the following scenarios require a crossover cable? (Select three.)

A. Computer to router
B. Switch to switch
D. Computer to computer

586. Which layer of the OSI model handles logical addressing and routing?

layer 3 (Network layer)

C. Network

587. What organization is responsible for defining standards for Ethernet networking?

D. IEEE

588. Which of the following is a layer-2 device found on a network? (Select two.)

C. Switch
E. Bridge

589. Refer to the figure below. Each cable used has been assigned a letter and the cables that have been used are listed in your choices. Select the letter choices which specify the locations in which the correct cables have been used. (Select two.)

![[Pasted image 20260103225809.png]]

A = Straight
B = Striaght
C = Console
D = Crossover
E = Straight
F = Straight

C. Console
E. Straight

590. You want to set a session timeout on your router for 15 minutes and 30 seconds. Which command will do what you are looking for?

D. `exec-timeout 15 30`

591. What layer of the OSI model does CDP run at?

CDP = Cisco Discovery Protocol

D. Data link (layer 2)

592. MAC addresses are composed of two main components. What are they? (Select two.)

A. OUI
D. Vendor assigned values

A 48-bit MAC address is composed of two main pieces, a 24-bit Organizational Unique Identifier (OUI) assigned by IEEE, and a 24-bit vendor assigned end statio address, which is assigned by the network card manufacturer.

593. Which of the IEEE standards defines Gigabit Ethernet over copper wires?

802.11 => wireless

A. 802.3ab

594. Which TCP/IP protocol is responsible for reliable delivery?

B. TCP

595. Which of the following IPv6 addresses is the equivalent to the 127.0.0.1 IPv4 address? (Select two.)

A. ::1
D. 0000:0000 .....

IPv6 loopback is ::1

596. Which of the following application layer protocols use TCP as the transport layer protocol? (Select two.)


B. SMTP
D. FTP

**TCP**

| Port | Service     | Desc                                            |
| ---- | ----------- | ----------------------------------------------- |
| 20   | FTP Data    | Used by FTP send data to a client               |
| 21   | FTP Control | Used by FTP commands sent to the server         |
| 22   | SSH         | Secure Shell                                    |
| 23   | Telent      |                                                 |
| 25   | SMTP        | send e-mail                                     |
| 53   | DNS         | Domain Name Service                             |
| 80   | HTTP        |                                                 |
| 110  | POP3        | read e-mail                                     |
| 143  | IMAP        | read e-mail                                     |
| 443  | HTTPS       | Secure HTTP                                     |
| 3389 | RDP         | Remote Desktop Protocol (manage windows system) |

**UDP**

| Port        | Service | Desc                               |
| ----------- | ------- | ---------------------------------- |
| 53          | DNS     | DNS queries                        |
| 67 and 68   | DHCP    | 67: server, 68: client             |
| 69          | TFTP    | Trivial File Transfer Protocol     |
| 137 and 138 | NetBIOS |                                    |
| 161         | SNMP    | Simple Network Management Protocol |

597. Which of the following is a valid IPv6 address?

B. fe80:d351:3f16:dc41:ed36:317e:410e:3f28

598. Which of the following are considered private IP addresses? (Select two.)

**ipv4 private address range**

* 10.0.0.0 - 10.255.255.255
* 172.16.0.0 - 172.31.255.255
* 192.168.0.0 - 192.168.255.255

**IPv6 Private address**

| Private | FE80::/10 | Like IPv4, IPv6 originally supported private addressing, which is used by devices that don't need to access a public network. The first two digits are FE, and the third digit can range from 8 to F. |
| ------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |


* **Site-local/unique-local**
	* A site-local address (aka unique-local address) is considered a private IP address, similar to the 10.0.0.0/8 private IP with IPv4. This address is used for local communication within a site.
	* A site-local address always starts with FEC:: through FFF::.
* **Link-local**
	* A link-local address is similar to an APIPA address in IPv4 and is used for communication on the network link.
	* A link-local address starts with FE80::/10 (FE8:: through FEB::).

B. 172.25.56.10
E. 10.45.3.20

127.0.0.1 = loopback

599. You are designing an IP scheme for a network that will contain up to 8 subnets with no more than 26 hosts on each. Which of the following subnet masks will be used?

A. 255.255.255.224

600. Which of the following statements are true about the UDP protocol? (Select two.)

A. All packets are sent individually
D. Packet delivery is not guaranteed

601. How many bits are in an IPv6 address?

C. 128

IPv6 = 128 bit
IPv4 = 32 bit

602. Given the network shown in the figure below, which network ID would be the smallest that can be used for segment A?

![[Pasted image 20260104142124.png]]

```
2^masked_bits = number of subnets
2^host_bits - 2 = number of hosts supported
```

Segment A has 115 hosts, so subnet should support at least 115 hosts: `2^host_bits - 2 >= 115`. Because 2^6 = 64, 2^7 = 128 then host bits are 8 bits. (11111111.11111111.11111111.10000000, 255.255.255.128, /25)

A. 192.168.15.0/25

603. When working with the TCP/IP protocol suite, what protocol makes use of windowing?

### Windowing

*Windowing* is much more sophisticated method of flow control than using ready/not ready signals.

With windowing, a window size is defined that specifies how much data (segments) can be sent before the source has to wait for an acknowledgement (ACK) from the destination.
Once the ACK is received, the source can send the next batch of data (up to maximum defined in the window size).

Windowing accomplishes two things: First, flow control is enforced, based on the window size.
In many protocol implementations, the window size is dynamically negotiated up front and can be renegotiated during the lifetime of the connection.
This ensures that the most optimal window size is used to send data without having the destination drop anything.
Second, through the windowing process, the destination tells the source what was received.
This indicates to the soruce whether any data was lost along the way to the destination and enables the source to resend any missing information.
This provides reliability for a connection as well as better efficiency than ready/not ready protocols, such as TCP/IP's TCP, use windowing to implement flow control.

The window size chosen for a connection impacts its efficiency and throughput in defining how many segements (or bytes) can be sent before the source has to wait for an ACK.

B. TCP

604. Given the network shown in the figure below, which network ID would be the smallest that can be used for segment C?

![[Pasted image 20260104142715.png]]

2^9 = 512, 2^10 = 1024 host bit = 10 /22

B. 172.18.0.0/22

605. What are the three steps and order of the TCP three-way handshake process?

### Three-way handshake

1. **SYN**
	* The sending system sends a SYN message to the receiving system.
	* Each packet sent is assigned a unique sequence number.
	* The SYN message contains *initial sequence number (ISN)*, which is the first sequence number to be used.
2. **SYN/ACK**
	* This phase acknowledges the first message, but at the same time indicates its initial sequence number.
3. **ACK**
	* The final phase is the acknowledgement message that acknowledges that the packet sent in the second phase has been received.

C. SYN, SYN/ACK, ACK

606. When working with a mask that is 22 bits long, which subnet mask is an alternate representation of 22 bits?

11111111.11111111.11111100.00000000
255.255.252.0

C. 255.255.252.0

607. Given the network shown in the figure below, which network ID would be the smallest that can be used for segment B?

![[Pasted image 20260104143039.png]]

2^4 = 16, 2^5 = 32

11111111.11111111.11111111.11100000
255.255.255.224
/27


?????

608. Which of the following is the broadcast address of 216.83.25.87/26?

216.x.x.x is class C address, so deafult subnet mask is /24. 216.83.25.87/26 is subnetted by 4 subnets (`2^masked_bits = number of subnets`). Range is 01000000 (64).

| Network ID    | BRD           |
| ------------- | ------------- |
| 216.83.25.0   | 216.83.25.63  |
| 216.83.25.64  | 216.83.25.127 |
| 216.83.25.128 | 216.83.25.191 |
| 216.83.25.192 | 216.83.25.255 |

216.83.25.87/26 is in 216.83.25.64/26 network and this subnets' broadcast address is 216.83.25.127.

D. 216.83.25.127

609. Which of the following is the third valid address of the network containing 220.19.36.112/27?



| Network ID    |
| ------------- |
| 220.19.36.0   |
| 220.19.36.32  |
| 220.19.36.64  |
| 220.19.36.96  |
| 220.19.36.128 |

B. 220.19.36.99

610. Your manager has asked about the new IPv6 address format. He asks which of the following addresses is a link-local address. What is your answer?

### Link local vs. Site local

* **Site-local/unique-local**
	* A site-local address (aka unique-local address) is considered a private IP address, similar to the 10.0.0.0/8 private IP with IPv4. This address is used for local communication within a site.
	* A site-local address always starts with FEC:: through FFF::.
* **Link-local**
	* A link-local address is similar to an APIPA address in IPv4 and is used for communication on the network link.
	* A link-local address starts with FE80::/10 (FE8:: through FEB::).

::1 is loopback address
200~~~ is global unicast


C. fe80:d351:3f16:dc41:ed36:317e:410e:3f28

611. The first 24 bits of the MAC address is known as the `__________`.

D. OUI

612. Which of the following network IDs should be used on a WAN link with just two routers using a single IP address each?

It needs two host (2^host_bit - 2 = 2, so 2 host bits) (11111111.11111111.11111111.11111100, 255.255.255.253, /30)

A. 195.56.78.0/30

613. Which of the following IPv6 communication methods is best known as “1-to-nearest”?

D. Anycast

### IPv6 Communictaion Methods

* **Anycast**
	* This is very different from an IPv4 broadcast.
	* An anycast address is used for one device to talk to the nearest device that has that address assigned to its interface (one-to-the-nearest address), where many interface can share the same address. These addresses are taken from the unicast address space but can represent multiple devices, such as multiple default gateways.
	* For example, using an anycast address as a default gateway address on your routers, user devices have to know of only one address, and you don't need to configure a protocol such as Hot Standby Router Protocol (HSRP) or Virtual Router Redundancy Protocol (VRRP).
* **Multicast**
	* This is similar to a multicast in IPv4
* **Unicast**
	* This represents a single unique address used for direct communication.

614. You need to back up your router configuration to a TFTP server. Which of the following commands would you use?

router configuration (running-config or startup-config) to tftp server
`copy running-config tftp`

B. `R1#copy runnig-config tftp`

615. You are troubleshooting a routing problem and need to view your routing table. What command would you use?

C. `show ip route`

616. You have shelled out of your telnet session and wish to view the telnet sessions that are currently running in the background. What command would you use?

C. `R1>show sessions`

617. You are the administrator for router R1 shown in the figure below. You need to add a static route for the 13.0.0.0 network. What command would you use?

![[Pasted image 20260104160625.png]]

```
R1(config)#ip route 13.0.0.0 255.0.0.0 12.0.0.2
```

618. Which of the following configures a console password on your Cisco device?

```
Router#config t
Router(config)#line con 0
Router(config-line)#password conpass
Router(config-line)#login
```

619. You wish to delete a route from the routing table, what command would you use?

C. `no ip route`

620. You have routing tables specifying gateways for the networks of 10.0.0.0/8, 10.10.0.0/16, 10.10.10.0/24, and 10.10.10.10/32. You have attempted ping a device with an address of 10.10.10.11. Which entry in the routing table will direct your traffic to a gateway or router?

C. 10.10.10.0/24

When choosing a gateway, the router will always choose the entry in the routing table which most closely matches the destination address.

621. Which devices added to your network will reduce the size of a broadcast domain?

B. Router

622. You need to configure your Fast Ethernet port 0/0 for the IP address of 14.0.0.1 and ensure that the card is enabled. What commands would you use?

```
R1#config term
R1(config)#interface F0/0
R1(config-if)#ip address 14.0.0.1 255.0.0.0
R1(config-if)#no shutdown
```


623. You wish to configure your serial 0/0 interface as the DCE end of a point-to-point link. Which of the follow represents the commands you would use?

```
### verify DCE or DTE
R1#show controllers s0/0

### DCE
R1#config term
R1(config)#interface S0/0
R1(config-if)#ip address 14.0.0.1 255.0.0.0
R1(config-if)#encapsulation ppp
R1(config-if)#clock rate 64000
R1(config-if)#no shutdown
```

624. You need to ensure that CDP is enabled on your Cisco device. What command would you type?

D. `R1(config)#cdp run`

625. You have configured RIP routing on your routers. You have configured a static route to the same destination that your router learns of through RIP. Which entry is placed in the routing table?

B. Static route

**Administrative Distance**

| Value | Route Type                                                       |
| ----- | ---------------------------------------------------------------- |
| 0     | Connected interface route                                        |
| 1     | Static route                                                     |
| 90    | Internal EIGRP router (within the same AS)                       |
| 110   | OSPF route                                                       |
| 120   | RIPv1 and v2 route                                               |
| 170   | External EIGRP (from another AS)                                 |
| 255   | Unknown route (considered an invalid route and will not be used) |

626. You wish to configure a username bob and ensure telnet requires a username and password before allowing access to the device. Which of the following configuration allows this?

```
### add user
R1#config term
R1(config)#username bob password P@ssw0rd

### vty configuration
R1(config)#line vty 0 4
R1(config-line)#login local
```

627. You have created access list I to include all IP address that can use NAT. How would you configure NAT on the router to have all internal IP assress use the public IP assigned to the serial 0/0 interface?

D. `ip nat inside source list 1 interface serial 0/0 overload`

To configure NAT using the access list, you use the `ip nat inside source list <#>` command and continue by specifying which interface you wish to overload.

628. You wish to recover a forgotten password on your router. What value would you set the configuration register to?

D. 2142

#### Configuration registers

if you wanted to boot the device and have it load the IOS and the statup configuration, you would set the configuration register value to 0x2012 (the default setting).
But if you wanted to restart the Cisco device and have it load the IOS, but not the startup configuration, you would set the configuration register to 0x2142 (which enables bit 13, bit 8, bit 6 and bit 1)

| Bit Number | Explanation                                          |
| ---------- | ---------------------------------------------------- |
| 0-3        | Known as the boot fields discussed in next section   |
| 6          | Ignore the contents of NVRAM                         |
| 7          | Disable the display of boot messages                 |
| 8          | Break is disabled                                    |
| 10         | IP broadcast with all zeros                          |
| 13         | Loads the default ROM software if network boot fails |

629. Which of the following does OSPF use to calculate the cost of a link?

D. Bandwidth

Distance vector protocol (RIP, IGRP) uses hop count as metric.
Link state protocol (OSPF, IS-IS)

630. You are configuring a serial port on the router. What command would set the clock rate to 32 Kb/s?

C. clock rate 32000

631. You are looking to implement a routing protocol with the following features:

✓ Scalable
✓ Support for VLSM
✓ Vendor compatibility
✓ Low overhead

A. OSPF

EIGRP and IGRP are propriety Cisco protocols, RIP does not support VLSM

RIPv1 works only with classful addresses because it dosen't send subnet mask information with the routing table.
RIPv2 is an update to RIPv1 and dose support classless addressing and variable-length subnet masks, because it sends the subnet mask information with the routing table.

RIPv1 -> classful
RIPv2 -> VLSM


632. There are many reasons for network congestion that can affect data movement and delivery. Which of the following are leading causes of congestion? (Select three.)

B. An execssive number of hosts in a broadcast domain
D. Broadcast storm
E. Half-duplex interfaces

Half-duplex interfaces only transfer data in one direction at a time
broadcast storms generates excessive traffic
excessive number of hosts generates traffic that needs to be processed by other devices in the broadcast domain.

633. Which of the following are link-state routing protocols? (Select two.)

C. OSPF
D. IS-IS

RIP, IGRP is Distance vector protocol

634. You have configured a switch port for trunk mode. What type of device is typically connected to this type of port?

D. switch

Trunk ports are typically used to connect a switch to another switch; most other devices will typically use access ports. A trunk mode will pass traffic for all VLANs.

635. What are three metrics that are used by routing protocols to determine an optimal network path? (Select three.)

A. hop count
B. bandwidth
E. delay

636. Which dynamic routing protocols support both route summarization and VLSM? (Select three.)

RIPv1 doesn't support VLSM
IGRP doesn't support VLSM

VTP = VLAN trunking protocol

A. EIGRP
C. RIPv2
D. OSPF

637. You have configured your router for NAT as per the figure below, but most of your network users are not able to connect to external resources. What command will fix the problem? (Select two.)

![[Pasted image 20260107142614.png]]

A. `ip nat inside source list 1 interface FastEthernet 0/0 overload`
D. `ip nat pool no-overload 192.168.1.2 192.168.1.63 prefix 24`

The difference between these two options is based on the final result that you desired.
If your goal was to support a PAT configuration, the answer A enables overloading, which would allow your configuration to function, allowing more than one user out; but if you were attempting to perform NAT, then choice D defines a pool of outside addresses and assigns it to be used for NAT.

638. What is the purpose of the overload option when working with NAT?

C. It changes the function of NAT to PAT.

639. What is the Administrative Distance of a static route?

B. 1

640. Which of these dynamic routing protocols do not support VLSM? (Select two.)

A. RIPv1
E. IGRP


641. Which of the following commands would configure your router as a DHCP server, give out the address 192.168.1.1 as the default gateway value to clients, and lease the address to clients for 7 days?

#### DHCP Server Configuration

```
IOS(config)# service dhcp
IOS(config)# ip dhcp pool POOL_NAME
IOS(dhcp-config)# network NETWORK_NUMBER [mask | /prefix-length]
IOS(dhcp-config)# domain-name DOMAIN
IOS(dhcp-config)# dns-server ADDRESS [ADDR2, ..... address 8]
IOS(dhcp-config)# default-router ADDR
IOS(dhcp-config)# lease {days [hours] [minutes] | infinite}
IOS(dhcp-config)# exit
IOS(config)# ip dhcp excluded-address low-address [high-address]
```

In this example,

* **`service dhcp`**
	* Enables the DHCP server and/or relay features on your IOS device.
	* Without this command, the IOS device will not act as a server or relay agent.
	* As of IOS 12.2, this command is enabled by default.
* **`ip dhcp pool`**
	* Creates a name for the DHCP server address pool and places you in DHCP pool subcommand mode.
* **`network`**
	* Specifies the subnet network number and mask of the DHCP address pool.
	* Also *prefix-length* specifies the number of bits that make up the address prefix.
	* The prifix is an alternative way of specifing the network mask of the client.
	* The prefix length must be preceded by a forward slash (/).
* **`domain-name`**
	* Specifies the domain name to be assigned to the client.
* **`dns-server`**
	* Specifies the IP address of a DNS server that is available to a DHCP client.
	* One IP address is required; however, you can specify up to eight IP addresses in one command line.
* **`default router`**
	* Specifies the IP address of the default gateway to be assigned to the DHCP client
* **`lease`**
	* Specifies the duration of the lease
	* IP addresses are leased to hosts, typically on a temporary basis.
	* When a client's address lease time reaches half the life of the lease, it will attempt to renew the existing addressing information it obtained previously. If unsuccessful in its renewal attempt, the device will continue to attempt renewal at periodic intervals.
	* The default lease duration is one day, but other parameters can be configured for the pool, such as an TFTP server address for VoIP phones.
	* You can set the lease to infinite, which would mean the lease does not expire.
* **`ip dhcp excluded-address`**
	* Specifies the IP address that the DHCP server should not assign to DHCP clients based on the network numbers you've defined for the DHCP pool or pools.
	* These addresses are already used in the subnet, such as static address assigned to the switches and routing device.
	* If you are defining network numbers for your pool, make sure you exclude the network number and the directed broadcast address from the pool.
	* Notice this command is not part of the pool configuration, but is a Global Configuration command.

```
### enable dhcp
R1(config)#service dhcp
### create dhcp pool
R1(config)#dhcp pool MY_Network
### Specifies the network number
R1(config-dhcp)#network 192.168.1.0 255.255.255.0
### Speicifies default gateway
R1(config-dhcp)#default router 192.168.1.1
### set duration of the lease
R1(config-dhcp)#lease 7 0 0
```

```
R1(config)#ip dhcp pool NY_Network
R1(config-dhcp)#network 192.168.1.0 255.255.255.0
R1(config-dhcp)#default router 192.168.1.1
R1(config-dhcp)#lease 7 0 0
```

642. You are the network administrator for a network that is already running OSPF. You configure OSPF on your router with the following commands, but your router is not sharing routing information with other OSPF routers. What is wrong?

```
Enable
Config term
Router ospf 1
Network 12.0.0.0 255.0.0.0 area 0
```

B. You have configured the wrong wildcard mask (it should be 0.255.255.255)

643. Which of the following is one of the things you need to do to configure a router to route between VLANs using only a single connection to the switch?

D. configure sub-interfaces on the router

644. You are configuring NAT on your router and need to create an access list that includes all IP addresses in the 192.168.3.0 network so that they can use NAT services. Which of the following would you use as the access list?

A. `R1(config)#access-list 1 permit 192.168.3.0 0.0.0.255`

645. You need to configure OSPF on your router for network 192.168.2.0. What two commands would you use? (Select two.)

```
Router(config)# router ospf PROCESS_ID
Router(config-router)# network IP_ADDR WILDCARD_MASK
							area AREA_#
```

B. `router ospf 3`
E. `network 192.168.2.0 0.0.0.255 area 0`

646. You have a router with the following configuration. What is the OSPF router ID of the router?

```
Loopback 0: 1.1.1.1
Loopback 1: 2.2.2.2
FastEthernet 0/0: 12.0.0.1
FastEthernet 0/1: 13.0.0.1
Serial 0/0: 14.0.0.1
```

A. 2.2.2.2

The router ID that OSPF uses if one has not been configure is the highest to a loopback interface. If a loopback interface does not exist, then it is the highest IP address assigned to a physical interface.

647. What keystroke is used to suspend a telnet session?

B. `CTRL-SHIFT-6, then X`

648. You have client computers on your network that are having issues establishing sessions with a remote website. The router is performing NAT. You would like to reset their dynamic NAT addresses through the router. What command will erase their NAT entries in the router?

D. `clear ip nat translations *`

649. Which routing protocol supports transport protocols of IP, IPX, and Appletalk?

D. EIGRP

650. You have configured multiple routing protocols on your network. There is a path discovered to a remote network segment by each routing protocol. Which protocol's path will be chosen by your router?

| Value | Route Type                                                       |
| ----- | ---------------------------------------------------------------- |
| 0     | Connected interface route                                        |
| 1     | Static route                                                     |
| 90    | Internal EIGRP router (within the same AS)                       |
| 110   | OSPF route                                                       |
| 120   | RIPv1 and v2 route                                               |
| 170   | External EIGRP (from another AS)                                 |
| 255   | Unknown route (considered an invalid route and will not be used) |

D. EIGRP (internal)

651. Which of the following commands is used to assign a default gateway to a Cisco switch?

D. `R1(config)#ip default gateway 23.0.0.1`

652. You issue the command show mac-address-table on your switch. There are several MAC addresses listed for many of the ports. What is the significance of multiple addresses being listed on a port?

C. Multiple addresses let you know that the port is connected to another switch or hub

653. Which of the selections will reduce the size of a collision domain? (Select two.)

A. Upgrading a hub to a switch
C. Adding a bridge between two hubs

654. You have one switch on which you have created three VLANs. You have decided that you need to have devices on these VLANs communicate with each other. You need to add which device to your network to allow the communication?

D. Router

655. Which of the following commands would you use if you wanted the port security feature to learn the MAC addresses of connected systems?

R1(config-if)#switchport port-security mac-address sticky

656. Which Cisco command was used to create the output in the figure below?

![[Pasted image 20260107201013.png]]

C. `show cdp neighbors`

657. You wish to view the list of VLANs and the ports associated with each VLAN. What command would you use?

A. `show vlan`

658. You wish to configure all the ports on the switch for access mode. What command should you use?

D.`SW1(config-if-range)#switchport mode access`

659. You wish to configure an interface on the switch for full duplex mode. What command would you use?

B. `SW1(config-if)#duplex full`

660. VLAN information is propagated to all switches on a network through which protocol?

C. VTP (VLAN Trunking Protocol)

RIP: Routing protocol
STP: Spanning tree protocol, used for prevent loop
CDP: Cisco Discovery Protocol, used for find neighbors

661. When a switch is participating in a STP network, how is the root bridge determined on the network? (Select two.)

B. Lowest priority
D. Lowest MAC address

#### Switch ID

The switch ID is used to elect the root switch. The switch with the *lowest* switch ID is chosen as root.

The switch ID is made up of two components:

* The switch's priority, which defaults to 32,768 on Cisco switches (2 bytes in length)
* The switch's MAC address (6 bytes in length)

662. What are possible port states for a switch port that is participating in Spanning Tree Protocol (STP)? (Select three.)

* Blocking
* Listening
* Learning
* Forwarding
* Disabled

B. Blocking
C. Learning
F. Forwarding

663. Which port security violation mode disables the interface on the switch if an invalid system connects to the port until the administrator enables the interface?

C. Shutdown

**Switchport modes**

* **Protect**
	* This command sends an alert to the administrator when an unauthorized device is connected to the port.
* **Restrict**
	* The `restrict` setting results in the port being able to forward frames for the authorized device only.
	* If an unauthorized device connects to the port, the switch will drop those frames.
* **Shutdown**
	* With this command, the port is disabled the second an authorized device connects to the port.
	* The port is disabled until the administrator issues the `no shutdown` command.
	* This is the default violation mode if you don't specify the mode.

664. Which of the following commands creates a VLAN named MKT?

B. `3524XL(vlan)#vlan 2 name MKKT`

665. In order to configure port security on a Cisco switch, what mode does the port need to be running in?

C. Access

666. You wish to verify that port security was configured on port 6 of the switch. What command would you use?

A. `show port-security interface f0/6`

667. Given the network shown in the figure below, what switch will be the root bridge and what port will be the blocking port? (Select one switch and one port.)

![[Pasted image 20260107202438.png]]

Root: Switch 1
Switch 2 root port: D
Switch 3 root port: F
Switch 4 root port: H

Disabled -> I

A. Switch 1 will be root bridge
G. Port I will be blocking

668. You are using Catalyst 2950 24 port switch. You have pressed the mode button to enable UTIL mode. All the port LEDs turn green except for the last GBIC port. What is the bandwidth  utilization on the switch?

C. Between 25% and 50%

If all the LEDs were lit up, then the switch would be using more than 50% of its bandwidth; since there is still one LED that is out, the useage is between 25% and 50%

669. You wish to verify that you can communicate with a host on the network. What command would you use?

C. `ping <ip_address>`

670. Your coworker has configured the router with the following commands. What do they do?

```
line vty 0 4
password telnetP@ss
login
transport input ssh
```

A. Ensure that virtual terminal port communication is encrypted

671. You have used both enable secret and enable password commands to set passwords for your router. How do you get to the # prompt?

A. Use the secret password

672. The output of the `show interface` command is shown in the figure below. How many CRC errors have been encountered on this switch port?

![[Pasted image 20260111041015.png]]

A. 5

673. What are effects of using a half-duplex setting on a switch port? (Select two.)

A. Data can be sent only in one direction at a time
D. There will be an increase in throughput

Half-duplex settings only allow data to be sent in one direction at a time. This means that even on a port with a single device attached, there is an increased change that another deivce will send data to the end node when that end node is attempting to send data, which will yeid a collision.

674. What information is displayed by the show version command?

D. How the system was last restarted

`show version` will usually display hardware model, IOS version, compilation date, uptime, what initiated the last resort, the system image filename, the active configuration register, and hardware configuration such as ports, RAM, and NVRAM.

675. A standard ACL is able to filter traffic based only on which of these items?

B. Source IP address

#### Standard ACL / Extended ACL

* Standard ACL
	* ACL number range 1-99, 1300-1999
	* Filter only source IP address
* EX ACL
	* ACL number range 100-199, 2000-2699
	* It can filter:
		* TCP/IP protocol
		* Protocol information
			* Port numbers (TCP/UDP)
			* message type (ICMP)

676. What is a valid purpose for implementing ACLs?

C. IP route filtering

677. You wish to encrypt all passwords on the Cisco device. What command would you use?

B. `service password-encryption`

678. Which of the following commands would you use to troubleshoot layer 3 issues? (Select two.)

B. `show ip route`
D. `show ip protocols`

`show interfaces`, `show controllers` => layer 1 and layer 2

The `show ip protocols` command displays all the routing protocols, including RIP, which you have configured and are running on your router.
`show ip route` shows whole routing table information

679. You wish to view the IP addresses of Cisco devices closest to you. Which command would you use?

A. `show cdp neighbors detail`

680. You have configured a banner using the following command. What is wrong with the configuration?

```
R1(config)#banner motd #
Enter TEXT message. End with the character ‘#’.
Welcome to Glen’s network!
If you require assistance call the help desk at 555-5555.
```

D. Banners should not welcome someone.

681. Which command was used to create the output displayed in the figure below?

![[Pasted image 20260111050956.png]]

C. `show ip interface brief`

682. You have configured a router with an ACL and placed it on the inbound direction of an interface. When are packets processed against this ACL?

#### Inbound/Outbound

* Inbound
	* 패킷이 외부에서 내부 인터페이스로 들어올 때 비교
* outbound
	* 외부에서 들어온 패킷이 외부로 forwarding 될 때 비교

C. Before routing packets to an outbound interface

When an inbound ACL is placed on an interface, the ACL is processed when traffic enters the interface, before routing to an outbound interface.

683. You rely on CDP for network administration within your network, but you would like to prevent CDP information from being accessed from external devices. Which set of commands should you execute on your router?

```
interface serial0/0
no cdp enable
```

684. You have used SSH to connect to your switch, which is experiencing performance issues. You have enabled several debug options, but you do not see the output of the commands on your screen. What must you do to enable this functionality?

A. Type `terminal monitor`

685. You have lost the password for a network switch. What is the first step in the switch Cisco IOS password recovery process?

B. Enter rommon

#### Password recovery process

1. enter rommon mode
2. change configuration register to 0x2142 (`confreg`)
3. reboot
4. set password, check configuration (`show version`)
5. change configuration register to 0x2012 (to default, `config-register`)
6. reboot (`reload`)


-
686. You have configured your switch for SSH access using the commands shown in the figure below. When you attempt to connect to the switch you are not able to connect with ssh, but you are still able to connect to the switch with telnet. What is the likely cause the problem? 

![[Pasted image 20260111052834.png]]

C. You did not configure the vty (`transport input ssh`)

687. You have configured messages for banner exec, banner incoming, banner login, and banner motd. When you connect to a console interface, what is the first message you will see?

##### MOTD ordering

* **MOTD banner**
	* The MOTD banner appears before the administrator is asked to log in and is used to display a temporary message that may change from day to day
* **Login banner**
	* The login banner displays before the administrator logs in, but the MOTD banner appears, and is used to display a more permanent message to the administrator.
* **EXEC banner**
	* The exec banner is used to display a message after the administrator authenticates to the system and after he enters user EXEC mode.

MOTD -> login -> exec

router boots up -> *MOTD banner* -> *Login banner* -> after user logs in (enters EXEC mode) -> *EXEC banner*

D. banner motd

688. Your router does not have a boot directive in the current configuration. What is the order that the router will use to locate a valid IOS to load?

C. Flash, TFTP server, ROM

If there is no boot directive in the configuration, which specifies an IOS image to load, the router will search Flash for a valid IOS image. Then it will attempt to locate a TFTP server with an IOS image, and finally will load a mini IOS image that is found in ROM.

689. What is the transmission rate of 802.11g?

##### Wireless Standards

| Standard | Freq      | Transfer Rate  | Compatibility |
| -------- | --------- | -------------- | ------------- |
| 802.11a  | 5 GHz     | 54 Mbps        | 802.11a       |
| 802.11b  | 2.4 GHz   | 11 Mbps        | 802.11g/n     |
| 802.11g  | 2.4 GHz   | 54 Mbps        | 802.11b/n     |
| 802.11n  | 5/2.4 GHz | up to 600 Mbps | 802.11a/b/g   |
| 802.11ac | 5 GHz     | 1 Gbps         | 802.11a/b/g/n |

C. 54 Mbps

690. When a wireless network contains access points, it is referred to as which type of network?

D. Infrastructure

802.11b is WiFi standard, Ad-hoc does not contain dedicated access point (device itself acts like access-point).

When your wireless network uses access point rather than peer-to-peer networking, it is running in infrastructure mode, while peer-to-peer networks in ad-hoc mode.

691. You have just implemented a new 802.11b access point. What is the maximum speed of devices connected to this access point?

C. 11 Mbps

a, g -> 54 Mbps
b -> 11 Mbps
n -> 600 Mbps
ac -> 1 Gbps

692. What is the speed of an ISDN BRI link?

ISDN Basic Rate Interface (BRI) subscriptions have 128 Kbps bandwidth

C. 128 Kbps

693. Which of the following represents the term used for a wireless network made up of multiple access points using the same SSID?

A. ESS

The Extended Service Set (ESS) is the term for a wireless network that uses multiple access points with the same SSID

BSS (basic service set) has a single AP

694. What are three impacts that occur when objects are in the path of wireless or RF signals? (Select three.)

B. Refelction
D. Scattering
F. Absorption

RF waves suffer from refelction, sscattering, and absorption when they encounter objects in the path of their waves.

695. You have a serial port on your router and when you run show interface serial, you are told, Serial1 is up, line protocol is down. What are two possible causes of this error? (Select two.)

A. You have not set a clock rate
D. You have set the incorrect encapsulation type

696. You are implementing a new 802.11n network using the 2.4GHz channels. How many non-overlap-ping channels are available for use in the United States?

B. 3

1, 6, 11 channels do not overlap
![[Pasted image 20251110180737.png]]

697. Which selection is an authentication protocol that can be used on WAN connections that have used PPP encapsulation?


C. CHAP

698. What component does your router use to connect to a T1 leased line?

A. CSU/DSU

699. Which of the commands are valid on a WAN interface, but would not be used on a LAN interface? (Select two.)

C. `encapsulation ppp`
E. `authentication chap`

Encapsulation and authentication commands only used when dealing with WAN interfaces.

700. In regards to STP, what is the default priority on a Cisco switch?

A. 32768

701. Which layer of the OSI model does RSTP run at?

Rapid Spanning Tree Protocol (layer 2, Data Link)

B. Data Link

702. Which of the following is a benefit of RSTP over STP?

A. RSTP transitions to a forwarding state faster than STP does.

703. Which of the following terms describes a spanning tree network where all switch ports are in either forwarding or blocking state?

B. Converged

The term used for when the switch has gone through the election process and determined which ports are to be in a forwarding state and which ones are to be in a blocking state is "converged."

704. Which of the following is selected by STP as the root bridge?

D. The switch with the lowest bridge ID

The bridge ID is made up of the priority + MAC address of the switch

705. PVST+ introduced which of the following port states?

C. Discarding

Per VLAN Spanning Tree (PVST+) is based on the 802.1D standars, but uses only three port states of learning, forwarding, and discarding. (Like RSTP)

| STP        | RSTP       | PVST+      |
| ---------- | ---------- | ---------- |
| blocking   | Discarding | Discarding |
| listening  | Discarding | Discarding |
| Disabled   | Discarding | Discarding |
| learning   | learning   | learning   |
| forwarding | forwarding | forwarding |

706. Which of the following represent STP port states? (Select three.)

A. Blocking
B. Listening
D. Forwarding

The *disabled* state is a special port state. A port in a disabled state is not participating in STP.

707. Looking at the output of the show spanning-tree command displayed in the figure below, is SW1 the root bridge?

![[Pasted image 20260111073437.png]]

A. yes

When looking at the output of the `show spanning-tree` command, you can see the bridge ID of the device (the priority and MAC address).
Above the bridge ID, you can see the device that was selected as the root bridge. It also says, "This bridge is the root".

708. Looking at the following switch information, how can you configure SwitchC as the root bridge?

```
Name: SwitchA
Priority: 32768
MAC: 00-00-0c-00-b0-01

Name: SwitchB
Priority: 32768
MAC: 00-50-0d-10-00-00

Name: SwitchC
Priority: 32768
MAC: 0b-3f-27-00-93-3a
```

B. Lower the priority

709. Once the root switch has been selected, each switch must then determine its root port. Which port is selected as the root port?

C. The port with the lowest cost

710. Which of the following are true regarding RSTP? (Select three.)

B. Reduces converging time after a link failure
D. Uses additional port roles over STP (alternative ports, which are secondary root port, and backup ports which are secondary designated port)
E. Transitions to a forwarding state faster than STP

711. You would like to verify if your switch is the root bridge. What command would you use?

B. `show spanning-tree`

712. You have three switches connected in a loop with the configuration of each switch shown below. Which switch will be selected as the root bridge?

```
Name: SwitchA
Priority: 32768
MAC: 00-00-0c-00-b0-01

Name: SwitchB
Priority: 32768
MAC: 00-50-0d-10-00-00

Name: SwitchC
Priority: 32768
MAC: 0b-3f-27-00-93-3a
```

A. SwitchA (lowest MAC address)

713. Which of the follow commands was used to create the output in the figure below?

![[Pasted image 20260111084107.png]]

D. `show interface fastethernet 0/1 switchport`

The `show interface f0/1 switchport` command can be used to display trunking information such as the operational mode, the encapsulation protocol being used, the trunking VLANs, and the pruning VLANs.

714. Which of the following identifies additional port roles used by RSTP?

Alternative port = secondary root port
backup port = secondary designated port

D. Alternative

715. You would like to ensure that when a system connects to a switch that it receives connectivity right away with little wait time. Which of the following commands would you use?

A. (config-if)#spanning-tree portfast

716. Using the figure below, what do you need to do on switch SW2 in order to ensure it is the root bridge?

![[Pasted image 20260111084345.png]]

D. Decrease the priority below 4096

717. Which of the following statements are true of VLANs and their usage? (Select two.)

D. Each VLAN requires its own IP subnet.
A. Communication between VLAN requires a router

718. Which of the following are VLAN tagging protocols?

A. ISL
C. 801.q

802.11 is wireless standard
802.1d is STP

ISL is a Cisco proprietary protocol. (InterSwitch Link)

719. You would like to create a VLAN called MKT on your switch. Which command(s) would you use?

```
vlan 10
name MKT
```

720. Which of the following statements is true of the native VLAN?

A. Both sides of the trunk link should use the same native VLAN.

The native VLAN is the VLAN used on frames that have not been tagged for a particular VLAN. It is critical to set the native VLAN to the same VLAN on both ends of the trunk link.


721. You want to ensure that only VLAN 10 and VLAN 20 traffic can pass over a trunk link. Which command would you use?

C. `SW1(config-if)#switchport trunk allowed vlan 10,20`

By default, traffic for all VLANs is allowed to pass over a trunk port, but you can specify VLANs the `switchport trukn allowed vlan` command.

722. Looking at the figure below, which of the following statements are true about the interVLAN routing?

![[Pasted image 20260113025546.png]]


B. F0/0 must be configured with sub-interfaces
D. F0/0 on R1 and F0/24 on the switch must use the same encapsulation protocol

In order to route between two VLANs you must configure the router as a router-on-a-stick. This involves configuring the interface on the router with sub-interfaces and enabling the connecting port on both the router and switch as a trunk port for the same encapsulation protocol.

723. Two switches are connected together by a crossover cable. The ports connecting the two switches have been configured for access mode. You have systems on each switch that are part of VLAN 20 and VLAN 30. What is your assessment of this configuration?

A. Systems will not be able to communicate between the two switches (not trunk port)

In order to have the VLAN traffic travel between the two switches, you must configure the ports that connect the two switches together for trunk mode, not access mode.

724. Using the figure below, which of the following statements are true of router R1?

![[Pasted image 20260113034833.png]]

C. Interface f0/0 should be configured as a trunk port

When creating a router on a stick scenario, you will need to create the sub-interfaces on the port connected to the switch, but you also need to enable the port on both the router and the switch as a trunk port so that it can carry VLAN traffic.

725. You have created VLAN 10 and would like to place Fast Ethernet port 0/8 in VLAN 10. Which command(s) would you use?

```
sw(config)#interface f0/8
sw(config-if)#switchport access vlan 10
```

726. Which of the following port types cannot be ISL trunk ports?

C. 10 Mbps Ethernet

ISL trunk ports must be above 10 Mbps in order to carry VLAN traffic

727. Which protocol is used to facilitate the management of VLANs?

D. VTP (VLAN trunking protocol)

802.1q, ISL -> encapsulation protocol
STP -> spanning tree protocol (prevent loop in switches)

728. Which VTP mode does not allow the creating of VLANs?

A. Client mode

Parent mode does not exist, it has client, server, and transparent mode

729. You wish to prevent the sending of VLAN traffic to other switches if the destination switch does not have any ports in a specific VLAN. What VTP feature would you enable?

B. VTP pruning

730. You would like to configure switch SW2 to receive VLAN information for the glensworld VTP domain. Which command(s) would allow you to do this?

```
vtp domain glensword
vtp password P@ssw0rd
vtp mode client
```

731. Which VTP mode allows the creation of VLANs but does not accept changes from other VTP systems and does forward VTP messages on to other devices?

C. Transparent mode

732. Which of the following commands configures a port on the Cisco switch for trunking using 802.1q protocol?

```
sw(config-if)#switchport mode trunk
sw(config-if)#switchport trunk encapsulation dot1q
```

733. You have configured SW1 as a VTP server for the GlensWorld VTP domain and a password of P@ssw0rd. You use the following commands to configure SW2 as a VTP client, but are unsuccessful. What is the problem?

```
vtp domain glensworld
vtp password P@ssw0rd
vtp mode client
```

D. The VTP domain name is case sensitive

734. You have typed the following command on switch SW1. Using the figure below, what effect will the commands have on the network?

```
Interface f0/24
Switchport mode access
```

![[Pasted image 20260113071115.png]]

B. Systems will not be able to communicate between the two switches.

735. You wish to configure an Etherchannel link made up of two Fast Ethernet trunk ports without any negotiations. What command would you use when configuring the interfaces?

D. `channel-group 1 mode on`

**EtherChannel Modes**

| Mode      | Protocol | Desc                                                                                                                                               |
| --------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| auto      | PAgP     | Passively listens for PAgP queries from a Cisco device configured with either desirable or on. By default, the interface is not part of a channel. |
| desirable | PAgP     | Generates PAgP queries to form a channel, but by defaul is not part of a channel.                                                                  |
| on        | PAgP     | Generates PAgP queries and assuems the port is part of a channel.                                                                                  |
| active    | LACP     | Enables a channel if the other side responds to its LACP messages.                                                                                 |
| passive   | LACP     | Passively listens for LACP messages to from a channel from active port.                                                                            |

| Protocols | Desc                                                                                                                                                                                   |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PAgP      | Proprietary to Cisco. It enables connected devices to group similary configured ports dynamically into a single channel                                                                |
| LACP      | Defined in the IEEE 802.1ad standard. Like PAgP, it learns from a connected device which ports between the two are identically configured and dynamically forms a channel between them |

Setting the channel-group mode to on forces the interfae to be part of the grouping without any protocol negotiations. If you set the mode to active, then the interface can negotiate with other ports to be part of the grouping.

736. Which of the following is true in regards to the following commands?

```
Interface fa0/8
Switchport mode access
Switchport port-security
Switchport port-security maximum 3
Switchport port-security mac-address 1111.2222.3333
```

D. The system with the MAC address of 1111.2222.3333 can connect to port 8 along with two other systems.

737. You have configured three trunk ports in an Etherchannel group. What will happen when one port in the grouping fails?

A. The channel cost is increased

Because the Etherchannel group has lost some bandwidth, the cost on the grouped link is increased.

738. Where is the POST routines stored on a Cisco device?

C. ROM

* ROM
	* POST Routine
	* Bootstrap program
	* ROMMON
* Flash
	* IOS image
* VRAM (RAM)
	* Active configuration file (running-config)
	* Any tables (routing table, ARP, ...)
* NVRAM
	* Configuration file (starup-config)

739. Which of the following describes the benefit of booting from a TFTP server?

B. A central place to upgrade the IOS

740. What happens after the bootstrap program locates the IOS in flash memory?

**Boot Process**

* POST Routine
* bootstrap program loaded and executed
* Configuration register checked
* (In bootstrap program) finds and loads IOS
* After IOS loaded, find and load configuration file

C. The IOS is loaded into RAM

741. You wish to disrupt the boot process on the Cisco device. What keystroke would you press?

| Seq                   | Desc                                                                                               |
| --------------------- | -------------------------------------------------------------------------------------------------- |
| CTRL-A                | Moves the cursor to the begining of the line                                                       |
| CTRL-E                | Moves the cursor to the end of the line                                                            |
| ESC-B                 | Moves the cursor back one word at a time                                                           |
| ESC-F                 | Moves the cursor forward one word at a time                                                        |
| CTRL-B or LEFT-ARROW  | Moves the cursor back one character at a time                                                      |
| CTRL-F or RIGHT-ARROW | Moves the cursor forward one character at a time                                                   |
| CTRL-P or UP-ARROW    | Recalls the last command                                                                           |
| CTRP-N or DOWN-ARROW  | Recalls the most recently executed command                                                         |
| CTRL-D                | Deletes the character the cursor is under                                                          |
| Backspace             | Deletes the character preceding the cursor                                                         |
| CTRL-R                | Redisplays the current line                                                                        |
| CTRL-U                | Erases the line completely                                                                         |
| CTRL-W                | Erases the word the cursor is under                                                                |
| CTRL-Z                | Takes you from configuration mode back to privileged EXEC mode                                     |
| TAB                   | IOS device completes the word                                                                      |
| $                     | At the begining of a command line, this indicates that there are more characters to the right of $ |
| CTRL-Break            | disrupt the boot process                                                                           |

B. CTRL-Break

742. Which type of memory is used to store the bootstrap program?

C. ROM

743. The startup configuration is stored in which type of memory?

B. NVRAM

744. What happens if a startup config does not exist on boot-up?

If the startup config does not exist, the Cisco device will then try to load a startup
configuration from a TFTP server. If a TFTP does not exist, the device will then present
the initial configuration dialog to configure the device.

startup x -> tftp -> (if doesn't exist) -> initial configuration dialog

A. The device will try to connect to a TFTP server for the configuration

745. What command would you use to configure the router to not load the startup configuration?

D. `config-register 2142`


746. hich component is responsible for locating an operating system to load in memory?

A. Bootstrap

747. Looking at the figure below, what is the command to configure the Cisco device to boot from the IOS stored in flash memory?

![[Pasted image 20260113153834.png]]

B. `boot system flash c2800nm-advipservicesk9-mz.124-15.T1.bin`

748. What command would you use to change the configuration file that is applied on startup?

A. `boot config <file>`

749. You are troubleshooting the startup of your Cisco switch. What command was used in the figure below to display the following output?

![[Pasted image 20260114043427.png]]

D. `show boot`

750. The bootstrap program is stored in what type of memory on the Cisco router?

C. ROM

751. Looking at the figure below, what is the name of the image file that contains the Cisco IOS?

![[Pasted image 20260114043642.png]]

D. c2800nm-advipservicesk9-mz.124-15.T1.bin

752. You wish to back up your IOS image to a TFTP server. What command would you use?

A. `copy flash tftp`

753. Which of the following would be a downfall of loading the IOS image from a TFTP server?

A. If the server is unavailable devices cannot boot up

754. You have a copy of your IOS image located on a TFTP server. What command would you use to restore the IOS to your device?

D. `copy tftp flash`

755. You wish to find out what version of the Cisco IOS you are running. What command would you run?

D. `show version`

756. What command would you use to configure the Cisco device to boot from an IOS image located on a TFTP server?

C. `boot system tftp <filename>`

757. By default, the Cisco bootstrap program loads which IOS image if one is not specified?

D. The first image stored in flash

758. What command would you use to specify a secondary bootstrap image?

C. `boot bootstrap`

759. You need to find out what IOS image file was used to boot the router. What command would you use?

B. `show version`

760. What command would you use to configure your router to load the IOS image from a TFTP server?

C. `boot system tftp <filename>`

761. You wish to delete a file from flash memory. What command would you use?

A. `delete flash:<filename>`

762. You wish to invoke the initial configuration dialog. What command would you use?

D. `setup`

763. What file extension do Cisco IOS images use?

D. `.bin`

764. You are customizing the boot preferences and would like to change the size of the NVRAM buffer used for the IFS. What command would you use?

A. `boot buffersize`

Older versions of the IOS allowed you to alter how much NVRAM is used for the IFS by using the boot buffersize command.

765. You would like to know how much memory exists on the Cisco device. What command would  you use?

B. `show version`

766. Looking at the output of the figure below, what command was used?

![[Pasted image 20260114045919.png]]

B. `show flash`

767. What command can you use on your Cisco router to display the available flash memory?

A. `show flash`

768. You wish to display the contents of NVRAM. What command would you use?

A. `dir nvram`

769. Looking at the figure below, what is the platform for the device?

![[Pasted image 20260114050136.png]]

A. 2800

770. Which file contained in flash memory on a Cisco switch is used to store secured configuration data such as cryptography keys?

D. private-config.text

771. You wish to display the contents of flash memory using IFS commands. What ­command would you use?

B. `dir flash:`

772. Looking at the figure below, how much flash memory exists in the device?

![[Pasted image 20260115142318.png]]

A. 62720k

773. You wish to remove a license from your Cisco device. What command would you use?

C. `license clear ipservices`

774. You have copied a new Cisco license file called r1fs-ips from the TFTP server to flash memory. How do you install this new license on your Cisco switch?

B. `license install flash: r1fs-ips`

775. You wish to display the current license information. What command would you use?

B. `show license file`

776. Which of the following are true of RIPv1? (Select two.)

B. Is classful
C. Administrative distance of 120

#### Administrative Distance 

| Dist | Protocol         |
| ---- | ---------------- |
| 0    | Connected        |
| 1    | Static (default) |
| 90   | internal EIGRP   |
| 110  | OSPF             |
| 120  | RIPv1 and RIPv2  |
| 170  | External EIGRP   |
| 255  | Unknown          |

**Static route administrative distance**

```
Router(config)# ip route DEST_NETWORK_# [SUBNET_MASK] 
				IP_ADDR_OF_NEXT_HOP [ADMINISTRATIVE_DISTANCE] [permanent]

or

Router(config)# ip route DEST_NETWORK_# [SUBNET_MASK] 
				EXIT_INTERFACE_NAME [ADMINISTRATIVE_DISTANCE] [permanent]
```

It can be changed, defaults to 1

777. You have configured a network as shown in the figure below. From router R1, you can ping the S0/0 interface on router R2, but you cannot ping the F0/0 interface on router R2. What is the cause of the problem?

![[Pasted image 20260115143135.png]]

B. There is no route for the 13.0.0.0 network on router R1.

You need add static route or load a dynamic routing protocol.

778. Which of the following is true of classful routing protocols?

D. They do not send the subnet mask info with the routing update

Classful routing protocols do not send the subnet mask information with the routing update, so therefore do not support VLSM; they assume the same subnet mask is used on each network.

779. Which of the following is true of RIPv2? (Select two.)

C. Administrative distance of 120
D. Is classless

780. Distance vector routing protocols use which of the following as their metric?

C. Hop count

781. What is the administrative distance of RIP?

D. 120

0 -> directly connected
1 -> static
90 -> Internal EIGRP
110 -> OSPF
120 -> RIP
170 -> External EIGRP
255 -> unknown

782. Which of the following is true of EIGRP? (Select two.)

B. Is vendor-specific
E. Administrative distance of 90 (Internal EIGRP)

#### EIGRP

* Updated version of IGRP
* Cisco proprietary
* Classless
* max hop count of 255
* AS (autonomous system)
* routing table is advertised every 90 seconds
* using hop count as metric

783. Which of the following identifies the benefit of static routing over dynamic routing? (Select two.)

A. More secure because the administrator must add the routes.
D. No bandwidth is being used to update the routing tables.

784. What is the default administrative distance of a static route?

E. 1

785. Which of the following is true of OSPF? (Select two.)

A. Elects a DR
D. Uses cost as its metric
E. Administrative distance of 110

#### OSPF

* Open standard
* Classless
* Uses cost as metric
* Administrative distance of 110
* DR, BDR

786. What is the administrative distance of OSPF?

C. 110

787. You are the network administrator for a small network that has two routers (R1 and R2, as shown in the figure below) that are connected via serial interfaces. You have used the command Ip route 13.0.0.0 255.0.0.0 12.0.0.2 on R1 to finalize configuration. What happens if the serial interface on R2 shuts down?

![[Pasted image 20260115145242.png]]

C. The route of 13.0.0.0 remains on router R1

Because you are using static routing, there is no way for router R1 to know that the interface is down on router R2. This means that the route is still valid on router R1 as far as router R1 is concerned.

788. You have no static routes configured on the router. A router learns about a route to the same network from OSPF, EIGRP, and RIP. Which route is added to the routing table?

If EIGRP is Internal it selects EIGRP (cost 90)

B. The EIGRP route

0 - connected
1 - static
90 - internal EIGRP
110 - OSPF
120 - RIP
170 - External EIGRP
255 - unknown

789. Which of the following is a benefit of triggered updates?

A. It decreses convergence time by sending out an update as soon as there is a change to the network topology.

790. You are troubleshooting communication on router R1. You use the show ip route command to display the routing table. Using the output of the figure below, how will information be sent from R1 to a system with the IP address of 15.0.0.25?

![[Pasted image 20260115150206.png]]

D. Through 12.0.0.2, and through 14.0.0.2

When looking at the routing table, you can see that the route to the 15.0.0.0 network has two pathways – one going through 12.0.0.2 and the other pathway going through 14.0.0.2. In this case, you will notice that the administrative distance (90) and the cost (2681856) are the same. When the cost is the same, the router will load balance between the routes.

791. Which of the following is true of link state routing protocols? (Select two.)

B. They use more resources as they store mulitple tables in memory
D. Use triggered updates to reduce the time to converge

792. You would like to ensure that a static route is used as a backup over a route using the same pathway discovered by a dynamic routing protocol. What parameter would you change?

C. Administrative distance

793. What are the three tables that a link state routing protocol typically uses? (Select three.)

A. Topolgy table
C. Neighbor table
E. Routing table

794. Which of the following routing protocols does not auto summarize routes?

B. OSPF

795. Looking at the figure below, when router R1 sends data to router R4, what pathway will be used?

![[Pasted image 20260115151418.png]]

C. Through R2 (OSPF: 110, RIP: 120)

796. Which of the following commands alters the administrative distance on the static route from the default value?

D. `ip route 21.0.0.0 255.0.0.0 20.0.0.2 110`

797. You would like to enable IPv6 routing on your Cisco router. What command would you use?

D. `ipv6 unicast-routing`

798. Which of the following does a link state routing protocol use to build the topology database? (Select two.)

B. Hello messages
E. LSA from other routes

A link state routing protocol uses hello messages that it sends out to each of its interfaces to discover neighboring routers. The LSA messages are used to announce changes in the network topology.

799. Which of the following is the term for packets that are flooded when a topology change occurs?

D. LSA

800. Which of the following causes a router to ignore updates from lower metric paths for a period of time?

A. Holddown timer

The holddown timer is a feature on a router that forces it to retain routing information for a short period of time even after receiving a routing update with conflicting information. Once a route is marked unreachable, the route is held for the duration of the timer before accepting a new route to the destination.

801. Which of the following is the term for preventing a router from sending updates on a route through the interface on which it received the knowledge of the route?

E. Split horizen

Split horizon is a feature to help prevent routing loops by preventing a router from sending knowledge of a route out the interface it received knowledge of the route through.

802. Which of the following involves a router learning that a route is down from its neighbor, and then sending an update back to the neighbor on the route with an infinite metric?

B. Poison reverse

803. Which of the following routing protocols are vendor specific?

B. EIGRP

804. What is the goal of route summarization?

D. To reduce the size of the routing table

805. You are configuring OSPF and need to determine the summary route for networks 131.107.16.0 up to 131.107.19.0. Which of the following identifies the summary route?

131.107.00010000.0
131.107.00010011.0
-> 131.107.000100 is network ID (/22)

D. 131.107.16.0/22

806. You are configuring OSPF and need to determine the summary route for networks 172.16.0.0 up to 172.16.7.0. Which of the following identifies the summary route?

172.16.00000000.0
172.16.00000111.0
-> 172.16.00000 (/21)

B. 172.16.0.0/21

807. Which of the following identifies the summary route for networks 10.4.0.0 up to 10.7.0.0?

10.00000100
10.00000111
-> 10.000001 (/14)

C. 10.4.0.0/14

808. Which of the following identifies the summary route for networks 120.12.0.0 to 12.15.0.0?

120.00001100
120.00001111
-> 12.000011 (/14)

B. 120.12.0.0/14

809. You have a router connected to a switch that has three VLANs. You would like to configure the router so that it can be used to route traffic between the three VLANs. What do you need to do?

C. Configure router on a stick

810. Which of the following switch commands would you use on the port that is connected to a router on a stick?

A. SW1(config-if)#swichport mode trunk

811. Router R1 is connected to a switch that has three VLANs. You are configuring the router to route traffic between the three VLANs. What do you need to do? (Select two.)

A. Configure the interface that is connected to the switch as a trunk port using the same encapsulation protocol.
D. Configure the interface that is connected to the switch with three sub-interfaces

812. Looking at the figure below, how would you configure the router and switch to route between the VLANs? (Select two.)

![[Pasted image 20260115161837.png]]

```
R1(config)#interface f0/0
R1(config-if)#no shutdown
R1(config)#interface f0/0.10
R1(config-subif)#encapsulation dot1q 10
R1(config-subif)#ip address 192.168.10.1 255.255.255.0
R1(config)#interface f0/0.20
R1(config-subif)#encapsulation dot1q 20
R1(config-subif)#ip address 192.168.20.1 255.255.255.0

SW(config)#interface f0/1
SW(config-if)#switchport mode trunk
```

813. Sue has been assigned the task of creating a router-on-a-stick network. She has configured what is shown in the figure below. Assuming all interfaces have been properly configured, what else needs to be done?

![[Pasted image 20260115162217.png]]

A. Noting

814. What is the administrative distance of RIP?

B. 120

0 - directly connected
1 - static (default)
90 - Internal EIGRP
110 - OSPF
120 - RIP
170 - External EIGRP
255 - Unknown

815. After configuring RIP, you are not receiving any RIP entries in your routing table. What command could you use to verify you are receiving RIP updates?

C. `debug ip rip`

816. Which RIP version supports VLSM?

B. Version 2

817. Which of the following identifies differences between RIPv1 and RIPv2? (Select two.)

RIPv1, v2 sends updates every 30 seconds
RIPv1 is classful, RIPv2 is classless (supports VLSM)
RIPv2 supports authentication
RIPv1 sends updates via broadcast, RIPv2 sends updates via multicast

| v1        | v2                     |
| --------- | ---------------------- |
| broadcast | multicast              |
| classful  | classless              |
|           | authentication support |

C. RIPv2 sends updates using multicast communication
E. RIPv2 supports authentication


818. You have configured RIPv2 and you do not want it to auto summarize routes. What command would you use?

B. `no auto-summary`

819. Looking at the figure below, what version of RIP has been enabled?

![[Pasted image 20260115182650.png]]

B. Version 1

820. How frequently does RIPv2 send out routing table updates?

A. Every 30 seconds

821. You want to change the administrative distance of RIP to a value other than its default. What command would you use?

A. `R1(config-router)#distance 80`

0 - connected
1 - static
90 - Internal EIGRP
110 - OSPF
120 - RIP
170 - External EIGRP
255 - unknown

822. You have already configured RIPv1, but have recently learned of the benefits of RIPv2. What additional command(s) would you use to enable RIPv2?

A. `R1(config-router)#version 2`

823. Looking at the figure below, what is the administrative distance?

![[Pasted image 20260115183210.png]]

A. 25

824. You have configured RIP on your router and want to view the entries in the routing table. What command would you use?

C. `show ip route`

825. You have a router with two networks you wish to advertise through RIP – the 192.168.3.0 and the 192.168.4.0 networks. Which of the following identifies the commands you would use?

```
R1(config)#router rip
R1(config-router)#network 192.168.3.0
R1(config-router)#network 192.168.4.0
```

826. Looking at the figure below, how many hops away is the 14.56.7.8 system?

![[Pasted image 20260115184003.png]]

D. 3

827. You want to view how frequently RIP is sending updates. What command would you use?

D. `show ip protocols`

In order to view information about the routing protocols running on the router, use the `show ip protocols`.

828. Looking at the figure below, what command was used to create the output?

![[Pasted image 20260115184135.png]]

B. `show ip protocols`

829. Which of the following commands would you use to change the administrative distance with RIP?

```
R1(config)#router rip
R1(config-router)#distance 44
```

830. What is the maximum number of equal cost routes that can exist in the routing table with OSPF?

C. 4

831. What is the default administrative distance of routes learned with OSPF?

C. 110

0 - connected
1 - static
90 - Internal EIGRP
110 - OSPF
120 - RIP 
170 - External EIGRP

832. Which of the following does OSPF use as a metric value?

C. Cost

RIP -> hop count
IGRP -> hop count
EIGRP -> hop count
OSPF -> Cost

833. When the OSPF priorities on all the routers are the same, how is the DR selected?

B. The router with the highest router ID

DR is selected based on the router with the highest router ID.

834. How frequently are hello messages sent with OSPF?

D. Every 10 seconds

835. Which of the following areas is considered the “backbone” area with OSPF?

B. Area 0

836. How frequently does RIPv1 send out routing table updates?

A. Every 30 seconds

837. What is the purpose of a Designated Router (DR) with OSPF?

D. All other routers exchange info with the DR to cut down on bandwidh usage

838. You wish to ensure that router R1 never becomes the DR. What should you do?

A. Set the priority to 0

839. How often are hello messages sent with OSPF?

C. Every 10 seconds

840. What happens when the DR fails?

A. The BDR becomes the DR and an election for a new BDR occurs

841. What are two reasons why two OSPF routers would not be able to create neighbor relationships? (Select two.)

B. Hello and Dead Interval timers are not confiugred the same on both routers
D. The routers are in different areas

842. What is the purpose of the BDR with OSPF?

D. The BDR is used when the DR fails

843. Looking at the commands below, what is the purpose of the “1”?

C. It is the process ID for OSPF

844. Looking at the figure below, what does the /65 indicate in the third route of the routing table?

![[Pasted image 20260115190828.png]]

D. The cost

OSPF uses cost as metric.

845. Your router has the interface configuration shown below. What is the router ID of the router?

```
Loopback0: 12.0.0.1
Loopback1: 14.0.0.1
F0/1: 24.0.0.1
F0/2: 96.0.0.1
```

A. 14.0.0.1

router-id is not set, router will use the highest IP address on any loopback interface as the router ID.
If there is no loopback interfaces, the router will use the highest IP address on a physical interface as the router ID

846. Looking at the figure below, what would the router-ID of the router be?

![[Pasted image 20260115191041.png]]

no loopback interface -> highest ip address on a physical interface

D. 13.0.0.1

847. What commands would you use to configure OSPF and add network 192.168.5.0/24 to area 0? (Select two.)

B. `router ospf 1`
C. `network 192.168.5.0 0.0.0.255 area 0`

848. You want to change the OSPF priority of router R1. What command would you use?

D. `ip ospf priority 20`

849. Which of the following is used to calculate OSPF cost?

A. Bandwidth

850. You want to change the router ID of a Cisco router running OSPF. What command would you use?

B. `R1(config-router)#router-id 13.0.0.1`

851. Hello messages are sent to which of the following addresses?

B. 224.0.0.5

852. Which of the following is used when the successor route fails?

B. Feasible successor route

853. What is the purpose of the feasible successor route?

D. It is a backup route located in the topology table

854. Which of the following is true in regards to the autonomous system (AS) number with EIGRP?

B. It must match on all routers that you want to share routing information

855. What happens when EIGRP has two paths with equal metrics to the same destination?

C. It load balances the two paths.

856. In regards to the commands shown below, what does the 10 represent?

```
NY-R1(config)#router eigrp 10
NY-R1(config-router)#network 11.0.0.0
```

C. The autonomous system

857. The EIGRP algorithm looks at all the routes in the topology table and chooses the best route to a destination. The best route is known as the "".

B. Successor route

858. Which table does the successor route exist in? (select two)

A. Topology table
C. Routing table

859. EIGRP is considered which of the following?

A. A hybrid protocol

860. What is the administrative distance of EIGRP?

Internal: 90
External: 170

D. 90

861. Which table does the feasible successor route exist in?

C. Topology table

862. A router has learned of three routes to the same destination. The first is from RIPv2 with a metric of 6, the second route is from OSPF with a metric of 10, and the third route is from EIGRP with a metric of 2172416. Which route will be used?

D. The EIGRP route (administrative distance of 90)

0 - connected
1 - static
90 - EIGRP (Internal)
110 - OSPF
120 - RIP
170 - EIGRP (External)
255 - unknown

863. What is the benefit of having a feasible successor route with EIGRP?

B. Quicker convergence

864. With EIGRP, which of the following is also known as the routing domain ID?

D. Autonomous system number

865. Which of the following describes a feasible successor?

A. Backup route stored in topology table (backup for successor route)

866. Which of the following are true in regards to EIGRP? (Select two.)

A. Is vendor specific
C. Has a default administrative distance of 90

EIGRP metric: bandwidth, delay
DR and BDR is elected in OSPF

867. Which of the following is true of the EIGRP successor route? (Select two.)

B. Successor routes forwards traffic
C. May have a backup in a feasible successor route

868. Which of the following describes a successor route?

D. Primary route stored in routing table (and topology table)

869. Which of the following does EIGRP use as its metrics by default? (Select two.)

D. Bandwdith
E. Delay

870. Which of the following are true regarding successor routes? (Select three.)

B. They are backed by feasible successor routes.
D. They are used to forward traffic to the destination
E. They are stored in the routing table

871. What does an EIGRP router do if its successor route is active but there is no feasible successor route?

B. The router sends a multicast message to all adjacent neighbors.

872. When looking at the EIGRP topology, what code is used to indicate that there are no changes in the EIGRP topology?

B. Passive

873. What command is used to start EIGRP on your router?

C. `R1(config)#router eigrp 5`

874. You wish to disable auto summarization with EIGRP. What command would you use?

C. `R1(config-router)#no auto-summary`

875. You are configuring router R1 and wish to enable EIGRP on all interfaces for the 11.0.0.0 network and 12.0.0.0 network. What command would you use?

```
R1(config)#router eigrp 10
R1(config-router)#network 11.0.0.0 0.255.255.255
R1(config-router)#network 12.0.0.0 0.255.255.255
```

876. You wish to change the administrative distance of EIGRP. What command would you use?

D. `R1(config-router)#distance eigrp 35 35`

`(config-router)#distance eigrp <internal_distance> <external_distance>`

877. You have configured three routers with the configuration shown below. The routers are not exchanging routing updates. What should you do?

```
NY-R1(config)#router eigrp 10
NY-R1(config-router)#network 11.0.0.0

BOS-R1(config)#router eigrp 20
BOS-R1(config-router)#network 12.0.0.0

TOR-R1(config)#router eigrp 30
TOR-R1(config-router)#network 13.0.0.0
```

B. Confiugre the same AS on all routers

878. You have started the EIGRP process. What command would you use to enable EIGRP for network 192.168.3.64/27?

C. `network 192.168.3.64 0.0.0.31`

879. You have started the EIGRP process. What command would you use to enable EIGRP for network 172.16.16.0/20?

D. `network 172.16.16.0 0.0.15.255`

880. Looking at the figure below, how many packets are waiting to be delivered to the EIGRP neighbor?

![[Pasted image 20260116150356.png]]

B. 0

Q cnt -> how many packets are wating to be delivered to the EIGRP neighbor

881. You have configured EIGRP on router R1 and would like to configure router R2 for the same AS. Looking at the figure below, what AS is R1 using?

![[Pasted image 20260116150526.png]]

A. 10

882. What command was used in the figure below to display the output?

![[Pasted image 20260116150553.png]]

A. `show ip eigrp neighbors`

883. You would like to view the IP addresses of neighboring devices with EIGRP. What command should you use?

D. `show ip eigrp neghbors`

884. Looking at the figure below, which pathway would be used to reach the system with the IP address of 15.22.34.10?

![[Pasted image 20260116150648.png]]

B. R1 would alternate between 14.0.0.1 and 12.0.0.2.

885. You have configured EIGRP and wish to check the queue count and retransmit interval for established adjacency. What command would you use?

A. `show ip eigrp neighbors`

886. Looking at the figure below, how many paths can EIGRP use to the same destination?

![[Pasted image 20260116150911.png]]

A. 4

Maximum path: 4

887. You are troubleshooting EIGRP. What commands could you use to view the autonomous system number? (Select two.)

A. `show ip protocols`
D. `show ip eigrp topology`

888. Looking at the figure below, how long does it take router R1 to send an EIGRP packet to its neighbor and receive a reply?

![[Pasted image 20260116151147.png]]

D. 40 ms

Smooth Round Trip Time (SRTT ) to R1’s neighbor is 40 ms. This is the time it takes for a message to be sent to the neighbor and a reply to come back.

889. What command was used in the figure below to display the output?

![[Pasted image 20260116151824.png]]

A. `show ip protocols`

890. What configuration change is needed at the desktop workstation when activating a second router that has been configured using VRRP?

D. No changes are required (it is transparent for end stations)

891. Which of the following are Cisco HA options for routing? (Select two.)

HSRP, VRRP, GLBP. VRRP is open standard

B. GLBP
D. VRRP

892. What are requirements for VRRP to be used on a network? (Select two.)

B. Two routers
C. Additional virtual IP address

To implement VRRP on your network, you will require a least two routers, each with its own IP address, and one additional virtual IP, which will be used by one master router and will be taken over by the backup router in the event that the master router goes offline.

893. When using GLBP, if the AVG router fails, what router becomes the next AVG? (Select two.)

A. The router with the highest IP address
C. The router with the highest priority

894. What is the advertisement timer for VRRP?

A. 1 seconds

895. What is the major difference between GLBP and either VRRP or HSRP?

C. GLBP perfoms Active/Active load balancing

896. What statements are true about GLBP? (Select two.)

A. Routers share a common virtual IP address
D. Routers have unique virtual MAC address

GLBP routing groups will all share the same IP address, but each will have a separate MAC address. All hosts initially communicate with the AVG, which assigns the session to a router in the group.

897. Which HA option for routers will have the lowest level of impact to users on the network during a failover?

B. GLBP

Since all traffic is already load balanced between several active nodes of the GLBP routing group, device failure in that environment will cause the least user impact. When using VRRP and HSRP (without SSO), there will be a brief interruption in data services (from 3 to 10 seconds) while the secondary router is activated or promoted to the master role.

Ohter protocols (VRRP, HSRP) needs to active backup router. so it needs some time.

898. When working with GLBP, what is the role of the AVG?

C. Manages which data will be processed by which router

899. What is the benefit offered by the SSO addition to HSRP?

A. Preserves forwarding path

900. When using GLBP, two routers are configured with the commands shown in the figure below.

![[Pasted image 20260116161157.png]]

The AVG role is running on Router1 with the highest priority. Router1 is rebooted and the AVG role is transferred to Router2. When will the AVG role move back to Router1?

C. Never.

901. You are configuring VRRP and have used the commands shown in the figure below. It is not working. What changes would you make to the configuration to make it work?

![[Pasted image 20260116181113.png]]

E. All routers require matching VRRP primary and secondary IP address.

```
vrrp group ip ip-address [secondary]
```

After you identify a primary IP address, you can use the vrrp ip command again with the secondary keyword to indicate additional IP addresses supported by this group.

All routers in the VRRP group must be configured with the same primary address and a matching list of secondary addresses for the virtual router. If different primary or secondary addresses are configured, the routers in the VRRP group will not communicate with each other and any misconfigured router will change its state to master.

```
Router1#configure terminal
Router1(config)#interface G0/0/0
Router1(config-if)#ip address 172.16.6.5 255.255.255.0
### primary
Router1(config-if)#vrrp 10 ip 172.16.6.1
### secondary
Router1(config-if)#vrrp 10 ip 172.16.6.2 secondary

Router2#configure terminal
Router2(config)#interface G0/0/0
Router2(config-if)#ip address 172.16.6.5 255.255.255.0
### primary
Router2(config-if)#vrrp 10 ip 172.16.6.1
### secondary
Router2(config-if)#vrrp 10 ip 172.16.6.2 secondary
```

902. Which of the following are valid GLBP debug commands? (Select two.)

B. `debug glbp errors`
D. `debug glbp packets`

The following are valid debug options for glbp: `debug condition glbp`, `debug glbp errors`, `debug glbp events`, `debug glbp packets`, and `debug glbp terse`.

903. You have configured HSRP but want to add SSO. What changes do you need to make to Router2 to complete this configuration?

```
Router1(config)#redundancy
Router1(config-red)#mode sso
Router1(config-red)#exit
Router1(config)#no standby sso
Router1(config)#standby sso
```

904. Members of a GLBP group communicate with hello messages using the IP address of `________` on `____` port `_____`.

C. 224.0.0.102, UDP, 3222

905. The output shown in the figure below is from the show glbp command. You are having problems creating the router group to support HA routing. What options should you examine on the two devices? (Select two.)

![[Pasted image 20260116182728.png]]

A. Group numbers match
E. Virtual IP addresses match

906. How many standby groups can be assigned to a router when using HSRP?

C. 256

907. How does a syslog server separate incoming log information?

D. It dependds on the server.

908. What command will display the settings of logging on your switch or router?

B. `show logging`

909. When logging messages to a syslog server, what severity levels are you able to send to the syslog server?

D. All severity levels

910. You have received the information shown in the figure below on your syslog server. What does this information tell you about your devices on the network?

![[Pasted image 20260116183343.png]]

D. There was a change in the state of the loopback0 interface

911. You have configured your router using the commands shown in the figure below, but your Syslog server is not showing any data. Why not?

![[Pasted image 20260116183429.png]]

C. The syslog server is configured for a non-standard port

By default, syslog services run on UDP port 514. If your syslog server has been configured for a different port, you need to use a command with this format: `logging host 10.1.8.50 transport udp port <port number>`.

912. What is the default port on which a syslog server operates?

B. UDP 514

913. What elements make up a basic syslog message? (Select two.)

A. Time
C. Severity

914. You have been having issues with your router, which becomes unresponsive and will not respond to any of the administrative interfaces. Your only option is to reboot the router with a power reset. What tool can help you to identify the issue that is affecting the router?

C. Syslog

915. What is the name given to an alert generated by an SNMP agent?

D. Trap

916. Which of these are examples of SNMP Trap receivers? (Select two.)

A. HP Openview
C. Nagios

917. What is the name given to a piece of data that can be retrieved via SNMP?

D. OID

918. What is the default port used by SNMP?

D. UDP 161

919. What is name of the documented list of OID for a product?

B. MIB (Management Information Base)

920. What is the purpose of a community name in SNMP?

D. Security password

| version | Auth                    |
| ------- | ----------------------- |
| 1       | community string (name) |
| 2c      | community string (name) |
| 3       | username (noAuthnoPriv) |
| 3       | MD5 or SHA (AuthnoPriv) |
| 4       | MD5 or SHA (AuthPriv)   |

921. What is the major advantage of SNMP v3 over SNMP v2? (Select two.)

A. User authentication
D. Data encryption (MD5, SHA)

922. What are the two standard permission types used by SNMP? (Select two.)

B. Read
C. Write

923. What security step can be used to secure SNMP?

C. Implement access lists

924. What are options for securing SNMP traffic on your network? (Select three.)

A. Using secure community names
B. Restricting access to read-only
D. Implementing access lists

925. What information is required to send NetFlow data to a collector? (Select two.)

A. The IP address of the collector
C. The port the collector is using

926. Which of the following are components in NetFlow data analysis? (Select two.)

C. NetFlow-enabled device
D. NetFlow collector

927. You have configured NetFlow to send data to your collector using the four lines shown in the figure below.

![[Pasted image 20260117092240.png]]

The collector is not showing any data. What is the likely cause of the problem? (Select two.)

A. The collector is configured for a non-default port.
C. The collector is using a different version

```
### set UDP port
(config)#ip flow-export MGMMT_IP UDP_PORT

### set version
(config)#ip flow-export version {1 | 5 | 9}
```

928. What two devices are used on a Frame Relay connection? (Select two.)

B. DTE
D. DCE

929. There are two types of virtual circuits (VC) used on Frame Relay networks. What are they? (Select two.)

A. PVC
D. SVC

930. What type of virtual circuit (VC) destroys call sessions upon completion of transmission?

B. SVC

931. Frame Relay supports which topologies? (Select all that apply.)

A. Full Mesh
C. Partial Mesh
D. Star

932. The DLCI (data-link connection identifier) is a 10-bit number which is used for what purpose?

B. A local identifier between the local router and local frame relay switch

933. What is the purpose of the LMI (Local Management Interface)?

B. Signaling standard between the router (DTE) and Frame Relay Switch (DCE)

934. When dealing with a Full Mesh network with 5 sites, how many links will be required to create the full mesh?

B. 10

935. Which item references the bandwidth that will be available to a Frame Relay service subscriber?

B. CIR

936. Which protocol allows the router to automatically discover the network address of the remote DCE device on the virtual circuit (VC)?

D. Inverse ARP

937. What Frame Relay component is the clock speed of the connection to the Frame Relay cloud?

C. Local Access Rate

938. Rather than using Inverse ARP, DLCIs may be associated with network layer addresses by using which of the following?

C. Static map commands

939. What is the result when traffic on the PVC exceeds the CIR?

B. All excess traffic is marked as discard eligible

940. Which devices need to be configured with matching encapsulation types?

A. Matching DTEs

941. How often does a router send in Inverse ARP to all active DLCIs?

C. 60 seconds

942. How often do routers exchange LMI information with a switch?

B. 10 seconds

943. What is the purpose of a CSU/DSU?

B. It connectes a router to Frame Relay switch.

944. Which topology for Frame Relay has many, but not all sites connected to all other sites?

A. Partial Mesh

945. What Frame Relay component represents the highest average data rate?

A. CIR

946. What are valid LMI types used by Cisco routers? (Select two.)

A. ANSI
D. Q.993A

947. What defines the connection or logical circuit between the local router and the local Frame Relay switch?

C. DLCI

948. You have issued the command `frame-relay map ip 192.168.155.2 155 broadcast`. What does this command do?

C. Identifies the DCLI to be used for traffic being sent to 192.168.155.2

949. When entering the command `frame-relay interface-dlci 155`, what action are you performing?

D. Assigning a DLCI to a local serial sub-interface

950. You would like to implement connections to 5 branch offices over a single physical serial connection. How will you do this?

D. Implement your configuration on sub-interfaces on the serial connection

```
interface Serial1
no ip address
encapsulation frame-relay

interface Serial1.100 point-to-point
ip address 10.1.100.1 255.255.255.0
bandwidth 64
frame-relay interface-dlci 100

interface Serial1.110 point-to-point
ip address 10.1.110.1 255.255.255.0
bandwidth 64
frame-relay interface-dlci 110
```

951. You are connecting 5 sites together with a Frame Relay network. You have had VCs created between each pair of routers that you will be using. If you are using point-to-point sub-interfaces, how may IP subnets will you need to use?

When using point-to-point configuration, you will require one subnet for each VC. You can figure out the VCs for this fully meshed network using the formula 5(5-1)/2 VCs, or 10.

C. 10

952. You have run the command shown in the figure below. Does the output identify an error, and if so, what is the likely issue?

![[Pasted image 20260117145313.png]]

D. There is no apparent issue.

953. You are troubleshooting issues with your Frame Relay connections. Your physical connection is properly in place, but the line protocol is still listed as down. You view the LMI configuration as shown in the figure below. What would you want to verify with your provider?

![[Pasted image 20260117145503.png]]

A. Ensure you are using the correct LMI type

954. You are troubleshooting issues with Frame Relay connections. The results of show interface serial identified the interface as up, but the line protocol as down. You issued the command shown in the figure below to further troubleshoot the issue. What is the likely cause of the problem?

![[Pasted image 20260117145701.png]]

B. The DLCI is not corretly configured

955. What happens when your router does not receive LMI data for 30 seconds?

B. The router will consider the link to be failed

956. You would like to see information about ARP data that is being exchanged with specific DLCIs over the Frame Relay network. What command do you use?

B. `debug frame-relay events`

957. When reviewing the PVC status on your switch, you notice that the LMI data is reporting a status of active. Where would you start looking for your troubleshooting?

B. There is no issue (LMI data is reporting a status of active)

958. What command would you use to view details about all the data that is crossing the Frame Relay connection?

B. `debug frame-relay packet`

959. You are troubleshooting issues with Frame Relay connections. The results of show interface serial are shown in the figure below. What else should be checked during your troubleshooting? (Select two.)

![[Pasted image 20260117150243.png]]

B. Verify the serial cable is correctly connected.
D. Ensure the interface has not been shut down.

960. You are troubleshooting issues with Frame Relay connections. The results of show interface serial is shown in the figure below. What else should be checked during your troubleshooting?

![[Pasted image 20260117150407.png]]

D. Verify you are using the correct DLCI

961. You are troubleshooting issues with Frame Relay connections. The results of show interface serial is shown in the figure below. What else should be checked during your troubleshooting?

![[Pasted image 20260117150513.png]]

B. Verify the serial cable is correctly connected

962. You are able to connect to the Frame Relay cloud, and when you perform show interface serial, you see that your interface and line protocols are both up. You have recently changed the address on the remote network segment and can no longer connect to it. From the command shown in the figure below, what steps could you take to resolve the issue?

![[Pasted image 20260117150617.png]]

C. The Frame Relay map may need to be cleard

If the DLCI or encapsulation were not correct, then the line protocol would be down. Since this connection worked prior to changing the remote address, and since the Frame-Relay map is set to dynamic, it is likely that the map entries are out of date.

You can have the router rebuild the inverse ARP cache by issuing the `clear frame-relay-inarp` command

963. You are having trouble assigning DLCI 1012 to your configuration. What is the likely reason for that?

A. That is a reserved number

There is a set of DLCIs which are reserved, and it includes 1-15 and 1008-1023.

964. When reviewing the PVC status on your switch, you notice that the LMI data is reporting a status of Inactive. Where would you start looking for your troubleshooting?

An Inactive status indicates that your connection to the DCE is proper, so the likely source of the issue is on the remote end of the connection.

C. The remote side of the connection

965. When viewing show frame-relay map output, what information will you see? (Select two.)

A. PVC status
C. Local DLCI

966. Which of the following devices must configure the clock rate on a point-to-point link?

B. DCE

967. Which of the following is the command you would use to set the clock rate on the DCE end of the link?

C. `R1(config-if)#clock rate 64000`

968. You are connecting a Cisco router to a WAN environment that uses routers from different vendors. What protocol should you use as the encapsulation protocol on the serial link?

A. PPP

HDLC is cisco proprietary

969. Which PPP authentication protocol uses a challenge/response algorithm to perform the authentication?

D. CHAP

970. Which of the following layer-2 protocols supports asynchronous communication and authentication?

B. PPP

971. Your router has the following ports:
 
✓ AUI
 
✓ BRI
 
✓ Serial
 
✓ Console
 
✓ Fast Ethernet
 
 
Which port would you use to connect to a T1 line?

C. Serial

972. Which PPP authentication protocol transfers username and passwords in clear text?

C. PAP

973. Which of the following are true in regards to PPP versus HDLC? (Select two.)

B. PPP supports authentication
E. PPP is vendor neutral

974. Which of the following enables CHAP authentication and uses PAP authentication as a backup authentication method?

D. `ppp authentication chap pap`

975. You are looking to configure PPP authentication between your two routers. What should you do? (Choose three.)

B. Set the hostname on each router to a unique name
D. Create a username on each router that matches the hostname of the other router
E. Enable PPP authentication on both routers and specify CHAP as the authentication protocol.

976. Which PPP sub-protocol is responsible for allowing PPP to support multiple network protocols?

B. NCP

977. Which of the following commands would you use to configure your router on a WAN that has non-Cisco routers as well?

```
R1(config-if)#ip address 165.45.1.1 255.255.255.252
R1(config-if)#encapsulation ppp
R1(config-if)#no shutdown
```

978. PPP has a number of sub-protocols that perform different functions. Which protocol is responsible for negotiating authentication options?

NCP -> responsible for allowing PPP to support multiple network protocols

B. LCP

979. Which of the following serial link protocols can implement multilink for load balancing?

A. PPP

980. Which of the following are true in regards to WAN technologies? (Select three.)

B. A router is typically considered a DTE device.
C. A modem is used to connect to an analog line
E. A CSU/DSU is used to connect to a digital line

981. You have entered the command frame-relay map ip 192.168.5.10 100 broadcast on the router. What is the purpose of the broadcast option in the command?

B. Allows routing updates to be forwarded across the frame relay PVC

982. Your manager has been reading about frame relay and asks you for two characteristics of frame relay point-to-point sub-interfaces. What are they? (Select two.)

A. They emulate leased lines
C. They must have a unique subnet

983. Which of the following is true of frame relay DLCI?

C. They are locally significant.

984. What protocol does frame relay use to create a mapping of the layer-3 address to the DLCI?

D. Inverse ARP

985. Which of the following would configure a sub-interface for a point-to-point frame relay PVC using DLCI 102 that maps to IP address 192.168.1.2?

```
interface s0/1/0.102 point-to-point
ip address 192.168.1.2 255.255.255.0
frame-relay interface-dlci 102
```

986. Which of the following commands would you use to map the IP address of 10.0.0.2 to a DLCI of 100?

C. `R1(config-if)#frame-relay map ip 10.0.0.2 100`

987. Using the figure below, what is the protocol being used across the WAN link?

![[Pasted image 20260117153542.png]]

C. HDLC

988. Your Cisco router is having trouble communicating with the 3COM router. While troubleshooting, you use the command in the figure below. What would you do to solve the problem?

![[Pasted image 20260117153725.png]]

D. Change the encapsulation protocol to PPP.

989. You wish to determine if your serial port is the DCE or DTE end of a point-to-point link. What command would you use?

A. `show controllers s0/0`

990. Using the figure below, which of the following is considered layer-2 status information?

![[Pasted image 20260117153858.png]]

A. Line protocol is up

991. You have configured the network show in the figure below. Router R1 cannot communicate with router R2. Why not?

![[Pasted image 20260117153938.png]]

A. The passwords need to be the same

992. The serial port on your router is showing a status of `Serial0/0 is up, line protocol is down.` What would be two reasons for this status? (Select two.)

C. The clock rate is not set on the DCE device
D. The encapsulation protocol is not the same on both ends of the link

993. You use the show interfaces command to determine the status of your serial link. Which of the following identifies a layer-1 issue with the serial port?

A. serial0/3/0 is down, line protocol is down

994. You are troubleshooting why you cannot communicate across the WAN link with serial 0/3/0. Which of the following identifies that the port has been disabled?

D. serial0/3/0 is administratively down, line protocol is down

995. Which of the following identifies a layer-2 issue with your serial port?

B. serial0/3/0 is up, line protocol is down

996. You are troubleshooting communication issues and use the show interfaces command. Which of the following statuses on your serial port indicate it is operational?

C. serial0/3/0 is up, line protocol is up

997. You are looking at options for connecting two autonomous systems together. What routing protocol should you use?

D. BGP

The Border Gateway Protocol (BGP) is used to connect two different autonomous systems together.

998. You are troubleshooting a frame relay link to non-Cisco frame relay routers. You want to verify whether you are using the Cisco encapsulation protocol or the IETF encapsulation protocol. What command would you use?

D. `show frame-relay map`

999. You are troubleshooting your frame relay link and use the show frame-relay pvc command. The results come back with a `“PVC STATUS=INACTIVE”`. What is the cause of the problem?


When viewing the PVC status, you can have any of the following three status messages:

✓ Active: The PVC is operational and all is good!
✓ Inactive: There is a problem on the remote end of the PVC.
✓ Deleted: There is a problem with the PVC at the provider. Contact the provider to verify the link.

A. Locally you are configured properly, but the other end of the link is misconfigured

1000. Which of the following commands would you use to troubleshoot authentication with PPP?

D. `debug ppp authentication`

1001. Looking at the results of the show frame-relay map command shown in the figure below, what is the meaning of dynamic in the command output?

![[Pasted image 20260117154602.png]]

C. The DLCI mapping was learned through Inverse ARP
