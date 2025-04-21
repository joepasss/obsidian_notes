* functions of the OSI Reference Model layers
* encapsulation and de-encapsuation

## Layers of the OSI Reference Model

in the early 1980s, the international Organization for Standardization (ISO) defined a starndard for manufacturers of networking components that would enable these networking components to communicate in dissimilar environments. The product of the standard is demonstrated in the seven layers of the OSI Reference Model, *application, presentation, session, transport, network, data link, and physical*

![[스크린샷 2025-04-18 02.42.34.png]]

**Upper layers**
: application, presentation, session
* typically controlled by the application software.
* normally not controlled or modified by a network administrator.

**lower layers**
: Transport, Network, data link, physical

*Switch -> Layer 2, 3*
*Router -> Layer 3*

Each layer of the OSI Model is responsible for communicating with the layers directly above and below it, receiving data from or passing data to its neighboring layers

Ex) the presentation layer (layer 6) will receive information from the application layer (layer 7), format it appropriately, and then pass it to the session layer (layer 5) *the presentation layer will never deal directly with the transport, network, data link and Physical layers*

**Device A**
Application Layer -> 패킷 생성
Physical Layer 에서 다른 기기로 패킷 전송

**Device B**
Device A 에서 보내온 패킷을 Physical Layer 에서 receive
Physical Layer 에서 Application Layer 까지 보낸 다음 application layer 에서 받아온 패킷 processing

![[스크린샷 2025-04-18 03.16.49.png]]

whatever action is done at one layer of a sending computer is undone at the same layer on the receiving computer.

EX) if the presentation layer compresses the information on the sending computer, then the data is uncompressed on the receiving computer.

> Computer1 and Server1 are exchanging data on the network.
> Computer1 is the sending computer, and Server1 is the receiving computer
> 
> The data exchange starts with Computer1 sending a request to Server1 in the application layer.
> At Server1, the data moves back up through the layers to the application layer, which passes the data to the appropriate application or service on the system

![[스크린샷 2025-04-18 03.20.28.png]]

*at layer 4, the data is called a **segment***
*at layer 3, the data is called a **packet***
*at layer 2, the data is called a **frame***

### Layer 7: The Application Layer

The application layer running on the sending system (Computer1) is responsible for initiating the actual request. This could be any type of networking request (HTTP, SMTP, FTP, SMB ...). On the receiving system, the application layer would be responsible for passing the request to the appropriate application or service on that system.

we will assume that you are sitting at Computer1 and you have typed the address of Server1 into your web browser to create an HTTP request.

> **Some of protocols used in application layer**
> HTTP, telnet, Secure Shell (SSH), File Transfer Protocol (FTP), Post Office Protocol 3 (POP3), Simple Mail Transfer Protocol(SMTP) ...

application layer provides the protocols and services that enable systems to initiate a request (sending system) or service a request (receving system)

### Layer 6: The Presentation Layer

After the request is made, the application layer passes the data down to the presentation layer, where it is formatted so that the data (or request) can be interpreted by the receiving system.

When the presentation layer receives data from the application layer, it makes sure the data is in the proper format if it is not, the presentation layer converts the data accordingly. On the receiving system, when the presentation layer receives network data from the session layer, it makes sure the data is in the proper format and once again converts it if it is not.

Formatting functions that could occur at the sending system's presentation layer include compression, encryption, and ensuring that the character code set can be interpreted on the other side.

EX) if we choose to compress our data from the application that we are using, the application layer will pass that request to the presentation layer, but the presentation layer does the compression. At the receiving end, this data must be decompressed so that it can be read. When the data reaches the presentation layer of the receiving computer, it will decompress the data and pass it up to the application layer.

=> 포멧 변환 (Data Translation)
=> 압축/해제 (Compression/Decompression)
=> 암호화/복호화 (Encryption/Decryption)


### Layer 5: The Session Layer

The session layer manages the dialog between computers by establishing, managing, and terminating communications between them. (TCP, UDP?)

When session is established, three distinct phases are involved. In the establishment phase, the requestor initiates the service and the rules for communication between the two systems. These rules could include such things as who transmits and when, as well as howe much are like the etiquette of the conversation. Once the rules are established, the data transfer phase begins. Both sides know how to talk to each other, what are the most efficient methods to use, and how to detect errors, all because of the rules defined in the first phase. Finally, termination occurs when the session is complete, and communication ends in an orderly fashion

1. session established
	1. (the requestor) initiates the service and the rules for communication between two systems
	2. data transfer phase
2. session terminate (when the session is complete)
3. communication ends in an orderly fashion


=> Responsible for RPCs (Remote Procedure Calls), NFS (Network File System)

### Layer 4: The Transport Layer

The transport layer handles functions such as reliable and unreliable delivery of the data. For reliable transport protocols, the transport layer works hard to ensure reliable delivery of data to its destinations. On the sending system, the transport layer is responsible for breaking the data into smaller parts, called *segments* so that if retransmission is required, only the missing segments will be sent

Missing segments are detected when the transport layer receives acknowledgments (ACKs) from the remote system upon receiving the packets, At the receiving system, the transport layer is responsible for opening all of the packets and reconstructing the original message.

1. Send data
2. receive data (ACK), check missing segments if there is missing segments -> request missing segment to sender, if is not reconstruct the original message

Another function of the transport layer is segment sequencing. Sequencing is a connection-oriented service that takes segments that are received out of order and resequences them in the right order

=> 도착한 segments 를 순서에 맞게 재조립 (sequencing)

The transport layer also enables the option of specifying a "service address," known as a *port address* The port address enables the services or applications that are running on the systems to specify what application the request came from and what application the request is headed for by having each application use a unique port address on the system.

=> port address (SSH 연결할때는 22번 포트 사용하고 이런거) specify 해줌

TCP -> reliable delivery, UDP -> unreliable delivery

### Connection-Oriented Communication and Connectionless Communication

Connection Oriented communication ensures reliable delivery of data from the sender to the receiver. When establishing connection services, this form of communication requires that some sort of handshaking function be performed at the beginning of a communication session.

1. two computer determine the rules for communication
	1. how much data to send at one time
	2. which port to use

Handshaking also determine the rules for communication & determine the proper way to terminate session

A session is reliable dialog between two computers. A session is like a telephone call: you set up a telephone call by dialing (handshaking), then speak to the other person (exchange data), say "Goodbye," and finally hang up when finished.

Connectionless communication is a form of communication in which the sending system does not "introduce" itself it just fires off the data. Also, the destination computer does not notify the source when the information is received. (unreliable)

connectionless communication can be faster than connection-oriented communication because the overhead of managing the session is not there, and after the information is sent, there is no second step to ensure that it was received properly

**main functions of the transport layer**

* it sets up, maintains, and tears down a connection between two devices or systems.
* it can provide for the reliable or unreliable delivery of data across this connection.
* it breaks up data into smaller, more manageable segments.
* it multiplexes connections, enabling multiple applications to send and receive data simultaneously on the same networking device.
* it can implement flow control through ready/not ready signals or windowing to ensure that one component doesn't overflow another with too much data on a connection. Both of these methods are used to avoid congestion and typically use buffering

### Segmentation

Another function of the transport layer is to set up, maintain, and tear down connections for the session layer that is, it handles the actual mechanics for the connection. The information transferred between networking devices at the transport layer is divided into segments.

Segmentation is necessary to break up large amounts of data into more manageable sizes that the network can commodate. A good analogy of this process is "it's easier to pour pebbles down a pipe than giant boulders."

### Connection Multiplexing

Because multiple connections may be established from one component to another component or to multiple components, some type of multiplexing function is needed to differentiate between data traversing the various connections, This ensures that the transport layer can send data from a particular application to the correct destination and application, and, when receiving data from a destination, the transport layer can get the data to the right local application. To accomplish connection multiplexing, a unique port number is assigned to each application

-> multiple connection 이 생성될 수 있음 => multiplexing 기능 필요함 (다양한 연결점에 데이터 전달해야함) 이 multiplexing이 transport layer 에서 특정 application 으로 데이터를 전송하는거를 ensure 해줌
multiplexing 을 하려고하면은 unique 한 port number가 각 application 에 assign 되어야 멀티플렉싱기능을 사용할 수 있음

### Flow Control

Another function of the transport layer is to provide optional flow control.

Flow control is used to ensure that networking components don't send too much information to the destination, overflowing its receiving buffer space and causing it to drop some of the transmitted information. Overflow is not good because the source will have to resend all the information that was dropped.

The transport layer can use two basic flow control methods:
* Ready/not ready signals
* Windowing

transport 레이어에서 flow control 기능을 제공할 수 있음.
버퍼를 설정해서 이친구 사이즈보다 큰 데이터가 들어오면, overflow 가 나는데 overflow 가 나면 오버플로우된 데이터를 다 drop 한 뒤에 sender 에서 drop 된 데이터를 다시 다 보내줘야함

### Ready/Not Ready Signals

When the destination receives more traffic than it can handle, it can send a *not ready* signal to the source, indicating that the source should stop transmitting data.

When the destination has a chance to catch up and process the source's data, the destination responds back to the source with a ready signal. Upon receiving the ready signal, the source can resume sending data

목적지에서 감당할 수 있는 트래픽보다 더 많이 트래픽이 들어오면 source 에 not ready 시그널을 보냄 (source 에다가 데이터 전송 중지하라는 신호) 목적지가 source's data를 따라잡으면, ready 시그널을 source 에 보내서 전송 재개할 수 있게함

Two problems are associated with the use of ready/not ready signals to implement flow control.
the destination may respond to the source with a not ready signal when its buffer fills up. While this message is on its way to the source, the source is still sending information to the destination is ready to receive more information, it must first send a ready signal to the source, which must receive it before more information can be sent. This causes a delay in the tranfer of information. Because of these two ineffciencies with ready/not ready signals, they are not commonly used to implement flow control. Sometimes this process is referred to as stop/start where you stop transmitting for a period and then start retransmitting

flow control 로 ready/not ready 를 사용할 때 문제점,
1. 버퍼가 꽉 찬 뒤에도 계속 데이터가 옴
	* not ready 신호가 source 까지 전달되는데는 시간이 걸리기 때문에, 그 동안 Source 에서는 계속 데이터를 전송하고 있음, 이때 추가로 오는 데이터는 목적지 버퍼에 공간이 더 이상 없기 때문에 결국 데이터를 drop 해야 함
2. ready 신호로 인한 전송 지연
	* ready 신호를 source 에 보내는데 또 시간이 걸리기 때문에, 그 동안 전송을 재개하기까지 delay 가 생김

### Windowing

Windowing is much more sophisticated method of flow control than using ready/not ready signals.

With windowing. a window size is defined that specifies how much data (segments) can be sent before the source has to wait for an ACK from destination. Once the ACK is received, the source can send the next batch of data (up to the maximum defined in the window size)

*windowing*

window size = 데이터(segment)를 얼마나 보낼 수 있는지
Source 에서 window size 만큼 데이터를 전송한 후에 ACK 를 받을 때 까지 기다린 후, ACK 응답을 받으면 다음 window size 만큼의 데이터를 보낼 수 있음

Windowing accomplishes two things: First, flow control is enforced, based on the window size. In many protocol implements, the window size is dynamically negotiated up front and be renegotiated during the lifetime of the connection. This ensures that the most optimal window size is used to send data without having the destination drop anything. Second, through the windowing process, the destination tells the source what was received. This indicates to the source whether any data was lost along the way to the destination and enables the source to resend any missing information. This provides reliability for a connection as well as better effciency than ready/not ready signals, Because of these advantages, most connection-oriented tranport protocols, such as TCP/IP's TCP, use windowing to implement flow control

flow control 이 필수적인 상황일 때, window size 에 기반을 두는데, 이 window size 는 바뀔 수 있음. 그래서 가장 효율적인 window 사이즈를 connection lifetime 어디에서나 설정 가능함
windowing process 증에, destination 이 source 에 어떤 데이터가 들어왔는지 말해줌. 이 정보로 source 가 무슨 데이터가 loss 되었는지 확인 할 수 있음 -> reliability 제공
이러한 이유 때문에 대부분의 connection-oriented transport protocols (TCP/IP's TCP 등)이 windowing 을 flow control 에 사용함

The window size chosen for a connection impacts its efficiency and throughput in defining how many segments (or bytes) can be sent before the source has to wait for and ACK

![[스크린샷 2025-04-19 23.38.30.png]]

![[스크린샷 2025-04-19 23.45.05.png]]

one segment gets lost on its way to the destination, this example, the window size negotiated is 3. PC-A sends its first three segments, which are successfully received by PC-B. PC-B acknowledges the next segment it expects, which is 4. When PC-A receives this ACK, it sends segments 4, 5 and 6. For some reason, segment 4 becomes lost and never reaches the destination, but segments 5 and 6 do arrive. the detination is keeping track of what was received: 1, 2, 3, 5, and 6, in this example, the destination sends back an ACK of 4, indicating that segment 4 is expected next.

At this point, how PC-A reacts depends on the transport layer protocol that is used. Here are some possible options:

* PC-A understands that only segment 4 was lost and therefore resends segment 4. It then sends segments 7 and 8, filling up the window size.
* PC-A doesn't understand what was or wasn't received, so it sends three segments starting at segment 4, indicated by PC-B.

if two segments are lost, the first option listed won't work unless the destination can send a list of lost segments. Therefore, most protocol stacks that use windowing will implement the second option. Given this behavior, the size of the window can affect your performance. You would normally think that a window size of 100 would be very efficient; however, if the very first segment is lost, some protocols will have all 100 segments resent! As mentioned earlier, most protocol stacks use a window size that is negotiated up front and can be renegotiated at any time. Therefore, if a connection is experiencing a high number of errors, the window size can be dropped to a smaller value to increase efficiency. And once these errors disappear or drop down to a lower rate, the window size can be increased to maximize the connnection's throughput.

What makes this situation even more complicated is that the window sizes on the source and destination devices can be different. For instance, PC-A might have a window size of 3, while PC-B has a window size of 10. In this example, PC-A is allowed to send ten segments to PC-B before waiting for and acknowledgment, while PC-B is allowed t osend only three segments to PC-A.

### Layer 3: The Network Layer

responsible for managing logical addressing information in the packets and the delivery, or routing, of these packets by using information stored in a routing table. The routing table is a list of available destination that are stored in memory on the routers.

The network layer is responsible for working with logical addresses. Logical addresses uniquely identify a system on the network and, at the same time, identify the network that the system resides on. This is unlike a Media Access Control (MAC) address because a MAC address just gives the system a unique address and does not specify or imply what network the system lives on. The logical address is used by network-layer protocols to deliver the packets to the correct network.

In our example, the request is comming from a web browser and is destined for a web server, both of which are applications that run on TCP/IP. At this point, the network layer will add the source address (the IP address of the sending system) and the destination address (the IP address of the destination system) to the packet so that the receiving system will know where the packet came from.

**IP address = layer 3 address(logical address) != MAC address**

The network layer is responsible for four main functions:

* Defines logical address used at layer 3
* Finds paths, based on the network numbers of logical address, to reach destination components
* Connects different data link layer types together, such as Ethernet, Fiber Distibuted Data Interface (FDDI), serial, and Token Ring
* Defines segmentation via the use of packets to transport information

To move information between devices that have different network numbers, a router is used. Routers use information in the logical address to make intelligent decisions about how to reach a destination. when a router receives a packet, it compares the destination address (from the layer 3 header of the packet, which is the destination IP address) in the packet to its routing table to determine whether the router knows how to send data to that destination network.

*Router -> layer 3 device*
*Switch -> Layer 2 device (or if it has extended functions it treated as layer 3 device)*

### Layer 2: The Data Link Layer

At the data link layer, the data is converted from a packet to a pattern of electrical bit signals that will be used to send the data across the communication medium. On the receiving system, the electrical signals will be converted to packets by the data link layer and then passed up to the network layer for further processing. The data link layer is divided into two sublayers:

1. Logical Link Control (LLC)
	* This is responsible for error correction and control functions.
2. Media Access Control (MAC)
	* This determines the physical addressing of the hosts.
	* It also determines how the host places traffic on the medium
	* ex) CSAM/CD vs Token passing

*Ethernet and Token Ring network architectures are defined at layer 2 of the OSI model*

The MAC sublayer maintains physical device address (MAC address) for communicating with other devices on the networks. These physical address are burned into the network cards and consititude the low-level addresses used to determine the source and destination of the network traffic.

*MAC address is used for communication on the local network segment*
*IP address is used for communication on different networks*

*MAC address is the physical address assigned to the network card and is known as a layer 2 address. The MAC address is a 48-bit value displayed in hexadecimal format. (ex, 00-02-3F-6B-25-13)*

once the sending system's network layer appends the IP address information, the data link layer will append the MAC address information for the sending and receiving systems. This layer will also prepare the data for the wire by converting the packets to binary signals. On the receiving system, the data link layer will convert the signals passed to it by  the physical layer to data and then pass the packets to the network layer for further processing.

### Layer 2 Frames

The data link layer defines how a networking component accesses the media to which it is connected, and it also defines the media's frame type and transmission method. The frame includes the fields and components the data link layer uses to communicate with devices on the same wire or layer 2 topology.

This communication occurs only for components on the same data link layer media type (or same piece of wire), within the same network segment. To traverse layer 2 protocols, Ethernet to Token Ring, for instance, a router is typically used.

Examples of layer 2 protocols and standards for local area network (LAN) connections include Institude of Electrical and Electronic Engineers (IEEE) 802.2, 802.3, and 802.5: Ehternet II; and FDDI. Example of layer 2 WAN protocols and techniques include Asynchronous Transfer Mode (ATM), Frame Relay, High-Level Data Link Control (HDLC), Point-to-Point Protocol (PPP), Synchronous Data Link Control (SDLC), Serial Line Internet Protocol (SLIP), and X.25

The data link layer is also responsible for defining the format of layer 2 frames as well as the mechanics of how devices communicate with each other over the physical layer. The data link layer is responsible for the following:

* Defining the MAC or hardware addresses
* Defining the physical or hardware topology for connections
* Defining how the network layer protocol is encapsulated in the data link layer frame
* Providing both connectionless and connection-oriented services
* Verifying the checksum of the received frame to ensure it is valid (if it is invalid, it is discardes)

### Data Link Layer Addressing

the data link layer uses MAC, or hardware, addresses for communication. For LAN communications, each machine on the same network segment or topology needs a unique MAC address. A MAC address is 48 bits in length and is represented as a hexadecimal number, 12 characters in length. To make it easier to read, the MAC address is represented in a dotted hexadecimal format, (FFFF,FFFF,FFFF) it is also common to see MAC addresses formatted in (FF:FF:FF:FF:FF:FF). Since the MAC address uses hexadecimal numbers, the values used range from 0 to 9 and A to F, for a total of 16 values for a single digit.

Each manufacturer of network cards is assigned a unique 24-bit vendor ID, which is then used as the first 24 bits of a MAC address for any network cards created by that vendor. Each vendor has one or more unique vendor IDs, with each making up the first half of a MAC address. (OUI, organizationally unique identifier)

*CISCO OUI= 0000.0C*
*CISCO MAC ADDR = 0000.0Cxx.xxxx*

Some devices enable you to change this hardware address, while others do not.

You can have the same MAC address in different broadcast domains or virtual LANs, but not on the same network segment

### Communication Types

Different types of communication can occur on the network, and each of these uses a specific method of addressing to identify who a message is for.

Each data link layer frame contains two MAC addresses:

1. a source MAC address of the machine creating the frame
2. destination MAC address for the device or devices intended to receive the frame

Three general types of communication

1. Unicast
	* communication to a single device on a segment
2. Broadcast
	* communication to every device on a segment
3. Multicast
	* communication to a group of devices on a segment

### Unicast

A frame with a destination unicast MAC address is intended for only one network component on a segment.

> PC-A creates an Ethernet frame with a destination MAC address that contains PC-C's address.
> 
> When PC-A places this data link layer frame on the wire, all the devices on the segment receive it, but all systems except PC-C discard the frame because it is not destined for  them.
> 
> This gives us one-on-one communication with unicast

### Multicast

Unlike a unicast address, multicast communication represents communication to a group of devices on a segment. The multicast group can contain no devices up to every device on a segment.

One of the interesting things about multicasting is that the membership of a group is dynamic devices can join and leave the multicast group

### Broadcast

A broadcast message is sent to all systems on the network. PC-A puts a broadcast address in the destination field of the data link layer frame. The layer 2 representation of a broadcast address is FF:FF:FF:FF:FF:FF(broadcast address). This frame is then placed on the wire.

![[스크린샷 2025-04-20 07.30.43.png]]

### Layer 1 The Physical Layer

The bottom layer of the OSI hierarchy is concerned only with moving bits of data on and off the network medium. This includes the physical topology (or structure) of the network, the electrical and physical aspects of the medium used, and the encoding and timing of bit transmission and reception.

once the network layer has appended the logical addresses (IP) and passed the data to the data link layer, where the MAC addresses were appended and the data was converted to electrical signals, the data is then passed to the physical layer so that it can be released on the communication medium. On the receving system, the physical layer will pick up the data off the wire and pass it to the data link layer, where it will ensure that the signal is destined for that system by reading the destination MAC address.

The physical layer is responsible for the physical mechanics of a network connection, which include the following:

* The type of interface used on the networking device
* The type of cable used for connecting devices
* The connectors used on each end of the cable
* The pin patterns used for each of the connections on the cable
* The encoding of a message on a signal by converting binary digits to a physical representation based on the media type (copper, fiber or radio wave)

Type type of interface, or NIC, can be a physical card that you put into a computer, such as Gigabit Ethernet card, or a fixed interface on a router, such as a Gigabit Ethernet port

The physical layer is also responsible for how binary information is converted to a physical layer signal and vice versa. For example, if the cable uses copper as a transport medium, the physical layer defines how binary 1s and 0s are converted into electrical signals by using different voltage levels. If the cable uses fiber, the physical layer defines how 1s and 0s are represented using a light-emitting diode (LED, MMF) or laser (SMF) with different light frequencies.

### Devices

![[스크린샷 2025-04-20 08.12.11.png]]

### Review

![[스크린샷 2025-04-20 23.53.48.png]]

* Responsible for the logical addressing and delivery of the packets
	* Network
* Responsible for formatting the message
	* Presentation
* Responsible for physical addressing (MAC) and converting the data packets to electrical signals
	* Datalink
* Responsible for creating, managing, and ending a dialog
	* session
* Responsible for reliable deilvery, sequencing, and breaking the message into packets
	* transport
* Responsible for placing or removing the signal on and off the wire
	* Physical
* Responsible for initiating or receiving the network request
	* Application


| layer | name         | desc                                                                                                                    |
| ----- | ------------ | ----------------------------------------------------------------------------------------------------------------------- |
| 7     | application  | HTTP 등 프로토몰 이용, network request initiating + response                                                                   |
| 6     | presentation | application 에서 받은 데이터를 Formatting, 압축, 압축해제, Encryption                                                                 |
| 5     | session      | communication 생성 & 파괴 (establish -> init service + rules -> data transfer -> terminate)                                 |
| 4     | transport    | TCP(reliable), UDP(unreliable), port 주소로 multiflexing + flow control + Segmentation                                     |
| 3     | network      | IP 주소(논리적 주소) 이용, Router                                                                                                |
| 2     | datalink     | MAC 주소 (물리 주소) 이용, Switch, LLC(에러 수정, control), segement 에서 frame 으로 전환, unicast, multicast, broadcast (FF:FF:FF:FF:FF) |
| 1     | physical     | 데이터를 전기 신호로 전환                                                                                                          |

## Encapsulation and De-encapsulation

`data encapsulation` = data is passed down the seven layers of the OS model, header information is added to the message.

when the information reaches layer 4 of the OSI model, a layer 4 header is added, which contains protocol information for that layer, such as the port number.

![[스크린샷 2025-04-21 00.44.42.png]]

**SENDING SYSTEM**

계층 7에서 시작, 한 단계 내려갈 때 마다 header 추가 (encapsuation)

**RECEVING SYSTEM**

계층 1에서 시작, 한 단계 올라갈 때 마다 header 제거 (de-encapsulation)

protocol data unit (PDU) describes as it passes through each layer of the OSI model. At each layer, a new header is added to the data.
For instance, as data is passed from the session layer to the transport layer, the transport layer encapsulates the data PDU by adding a layer 4 header. At this point the data is now referred to as a segment. As the PDU information is passed down, each layer add its own header, and at that point the data uses a different term in the model. For example, once the layer 3 header is added, the data is no longer termed a segment; it is now considered a packet. Likewise, once layer 2 header information is added, the data is known as a frame

![[스크린샷 2025-04-21 03.01.47.png]]


**Encapsulation and de-encapsulation processes**

![[스크린샷 2025-04-21 03.02.34.png]]

### Going Down the Protocol Stack

PC-A sends data to PC-B. In this example, assume that the data link layer is Ethernet and the physical layer is copper.

The first thing that occurs on PC-A is that the user, The application (or operating system) at the session layer then determines whether or not the data's intended destination is local to this computer or a remote location. In this instance, the user is sending the information to PC-B We'll assume that the user is executing a telnet connection.

The session layer determines that this location is remote and has the transport layer deliver the information. A telnet connection uses TCP/IP and reliable connections (TCP) at the transport layer, which encapsulates the dat from the higher layers into a segment. With TCP, only a header is added. The segment contains such information as the source and destination port number.

> Based on RFC standards, the TCP or UDP source port number really should be above 49,151, but not all operating systems follow this standard verbatim in many cases, the source port number will be above 1023.

The transport layer passes the segment down to the network layer, which encapsulates the segment into a packet. The packet adds only a header, which contains layer 3 logical addressing information (source and destination address) as well as other information, such as the upper-layer protocol that created this information. In this example, TCP created this information, so this fact is noted in the packet header, and PC-A places its IP address as the source address in the packet and PC-B's IP address as the destination. This helps the destination, at the network layer, to determine whether the packet is for itself and which upper-layer process should handle the encapsulated segment. In the TCP/IP protocol stack, the terms packet and datagram are used interchangeable to describe this PDU.

The network layer then passes the packet down to the data link layer. The data link layer encapsulates the packet into a frame by adding both a header and a trailer. This example uses Ethernet as the data link layer medium, The important components placed in the Ethernet frame header are the source and destination MAC addresses, as well as a field checksum sequence (FCS) value so that the destination can determine whether the frame is valid or corrupted when it is received. In this example, PC-A places its MAC addresss in the frame in the source field and PC-B's MAC address in the destination field.

The data link layer frame is then passed down to the physical layer. from a computer's perspective, the data is just a bunch of binary values, 1s and 0s, or bits. The physical layer converts these bits into a physical property based on the cable or connection type

### Going Up the Protocol Stack

assume PC-A and PC-B are on the same piece of copper.

Once the destination PC receives the physical layer signals, the physical layer translates the voltage levels back to their binary representation and passes these bit values up to the data link layer.

The data link layer reassembles the bit values into the original data link frame (Ethernet). The network adapter examines the FCS to make sure the frame is valid and examines the destination MAC address to ensure that the Ethernet frame is meant for itself. If the destination MAC address doesn't match its own MAC address, or it is not a multicast or broadcast address, the NIC drops the frame. Otherwise, the NIC processes the frame. In this case, the NIC sees that the encapsulated packet is a TCP/IP packet, so it stripes off (de-encapsulates) the Ethernet frame information and passes the packet up to the TCP/IP protocol stack at the network layer

The network layer then examines the logical destination address in the packet header. If the desintaion logical address doesn't match its own address or is not a multicast or broadcast address, the network layer drops the packet. If the logical address matches, then the destination's network layer examines the protocol information in the packet header to determine which protocol should handle the packet. In this example, the logical address  matches and the protocol is defined as TCP. Therefore, the network layer strips off the packet information and passes the encapsulated segment up to the TCP protocol at the transport layer.

Upon receiving the segment, the transport layer can perform many functions, depending on whether this is a reliable (TCP) or unreliable (UDP) connection. This discussion focuses on the multiplexing function of the transport layer. In this instance, the transport layer examines the destination port number in the segment header. In our example , the user from PC-A was using telent to transmit information to PC-B, so the destination port number is 23. The transport layer examines this port number and realizes that the encapsulated data needs to be forwarded to the telnet application. If it does, the transport layer strips off the segment information and passes the encapsulated data to the telnet application. If this is a new connection, a new telnet process is started up by the operating system.

### Layers and Communication

if the source and destination are on different segments, separated by other networking devices =>

![[스크린샷 2025-04-21 05.52.35.png]]

---

### Questions

1.  The OSI Reference Model provides for all of the following except which one?
	A. Defines the process for connecting two layers together, promoting interoperability between vendors
	B. Allows vendors to compartmentalize their design efforts to fit a modular design, which eases implementations and simplifies troubleshooting
	C. Separates a complex function into simpler components
	D. Defines eight layers common to all networking protocols
	E. Provides a teaching tool to help network administrators understand the communication process used between networking components

D => 7 layer

2.  Put the following OSI Reference Model layers in the correct order, from high to low: (a) session, (b) presentation, (c) physical, (d) data link, (e) network, (f) application, (g) transport.
	A. c, d, e, g, a, b, f
	B. f, a, b, g, d, e, c
	C. f, b, g, a, e, d, c
	D. f, b, a, g, e, d, c

D => physical -> presentation -> session -> transport -> network -> data link -> physical (f, b, g, e, d, c)

3.  The __________ layer of the OSI model provides physical addressing.
	A. Transport
	B. Network
	C. Data link
	D. Physical

C => physical addressing (MAC 주소) 데이터 링크 레이어에서 담당

Network layer 에서 IP (logical address) 담당
Transport layer 에서 port 담당
Physical layer는 Bits 를 voltage 로 변경해서 전달해줌

4.  Sue has noticed that the first half of the MAC address (24 bits) is the same for all the network cards on your computers. What does the first 24 bits of a MAC address represent?
	A. AUI
	B. NIA
	C. RTP
	D. OUI

앞 24비트 = OUI (제조사 ID, Vendor ID)
뒤 24비트 = NIC (디바이스 고유 번호)

vender id (모르겠음)

5.  The network layer handles all of the following problems except __________.
	A. Broadcast problems
	B. Conversion between media types
	C. Hierarchy through the use of physical addresses
	D. Splitting collision domains into smaller ones

A => Data link
B => ?
C => MAC 주소 사용 = Data link
D => ?


6.  __________ are used to provide a reliable connection.
	A. Ready/not ready signals
	B. Sequence numbers and acknowledgments
	C. Windows
	D. Ready/not ready signals and windowing

A, C, D = flow control
정답 = B

7.  Connection multiplexing is done through the use of a __________ number.
	A. Socket
	B. Hardware
	C. Network
	D. Session

multiplexing 은 transport layer 에서 담당. 세션 갯수랑 관련있나?
정답 = D (잘 모르곘음)

1.  Match the device with the layer of the OSI Reference Model that the device functions at.

![[스크린샷 2025-04-21 07.44.44.png]]

Repeater, Hub = Layer 1
Switch = Layer 2
Router = Layer 3

2.  Match the PDU name with the OSI Reference Model layer at which it is used.

![[스크린샷 2025-04-21 07.44.19.png]]

Layer 1 (physical) = Bits
Layer 2 (Data link) = Frames
Layer 3 (Network) = packet
Layer 4 (transport) = segments

layer 5, 6, 7 = data