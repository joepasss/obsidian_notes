## Chapter 1: Network Fundamentals

local area networks(LAN)
wide area networks(WAN)

## Introduction to Networks

A `network` includes all of the components (hardware and software) involved in connecting computers and applications across small and large distances.

Networks are used to share information and services with users to help them increase their productivity.

### Network Characteristics

To design a successful network solution, you need to be familiar with the organization's needs and goals.

* Cost
	* Includes the cost of the network components, their installation, and their ongoing maintenance.
* Security
	* Includes the protection of the network components and the data they contain and/or the data transmitted between them
* Speed
	* Includes how fast data is transmitted between network endpoints(the data rate)
* Topology
	* Describes the physical cabling layout and the logical way data moves between components
* Scalability
	* Defines how well the network can adapt to new growth, including new users, applciations, and network components
* Reliability
	* Defines the ability of network components to provide the appropriate levels of support and the required connectivity levels
	* The mean time between failures (MTBF) is a measurement commonly used to indicate the ilkelihood of a component failing.
* Availability
	* Measures the likelihood of the network being available to the users, where downtime occurs when the network is not available because of an outage or scheduled maintenance.
	* Availability is typically measured in a percentage based on the number of minutes that exist in a year. Therefore, `uptime` would be the number of minutes the network is available divided by the number of minutes in a year.

### Networking Components

`Applications`, the software that enables users to perform various tasks,

make up a key component of networking. Many applications are `network-aware`, which means they enable you to access and use resources that are not stored directly on your local computer.

While the number of networking applications ranges in the thousands, some of the more common networking applications in use in most organizations include e-mail applications for sending and receiving e-mail, File transfer protocol (FTP) applications for transferring files, and web applications for proving a graphical respresentation of information.

`Protocols` are used to enable systems to talk to one another on the network.

Some protocols are `open standard`, meaning that many vendors can create applications that can interoperate with other applications using that protocol.

Other protocols are `proprietary`, meaning that they work only with other applications created by a particular vendor.

**some common internet protocols**

* SMTP
	* Simple Mail Transfer Protocol
* POP3
	* Post Office Protocol 3
	* for downloading e-mail from an e-mail server to local system so that can read the e-mail
* IMAP4
	* Internet Message Access Protocol version 4
	* used to read e-mail
	* supports additional capabilities and folder support over POP3
* FTP
	* File Transfer Protocol
	* for transferring files across the internet
* HTTP
	* Hyprtext Transfer Protocol
	* used to deliver web pages from a web server to a web browser

Some applications require little bandwidth, while others require a lot.
Some applications operate in real-time, Some operate interactively, and some operate in a batch mode(사용자와의 상호작용이 거의 없음) => need QoS(Quailty of service) features

Qos is commonly implemented to ensure that these applications have enough bandwidth and are prioritized throughout the network to limit the amount of delay they incur.

**Build a network**

* Networking software
	* server software
		* offers some form of service to client on the network
		* provide e-mail ... etc
	* client software
* Networking hardware
	* hubs, switches, routers, firewalls, wireless access pointes, modems, channel service units/data service units (CSUs/DSUs) ...
* Media type
	* copper or fiber cabling, wireless

### Local Area Networks (LAN)

used to connect networking devices that reside in a close geographic area

PCs, servers, switches, routers, multilayer switches, voice gateways, firewalls, and other devices

The media types used in LANs include copper and fiber cabling. They can also include Ethernet, Fast Ethernet (FE), Gigabit Ethernet (GE), Token Ring, and Fiber Distributed Data Interface (FDDI), which are different network architectures. Today most networks use some form of Etherenet

*Hubs and bridges are rarely used in today's networks: they've been replaced with switches.*

### Wide Area Networks (WAN)

used to connect multiple LANs together.

Four basic types of connections, or circuits, are used in WAN services:
* circuit-switched
* cell-switched
* packet-switched
* dedicated connections

> LANs provide high-speed bandwidth connections to interconnect components in a geographically close location
> WANs involve paying recurring monthly costs to a service provider

A wide array of WAN services are available to connect different office locations, including analog dialup, Asynchronous Transfer Mode (ATM), dedicated circuits, cable, digital subscriber line (DSL), Frame Relay, Integrated Services Digital Network (ISDN), and X.25

Analog dialup and ISDN are examples of circuit-switched services; ATM is an example of a cell-switched service; and Frame Relay and X.25 are examples of packet-switched services

**Circuit-switched services**
* Analog dialup
* ISDN
* provide a temporary connection across a phone circuit
* In networking, these services are typically used for backup of primary circuits and for temporary boosts of bandwidth.
* More commanly today, celluar wireless services (3G and 5G) are used for dial-on-demand applications or backup connections
* 통신을 시작하면 전용 회선이 만들어짐 (임시 회선), 통신이 끝날 때 까지 고정된 상태로 유지됨

**Dedicated circuit**
* permanent connection between two sites in which the bandwidth is dedicated to that company's use.
* common when a variety of services, (voice, video, data) must travers the connection and you want guaranteed bandwidth available

**Cell-switched services**
* ATM
* can provide the same features that dedicated circuits offer,
* one of its advantages over dedicated circuits is that it enables a single device to connect to multiple devices on the same interface.
* The downside of these services is that they are not available at all locations
* difficult to set up and troubleshoot, equipment is expensive

**Packet switched services**
* X.25
* are similar to cell-switched services, except cell-switched services switch fixed-length packets called cells and packet-switched services switch `variable-length` packets. This feature makes packet-switched services better suited for data services, but they can nonetheless provide some of the QoS features that cell-switched services provide.


| Cell Switched         | Packet Switched        |
| --------------------- | ---------------------- |
| 데이터를 셀(고정 크기)로 쪼개서 전송 | 데이터를 패킷(가변 길이)로 쪼개서 전송 |
| 각 셀이 독립적으로 전달         | 각 패킷이 독립적으로 전달         |
| QoS 가 보장됨             | QoS 보장이 어려움            |

|**항목**|**Circuit-Switched**|**Dedicated Circuit**|**Cell-Switched**|**Packet-Switched**|
|---|---|---|---|---|
|연결 시점|요청 시 생성|항상 연결|요청 시 셀 경로 설정|요청 시마다 패킷 전송|
|전송 단위|전체 데이터 흐름|전체 데이터 흐름|**고정 길이 셀**|**가변 길이 패킷**|
|QoS 보장|중간|최고|매우 좋음|기본적으론 없음 (선택적 있음)|
|효율성|낮음|낮음|중간|높음|
|비용|중간|높음|중간|낮음|
|사용 예시|전화, 백업용|본사↔지사 연결|ATM망|인터넷, VPN, VoIP|
|현대 사용 빈도|낮음|낮음~중간|매우 낮음|**매우 높음**|

**QoS (Quality of Service)**
: 네트워크 자원이 한정되어 있을 때, 중요한 트래픽 (음성 영상 등)에 우선순위를 주고 지연, 지터, 패킷 손실을 최소화해주는 기술

Two other WAN services that are very popular technologies for high-speed internet connections are

1. digital subscriber lines (DSLs)
	- speeds up to a few megabits per seconds (Mbps)
	- costs much less than a typical WAN circuit from the carrier
	- supports both voice and video and doesn't require a dialup connection (always enabled)
	- coverage is limited (18,000 feet)
2. cable
	- access uses coaxial copper and fiber connections
	- supports higher data rates than DSL
	- provides a full-time connection
	- It is a shared service and functions in a logical bus topology much like Ethernet
	- more customers in an area that connect via cable, the less bandwidth each customer has;
	- because many people are sharing the medium, it is more susceptible to security risks

Examples of networking devices used in WAN connections include cable and DSL modems, carrier switches, CSU/DSUs, firewalls, analog modems, and routers

## Common Network Services and Devices

most of questions encounter on the CCNA exams

* proper implementation of features on Cisco router or switch
* some networking questions that test knowledge of basic concepts
	* purpose of different network services and network infrastructure components

This section learn about several network services and infrastructure components (routers, switchs)

### Types of Services

Networks today include many different servers that provide different types of services.

* DNS server
	* This service is responsible for resolving fully qualified domain names (FQDNs) to IP address
* DHCP server
	* responsible for assigning IP address to computers and devices when they connect to the network so that they can communicate with other systems on the network or surf the internet
* HTTP server
	* responsible for hosting web pages that can be delivered to clients that connect to the web server
* File server
	* used to hold all the data files for a company.
	* typically has lots of storage space and becomes the central point of storage for the company.
	* also becomes the central location of backup to ensure that data files are backed up
* Application server
	* specified applications installed to provide services on the network
	* ex) database product installed on the application server enables the server to provide application data out to the network;
	* ex) e-mail server application installed on the server enables all users to connnect to the server to send and receive e-mail
* SMTP/TOP
	* runs specific protocols that enable the e-mail software to send or receive e-mails
	* SMTP : internet protocol for sending email
	* POP  : Internet protocol for reading email
* FTP
	* This service can be installed on a host to enable someone to connect to the network and transfer files across the network or Internet
	* FTP uses TCP as the transport layer protocol and supports authentication
* TFTP
	* The Trivial File Transfer Protocol
	* enables to upload or download a file
	* TFTP uses UDP and does not support authentication
* Ping
	* `ping <ip_addr>`
	* `man ping` 으로 메뉴얼 확인가능
* Telnet
	* enables you to create a remote connection to system
* SSH
	* Secure Shell
	* secure method of performing remote administration of a device on the network
	* SSH encrypts all communication including the username and password
	* it is designed as a secure replacement to telnet

### Network Infrastructure Components

Networks today use a number of different devices to aid in their functionality or security.

### Hub

older network device, replaced by the more effective and secure switches

* No filtering
	* hub would receive data and then send the data to all other ports on the hub
	* The hub is a layer 1 device
	* simply received the signal and sent it to all ports;
	* it dose not understand Media Access Control (MAC) addresses,
* Collisions
	* Because any data sent was sent to all other ports, and because any system could send its own data at any time, this resulted in a lot of collisions on the networks
* Security
	* Because the data was sent to all ports on the hub, all systems receive all data.
	* if someone was running a packet sniffer on a system that used a hub, he would receive all packets and be able to read them.

Switches reduce collisions, optimize traffic, and are better from a security point of view

### LAN Switch

All other devices connect to the switch to gain access to the network. (For example, connect workstations, servers, printers, and routers to switch) that each device can send and receive data to and from other devices.

The switch acts as the central connectivity point for all devices on the network

### Layer 2 Switch

The switch tracks every device's MAC address (the physical address burned into the network card) and then associates that device's MAC address with the port on the switch to which the device is connected.

The switch stores this information in a MAC address table in memory on the switch. The switch then acts as a filtering device by sending data only to the port that the data is destined for

> device A (MAC: aaaa.aaaa.aaaa.aaaa) PORT 1
> device B (MAC: bbbb.bbbb.bbbb.bbbb) PORT 2
> device C (MAC: cccc.cccc.cccc.cccc) PORT 3
> device D (MAC: dddd.dddd.dddd.dddd) PORT 4
> SWITCH -> MAC Table { port1: aaaa.aaaa.aaaa.aaaa, 
> 					  port2: bbbb.bbbb.bbbb.bbbb,
> 					  port3: cccc.cccc.cccc.cccc,
> 					  port4: dddd.dddd.dddd.dddd}

* device A가 대상 mac 주소가 `cccc.cccc.cccc.cccc` 인 device로 프레임을 전송
* 프레임이 스위치로 들어감
	* 스위치가 mac table 을 확인하고, port3 로 프레임 전송

### Router

A switch is used to connect all systems together in a LAN type of setup

for send data from LAN to another LAN or Internet => use router

A router sends, or routes, data from one network to another until the data reaches its final destination. although switches look at the MAC address to decide where to forward a frame, routers use the IP address to determine what network to send the data to.

The router has a routing table, which is a list of all the networks that it knows how to reach. A router also includes a `gateway` of last resort (GWLR) setting is used if the router does not have a route to the destination in its routing table

* IP 주소 이용해서 어디로 보낼지 결정함
* Routing table
	* 192.168.1.0/24 -> Port 1
	* 10.0.0.0/8     -> Port 2

![[스크린샷 2025-04-15 16.41.38.png]]

**A 에서 D로 데이터 전송**

1. A -> 스위치
2. 스위치 -> R1 라우터
3. R1 라우터에서
	* IP table 확인,
	* 14.0.0.0 네트워크로 보내야하는데 직접 연결 안되어있음 -> R2 라우터로 보냄
4. R2 라우터에서,
	* 스위치로 보냄 -> 스위치가 MAC 주소 비교해서 D로 보내줌

switches (layer 2) => work with MAC addr
routers (layer 3) => work with IP addr

### Layer 3 switch

Layer 2 switch + functionality of being router

### Connecting to WAN with CSU/DSU

The channel service unit/data service unit (CSU, DSU) is a device that enables on organization to connect high-speed data link from the ISP to the organization's router for access to and from a LAN or WAN

These high-speed connections are usually T1 or T3 connections or E1 and E3

= CSU/DSU 를 이용해서 T1/T3 회선 사용 (인터넷 제공업체와 LAN, WAN 연결)

|**이름**|**기능**|
|---|---|
|**CSU (Channel Service Unit)**|신호를 깨끗하게 유지하고, 전기적 간섭으로부터 보호해줌. 서비스 제공자 측과 연결|
|**DSU (Data Service Unit)**|라우터에서 나온 데이터를 **T1/T3 회선에서 사용할 수 있는 형태로 변환**함|

![[스크린샷 2025-04-15 16.53.40.png]]

Router 에 integrated 된 경우가 많음

**demarc, demarc extension**

`demarc`: (demarcation point), is the point where the service provider's equipment connects to your building.

`demarc extension`: area between the connection into your building (from the service provider) and your company's communication equipment.
typically a patch cable that connects the service povider's line to your company's network equepment

![[스크린샷 2025-04-15 19.48.18.png]]

### Firewalls

firewalls control which traffic is allowed to enter a network or system and which traffic should be blocked.

When configuring a firewall, you create the rules for allowing and denying traffic based on the protocol, port number, and direction of the traffic.

* Packet filtering firewall
	* A packet filtering firewall can filter traffic based on the source and destination IP addresses, the source and destination port numbers, and the protocol used.
	* it does not understand the context of the conversation, so it is easy for a hacker to craft a packet to pass through the firewall
	* 출발지 IP, 목적지 IP, 포트 번호, 프로토콜 확인 (헤더만 확인)
* Stateful packet inspection firewall
	* filters traffic based on source and destination IP addresses, the source and destination port numbers,
	* it also understands the context of a conversation and will not allow a packet through the firewall unless it suits a specific scenario.
	* if hacker sends a packet to your network and tries to make it look like the packet is a response from a web site that someone visited, your firewall will know that no one visited that site, so it will not allow the traffic through
	* 기존 연결 상태 (세션)을 추적해서 방화벽 작동
* Next-generation firewall (NGFW)
	* layer 7 firewall
	* that can inspect the application data and detect malicious packets
	* regular firewall filteres traffic based on it being HTTP or FTP traffic (using port numbers), but it cannot determine if there is malicious data inside the HTTP or FTP packet.
	* An application-layer NGFW can inspect the application data that exists in the packet and determmine whether there is suspicious content inside.

|**방화벽 종류**|**계층**|**검사 방식**|**특징**|
|---|---|---|---|
|Packet Filtering|Layer 3/4|IP, 포트|기본적인 필터링|
|Stateful Inspection|Layer 3/4|세션 추적|연결 흐름 이해|
|NGFW|Layer 7|앱 데이터 분석|지능형 위협 탐지 가능|
### Intrusion Prevention System (IPS)

security device that monitors activity, logs any suspicious activity, and then takes some form of corrective action.

if someone is doing a port scan on the network, the IPS would discover this suspicious activity, log the activity, and then disconnect the system that is performing the port scan from the network

### DHCP

DHCP is responsible for assigning IP address information automatically to systems on the network.

The network administrator configures the DHCP server by configuring a scope (a range of addresses) that the server can assign addresses from.

The DHCP service may configure a client with all the TCP/IP settings, including the subnet mask, the default gateway, and the address of the DNS server

When a client starts up, it sends out a broadcast message looking for a DHCP server to receive an IP address from. The DHCP server replies with an offer, and the client then requests the address that has been offered. Finally, the DHCP service acknowledges that the client has the address for a period of time

**DORA**
1. Discover
	* 클라이언트가 network 접속시에, 브로드캐스트 메시지 보냄 (DHCP 서버 찾음)
2. Offer
	* DHCP 서버가 응답, IP 주소 할당해줌
3. Request
	* 클라이언트가 DHCP 서버가 할당해준 IP 주소 사용하겠다고 요청
4. Acknowledge
	* DHCP 서버가 확인 응답 보냄
	* 임대 기간(Lease Time) 정해짐

**Scope**

DHCP 서버가 IP주소를 할당할 수 있는 범위, 이 범위 안에서 IP를 클라이언트에게 나눠줌

### Network Address Translation (NAT)

NAT enables us to hide our internal network structure from the outside world by having the NAT device receive all outbound packets, take the internal source address out, and replace it with the public IP address of the NAT device.

This is beneficial because anyone who intercepts the data on the internet will believe that the packet come from the NAT device and not the internal computer on the LAN.

As a result anyone who decides to attack the source of the packets will be attacking the NAT device instead, which will typically be a firewall product as well.

=> 내부 사설 IP 주소를 외부 공인 IP주소로 변환해줌

### Wireless Access Points/Access Points

An `access Point` is device added to your network that enables wireless clients to connect to the network.

### Wireless LAN Controllers

Configuring each access point on a network can be a time-consuming undertaking, with lots of potentials for errors.

As a network administrator, you can centrally manage devices instead of needing to run around to each device and perform the configuration.

Administrators can use a wireless LAN controller (WLC) or wireless controller for short, to manage access points centrally using the Lightweight Access Point Protocol (LWAPP). This removes the administrative burden of configuring each access point individually.

=> WLC를 이용해서 중앙제어 가능 (LWAPP 프로토콜 사용)

### Endpoints and Servers

* Client
	* Linux, mac machine connects to the network to access resources such as files, printers, or the internet
* Server
	* provides resources to the network
	* file server, Active Directory server, a database server and a web server
* Printer
* Mobile device

### Collision Domains and Broadcast Domains

**Collision domain**

In this group of systems, simultaneous data transmissions can collide with one another

**Broadcast domain**

In this group of systems, each system can receive other systems' broadcast messages.

### Collision domain

In collision domain, data transmission collisions can occur.

For example, suppose you are using a hub to connect five systems to a network. Because traffic is sent to all ports on the hub, it is possible that if several systems send data at the same time, the data could collide on the network.

For this reason, all network ports on a hub (and any devices connected to those ports) are considered parts of a single collision domain.

This also means that when you cascade a hub off another hub, all hubs are part of the same collision domain.

### Broadcast Domain

A broadcast domain is a group of systems that can receive one another's broadcast messages. When using a hub to connect five systems in a network environment, if one system sends a broadcast message, the message is received by all other systems connected to the hub. For this reason, all ports on the hub create a single broadcast domain


=> 충돌 방지 = 스위치 사용, broadcast 도메인 분리 = 라우터 사용

---

## REVIEW

**네트워크 설계시 확인**
* cost
* security
* speed
* topology
* scalability
* reliability
* availablity

**Protocols**
=> SMTP, POP3, IMAP4, FTP, HTTP, ...

HUB => 여러 endpoint 장치 연결, 브로드캐스팅 (layer 1)
Switch => 여러 endpoint 장치 연결, MAC 주소로 목적지에만 패킷 전달 (layer 2 또는 라우터의 기능을 일부 가지는 경우 layer 3)
Router => 여러 Switch 또는 Hub 간 연결 + `CSU/DSU`포함된 경우 인터넷 (ISP) 연결 (layer 3)

firewall : 트래픽 제어
* 패킷 필터
	* IP 주소, 포트만 보고 필터링
* stateful 패킷 필터
	* 세션 상태도 함께 추적 (연결된 장비의 state 추적)
* NFGW

DHCP => IP주소를 할당해주는 서비스 (서버에서 돌아가는 프로그램임) 요청이 들어오면 IP주소 자동으로 할당해줌

**Wireless AP**
각 무선 endpoint 장비가 인터넷에 연결할 수 있도록 해줌, WLC로 중앙제어 가능 (CAPWAP 프로토콜 이용)

**colision domain**
hub로 장비를 연결했을때 각 장비가 데이터를 전송하려 할 때, 충돌이 일어날 수 있음 (switch (layer 2) 로 해결 가능함)

**broadcast domain**
switch 또는 hub 로 연결된 장비들이 broadcast 로 전달된 정보를 읽을 수 있음, router로 분리하거나 VLAN으로 해결 가능함

**NAT**
* Network Address Translation
* 내부 사설 IP주소를 외부 공인 IP주소로 변경해줌

---

## Network Design Models

A well-designed network is hierarchical in structure, where each layer in the hierarchy is focused on a specific job function. A well-designed network is also modular, in the sense that it is easy to add components to the network, so that as the company grows, the network can grow. The well-designed network should also be flexible and resilient, meaning that it is always available and includes some form of redundancy.

* hierarchical in strucuture
* modular
* flexible and resilient

When designing Cisco networks, we usually follow tow common design models

1. the three-tier architecture model
2. collapsed core architecture model

### Three-tier Architecture

* Access layer
	* This layer is at the bottom of the hierarchy;
	* it enables and users to connect to the network via switches.
	* The access layer provides features that enable you to isolate traffic between different types of users and divide broadcast domains.
	* A switch located on each floor of a building would represent this layer
	* 엔드 유저 장비들이 주로 연결됨, Layer 2 스위치 사용
	* VLAN, 포트 보안, QoS, 브로드캐스트 분리 기능 제공함
* Distribution layer
	* it enables you to control who can access what part of the network by using features such as routing, access control lists (ACLs), and network access policies.
	* A distribution layer could be a central layer 3 switch residing in two buildings, in which each switch could access the layer 3 switch of the other building to connect
	* Layer 3 스위치 또는 라우터 사용
	* 라우팅, ACL, VLAN간 통신, 정책 적용
	* 사용자 그룹별로 네트워크 접근 권한 제어
* Core layer
	* This layer is typically used to connect the networks of different buildings together.
	* It includes a number of core routers that are reponsible for delivering traffic to and from the network.
	* 고속 라우터 또는 layer 3 스위치 사용
	* 대규모 네트워크 간 트래픽 전송
	* 빠르고 안정적인 데이터 전달만 담당 (정책 없음)

|**계층**|**기능**|**장비**|**설명**|
|---|---|---|---|
|**Access**|사용자 연결|Layer 2 스위치|사용자 장치 접속 (VLAN 등)|
|**Distribution**|정책 적용 및 라우팅|Layer 3 스위치|VLAN 라우팅, ACL|
|**Core**|고속 백본|고속 라우터 or 스위치|속도와 안정성 위주 트래픽 전달|
유저 관리는 Access 에서만, 정책은 Distribution 에서 통제, 전달은 core에서 담당

### Collapsed Core (Two-tier) Architecture

A small organization may not need to use a three-tier model, so it can combine the distribution and core layers into a single layer known as collapsed layer

=> access layer + collapsed (distribution/core) layer

Each floor would have a switch (access) that enables clients on that floor to connect to the network. Each floor switch would then connect to a central switch in the building, which is connected to a router to access the WAN or Internet

### Spine-leaf Architecture

Another example of a two-tier architecure

leaf 스위치들이 사용자(또는 장비)를 연결, 그 위에 있는 spine 스위치들이 back-bone 역할을함

**Leaf 스위치**
* Network 의 Access layer
* 모든 leaf 스위치는 모든 spine 스위치에 연결됨
* leaf-leaf 간 직접 연결은 없음

**Spine 스위치**
* 모든 leaf와 Full Mesh 방식으로 연결됨
* Spine-Spine 간 직접 연결은 없음

```
      [Spine1]   [Spine2]
        ╱   ╲     ╱   ╲
       ╱     ╲   ╱     ╲
 [Leaf1]    [Leaf2]   [Leaf3]
    │          │         │
 [서버1]      [서버2]    [서버3]
```

| **항목**       | **Spine-Leaf 아키텍처**             | **Collapsed Core 아키텍처**            |
| ------------ | ------------------------------- | ---------------------------------- |
| **구조**       | Leaf ↔ Spine 간 **Full Mesh** 연결 | Distribution + Core 계층을 **하나로 통합** |
| **레이어**      | 2계층 (Leaf, Spine)               | 2계층 (Access, Collapsed Core)       |
| **Leaf 역할**  | Access 계층, 장비 직접 연결             | Access 계층과 동일                      |
| **Spine 역할** | 백본 역할, 모든 Leaf와 연결됨             | Distribution + Core 기능 통합          |
| **확장성**      | 매우 뛰어남 (데이터센터에서 주로 사용)          | 중소규모에 적합, 확장성은 제한적                 |
| **트래픽 흐름**   | Spine을 거쳐 Leaf ↔ Leaf 통신        | Core 장비가 중심, 트래픽 집중                |
| **특징**       | 고성능, 저지연, 이중화 구조                | 장비 수 줄여서 비용 ↓, 관리 간편               |
| **사용 예시**    | 데이터센터, 고성능 클라우드 인프라             | 소기업, 지점 사무실 등 중소규모 환경              |
|              |                                 |                                    |

모든 경로가 동일한 홉 수(Equal Cost Multi-path)를 가지기 때문에 성능 예측 가능

### Small Office/Home Office (SOHO)

SOHO network typically involves a high-speed internet connection into a home or office that connects to an ISP's modem. The ISP's modem is connected to a WAN/Internet port on a wireless router, which usually has four switch ports that you can use to connect clients using unshielded twisted pair (UTP) cabling.

A wireless router in a SOHO network could also provide a number of services, DHCP server, NAT device, firewall, wireless access point

### On-premises vs. Cloud

|**항목**|**On-Premises**|**Cloud**|
|---|---|---|
|위치|**회사 내부** 물리적 장소 (사무실, IDC 등)|**클라우드 제공자**의 데이터센터|
|소유권|서버, 장비 모두 **내가 직접 소유/관리**|장비는 **제공업체 소유**, **내가 빌려서 사용**|
|예시|회사에 설치된 WLC, 서버, 방화벽 등|AWS, Azure, Google Cloud, Cisco Meraki 등|
|유지관리|내가 해야 함 (백업, 장애대응, 업그레이드)|클라우드 제공자가 처리함|
|확장성|하드웨어 구매 필요 → 느림|설정만으로 확장 가능 → 매우 빠름|
|비용 구조|초기 설치 비용 큼 (CAPEX)|월 구독/사용량 기반 (OPEX)|
|보안|직접 제어 가능 (물리 보안 포함)|보안은 제공자 정책에 따라|

## Network Topologies

When you connect devices into a network, you need to decide which topology to use.

The topology of the network is the layout of the network, and you can choose from a handful of network topologies

![[스크린샷 2025-04-16 01.15.24.png]]

**Point to Point**

* single connection between two components
* two routers connected across a dedicated WAN link

**star topology**

* a central device has many point-to-point connections to other components
* many components need to be connected
* An example of a media type that uses a start toplogy is 1000BaseTx Ethernet.
* if the center of the start fails, no components can communicate with others

**bus topology**

* all components are connected to and share a single wire
* 10Base5 and 10Base2 Ethernet
* if any system sends data on the wire, the data travels the entire length of the wire.
* old 10Base5, each device connected to the coaxial cable via a vampire tap.

**ring topology**

* device one connects to device two, device two connects to device three, .... which connectes back to the first device
* can be implemented as single ring or a dual ring
* Dual rings are typically used when you need redundancy.
	* if one of the components fails in the ring, the ring can warp itself

**FDDI**

Fiber Distributed Data Interface
광케이블 기반, Dual ring topology

### Physical vs. Logical Topologies

Physical = 실제 케이블이 어떻게 연결되어 있는지
Logical = 데이터가 어떤 경로로 흐르는지, 어떻게 통신하는지에 대한 구조

ex1. 1000BaseT Ethernet
physical star topology, logical bus topology

![[스크린샷 2025-04-16 03.32.52.png]]

### Fully and Partially Meshed Topologies

![[스크린샷 2025-04-16 03.35.30.png]]

determine the number of links needed to fully mesh a WAN is N x (N-1) / 2, if you had 10 locations, 10 x 9 / 2 = 45 links

## Types of Cabling

LANs typically use either copper or fiber-optic cabling. Copper cabling can include one strand of copper across which an electrical voltage is transmitted, or it can be many strands of copper. Fiber-optic cabling uses light-emitting diodes (LEDs) and lasers to transmit data across a glass core. With this transmission, light is used to represent binary 1s and 0s: if light is on the wire, his represents a 1; if there is no light, this represents a 0.

### Copper Cabling

Between copper and fiber, implementing copper cabling is less expensive.

* Thicknet (10Base5)
	* Uses a thick coaxial cable
* Thinnet (10Base2)
	* Uses a thin coaxial cable
* Unshielded twisted pair (UTP)
	* Uses four pairs of wires in the cable, where each pair is periodically twisted
	* most common

copper cabling including UTP, has two disadvantages:

1. It is susceptible to electromagnetic interface (EMI) and radio frequency interface (RFI)
2. Distances of the cable are limited to a short haul (100 meters)

UTP's internal copper cables are either 22- or 24-gauge in diameter. UTP for Ethernet has 100-ohm impedance, so you can't use just any UTP wiring, like the cable that is commonly found with telephones, for example. Each of the eight wires inside the cable is colored: some solid, some striped. Two pairs of the wires carry a true voltage, commonly called "tip" (T1-T4), and the other four carry an inverse voltage, commanly called "ring" (R1-R4)

A pair consists of a positive(tip) and negative(ring) wires, such as T1 and R1, T2 and R2, and so on, where each pair is twisted down the length of the cable

|**항목**|**설명**|
|---|---|
|구조|4쌍(8가닥)의 꼬인 선, 색상은 보통 **단색 + 줄무늬** 쌍|
|두께|보통 **22 또는 24 게이지(gauge)** (굵기 단위)|
|임피던스|**100 ohm** (이더넷에 맞춰져 있음)|
|최대 길이|약 **100미터** (그 이상은 신호 약화됨)|
|설치|얇고 유연해서 설치 쉽고 비용 저렴|
|단점|전자기 간섭(EMI), 무선 간섭(RFI)에 약함 (차폐 없음)|

### UTP categories

![[스크린샷 2025-04-16 04.20.43.png]]

Each of the endpoints of a UTP cables has an RJ-45 connector.

### Cabling Devices

With today's implementation of Ethernet over copper, two components make up the connection: an RJ-45 connector and a Cat5, 5E, 6A UTP cable

![[스크린샷 2025-04-16 04.28.41.png]]

RJ-45 커넥터에 총 8핀이 있는데 이 8핀에 어떤 색상의 선이 들어가는지가 `pinout`

**pinout**

1. straight-through
	* pin 1 on the other end of the cable, pin 2 to pin 2, ....
	* used to connect dissimilar devices together
		* between computer and switch
		* switch and router

![[스크린샷 2025-04-16 04.36.27.png]]

2. crossover
	* pin 1 on the side is connected to pin 3 on the other side, and pint 2 is connected to pin 6
	* used to connect similar devices together
		* two computers together
		* hub to switch
		* hut to another hub
		* A switch to another switch
		* hub to switch
		* router to a computer 

![[스크린샷 2025-04-16 04.36.53.png]]

### Fiber

Fiber-optic cabling is typically used to provide very high speeds and to span connections across very large distances.

it is expensive to implement, difficult to troubleshoot and difficult to install.

is not affected by EMI(Electromagnetic Interference, 전자기 간섭) and RFI(Radio Frequency interference, 무선 주파수 간섭)

1. Multimode fiver (MMF)
	* transmits 850 or 1300-nanometer (nm) wavelengths of light
	* Fiber thickness for MMF is 62.5/125 microns
	* The core and cladding diameter (thickness of the actual cabling) is 50 to 100 micron
	* range for MMF. The 850/1300-nm wavelengths equate to frequencies in the terahertz(THz) range.
	* MMF's relatively large core diameter supports the propagation of multiple longitudinal modes (different light paths) at a given wavelength;

2. single-mode fiver (SMF)
	* transmits 1300- or 1550-nm light and uses a laser as the light source.
	* Because lasers provide a higher output than LEDs, SMF can span more than 10 kilometers in distance and have speeds up to 100Gbps.
	* Because of SMF's very small core diameter, only a single longitudinal mode is propagated at a given wavelength
	* SMF exhibits less dispersion than MMF and can therefore support much higher data speeds than MMF (100+ Gbps)

|**항목**|**Multimode (MMF)**|**Single-mode (SMF)**|
|---|---|---|
|**광원**|LED|Laser|
|**파장**|850nm / 1300nm|1300nm / 1550nm|
|**코어 두께**|50~62.5μm|8~10μm|
|**거리**|수백 미터 (최대 약 2km)|수 km 이상 (최대 수십~수백 km)|
|**속도**|기가급~수 Gbps|수 Gbps~수십 Gbps 이상|
|**비용**|저렴|비쌈|
|**용도**|건물 내 연결, 캠퍼스 네트워크|장거리, 대역폭 높은 백본, ISP 회선|
**wavelength division multiplexing (WDM)**

allows more than two wavelengths (signals) on the same piece of fiber, increasing the number of connections

**dense WDM (DWDM)**

allows yet more wavelengths, more closely spaced together: more than 200 wavelengths canbe multiplexed into a light stream on a single piece of fiber

Cabling provides the protective outer coating as well as the inner cladding. The inner cladding is denser to allow the light source to bounce off of it. In the middle of the cable is the fiver itself, which is used to transmit the signal. The index of refraction (IOR) affects the speed of the light source: it's the ratio of the speed of light in vacuum to the speed of light in the fiber. IOR measures the density of the fiber; the denser the fiber, the slower the light travels through it

케이블 = outer 코팅 + inner 클래딩
inner 클래딩은 밀도가 더 커서 light source 가 bounce 할 수 있게 함, 케이블 중간에는 fiver 가 있음

**IOR**
: ratio of the speed of light in vacuum to the speed of light in the fiber.
=> fiber 의 density 측정 가능

**loss factor**

used to describe any signal loss in the fiber before the light source gets to t he end of the fiber.

* Connector loss 
	* occurs when a connector joins two pieces of fiber: a slight signal loss is expected.
* fiber length (attenuation)
	* longer the fiber, the greater the likelihood that the signal strength will decrease by the time it reaches the end of the cable. (attenuation)

Two other terms describe signal degradation

* *Microbending*
	* occurs when a wrinkle in the fiber
	* typically where the cable is slightly bent,

To overcome degradation over long distances, optical amplifiers can be used. These are similar to an Ethernet repeater or hub. A good amplifier, such as an erbium-doped fiber amplifier (EDFA), converts a light source directly to another light source, providing for the best reproduction of the original signal. Other amplifiers convert light to an electrical signal and then back to light, which can cause degradation in the signal quality

* erbium-doped fiber amp (EDFA)
	* light -> light
* other amp
	* light -> electrical signal -> light

Two main standards are used to describe the transmission of signals across a fiber: SONET (Synchronous Optical Network) and SHD (Synchronous Digital Hierarchy)

**SONET**

* defined by the Exchange Carriers Standards AAssociation (ECSA) and American National Standards Institue (ANSI)
* typically used in North America

**SDH**

* international standards used throughout most of the world (except North America)

Both of these standards define the physical layer framing used to transmit light sources, which also includes overhead for the transmission. Three types of overhead are experienced:

1. Section overhead (SOH)
	* Overhead for the link between two devices, such as repeaters

2. Line overhead (LOH)
	* Overhead for one or more sections connecting network devices, such as hubs

3. Path overhead (POH)
	* Overhead for one or more lines connecting two devices that assemble and disassemble frames, such as carrier switches or router's fiber interface

오버헤드 : 실제 데이터를 전송할 때, 추가로 따라오는 관리용 정보
SOH: 장치 - 장치 간 물리적 구간의 전송 품질 관리, 오류 감지 시계 동기화 등 포함

LOH
* 여러 Section 을 묶은 라인 전체에 대한 제어 정보
* 네트워크 장비 간 트래픽 조정, 에러 관리, 유지보수 정보 포함

POH
* 시작점 - 종단점 전체 경로에 대한 정보
* 전송되는 프레임이 어떤 사용자 데이터인지 추적, 오류 검출 등 

| **오버헤드 종류** | **위치**      | **예시 장비** | **기능**        |
| ----------- | ----------- | --------- | ------------- |
| **SOH**     | 장치 ↔ 장치 구간  | 리피터 등     | 동기화, 기본 상태    |
| **LOH**     | 여러 섹션 묶음 구간 | 허브 등      | 신호 관리, 유지보수   |
| **POH**     | 사용자 경로 전체   | 라우터, 스위치  | 프레임 추적, 에러 관리 |
|             |             |           |               |

Typically, either a ring or point-to-point topology is used to connect the devices. With carrier metropolitan area networks (MANs), the most common implementation is through the use of rings. Auto-protection switching (APS) can be used to provide line redundancy: in case of failure on a primary line, a secondary line can automatically be utilized

MANs => 링 네트워크 또는 P2P 토플로지 사용
APS를 이용해서, 링 이중화 구성함

![[스크린샷 2025-04-17 00.57.02.png]]

OC (Optical Carrier) - 광신호 기준 SONET 속도 단위

The most common types of fiber-optic connectors, and their typical uses, are
* **Fiber Channel (FC)** 
	* Used by service providers in their patch panels
* **Local Connector (LC)**
	* Used for enterprise equipment and commonly connect to small form-factor pluggable (SFP) modules
* **Standard Connector (SC)**
	* Used for enterprise equipment
* **Straight Tip(ST)**
	* Used for patch panels because of their durability

Traditinally, SC the most commonly used connector type; however, LCs are becommoning more and more common

![[스크린샷 2025-04-17 22.06.40.png]]

## Access Methods

An access method determines how a host will place data on the wire: does the host have to wait its turn or can it just place the data on the wire whenever it wants? The answer is determined by three major access methods: CSMA/CD, CSMA/CA, and token passing.

### CSMA/CD (Carrier-sense multiple access with collision detection)

* every host has equal access to the wire can place data on the wire when the wire is free from traffic.
* If a host wants to place data on the wire, it will "sense" the wire and determine whether a signal is already present. If it is, the host will wait to transmit the data;
* if the wire if free, the host will send the data

if two systems "sense" the wire at the same time, they will both send data at the same time if the wire is free. When the two pieces of data are sent on the wire at the same time, they will collide with one another and the data will be destroyed.

if is destroyed in transit, the data will need to be retransmitted. Consequently, after a collision, each host will wait a variable length of time before retransmitting the data (the don't want the data to collide again), thereby preventing a collision the second time. When a system determines that the data has collided and then retransmits the data, this is known as collision detection.

with CSMA/CD, before a host sends data on the network,
1. it will "sense" (CS) the wire to ensure that is free of traffic
2. Multiple system have equal access to the wire (MA),
3. and if there is a collision, a host will detect that collision (CD) and retransmit the data.

### CSMA/CA (Carrier-sense multiple access with collision avoidance)

* before a host sends data on the wire, it will "sense" the wire to see if it is free of signals
* if the wire is free, the host will try to "avoid" a collision by sending a signal out, letting all others know they should wait before sending data.
* helps prevent collisions from occurring
* involves sending more data out on the wire

### Token Passing

With both CSMA/CD (충돌 디텍션), CSMA/CA (충돌 회피), the possibility of collisions is always there, and the more hosts that are placed on the wire, the greater the chances of collisions, because you have more systems "waiting" for the wire to become free so that they can send their data.

With tocken passing, an empty packet is running around on the wire (token) To place data on the wire, the system needs to wait for the token; once the system has the token and it is free of data, the system can place data on the wire.

Since there is only one token and a host needs to have the token to "talk", it is impossible to have collisions in a token-passing environment.

**example**

> if Workstation 1 wants to send data on the wire, the workstation would wait for the token, which is circling the network millions of time per second
> 
> Once token reaches Workstation 1, the workstation takes the token off the network, fills it with data, marks the token as being used so that no other systems try to fill the token with data, and then places the token back on the wire heading for the destination host
> 
> All systems will look at the data, but they will not process it, since it is not destined for them.
> However, the system that is the intended destination will read the data and send the token back to the sender as confirmation, Once the token has reached the original sender, the token is unflagged as being used and released as an empty token onto the network

1. wait for token
2. check token status
3. (if token is empty) loads data & change token status to "being used"
4. release token to networks
5. (in other system) check token, and look destination
	1. if destination is dose not match -> pass along
	2. if destination is dose match -> data process & resend to sender for confirmation
6. token has reached the original sender, the token is unflagged

## Network Architectures

### Ethernet

--> 10Base

**IEEE 802.3 10Mb Standards**
![[스크린샷 2025-04-17 22.49.17.png]]

One of the most common is IEEE 802.3 10Mb, The most common copper cabling for Ethernet is UTP.

Ethernet supports a bus topology--physical or logical.

In a bus topology, every device is connected to the same piece of wire and all devices see every frame.

**Examples**
> 10Base5 uses on long, thick piece of coaxial cable.
> NICs tap into this wire using a vampire tap.

With 10Base2, the devices are connected together by many pieces of wire using BNC connectors, commanly called T-tabs: one end of the T-tap connects to the NIC and the other two connect to the two Ethernet cables that are part of the bus.
Both endpoints of the cable must be terminated with a terminator cap. With 10BaseT, all devices are connected to a hub, where the hub provides a local bus topology. All of these 10 Mbps Ehternet solutions support only half-duplex: they can send or receive, but they cannot do both simultaneously.

### Fast Ethernet

--> 100Base

Ethernet 10Base2 and 10Base5 havent been used in years because of the difficulty in troubleshooting network problems based on the cabling they use. And 10BaseT networks have been supplanted by higher speed Ethernet solutions, such as Fast Ethernet and Gigabit Ethernet. All Ethernet standards uses CSMA/CD as the access method with Fast Ethernet running at 100 Mbps (while the older Ethernet ran at 10 Mbps). The older 10 Mbps Ethernet is half duplex, Fast Ethernet is full duplex (can send and receive data at the same time)

![[스크린샷 2025-04-17 23.37.13.png]]

Fast Ethernet supports both half and full duplex connections
### Gigabit Ethernet

-> 1000Base

defined in IEEE 802.3z. To achieve 1-Gbps speeds, IEEE adopted the ANSI X3T11 Fiber Channel standard for the physical layer implementation. The physical layer is different from Etherent and Fast Ethernet in that uses an 8B/10B encoding scheme to code the physical layer information when transmitting it across the wire. The IEEE standard has been around for about a decade. Gigabit Ethernet aconnections are comonly used for uplink connections (switch-to-switch) and server applications

![[스크린샷 2025-04-17 23.43.15.png]]

### 10-Gigabit Ethernet

* 10GBaseSR
	* Runs at 10 Gbps and uses "short-range" MMF calbe,
	* maximum distance of 400 meters
* 10GBaseLR
	* Runs at 10 Gbps and uses "long-range" SMF cable,
	* maximum distance of 10 kilometers
* 10GBaseER
	* uses "extra-long-range" SMF cable
	* maximum distance of 40 kilometers
* 10GBaseT
	* using CAT 6a UTP cabling
	* maximum distance of 100 meters

### Serial, Optical, and Other Architectures

### Serial

Many network technologies dliver data in serial fashion, which means they deliver the data in a stream of bits, one after the other. Serial connections are typically used with WAN connections, enabling your network to connect to other netoworks through a provider.

### Optical

There are also special WAN versions of 10-Gigabit Ethernet use fiber-optic cabling to connect to a SONET network:

* 10GBaseSW
	* short-range MMF cable
	* maximum distance of 100 meters
* 10GBaseLW
	* long-range SMF cable
	* maximum distance of 10 kilometers
* 10GBaseEW
	* extended-range SMF cable
	* maximum distance of 40 kilometers

### Concepts of PoE

PoE is known as the IEEE 802.af standard and is a method of delivering power to devices using the Ethernet port.

*802.at standard = PoE+*

The benefit of PoE is that a device that supports PoE does not need a separate power cable to power the device; it can receive the power through the Ethernet port from the switch it is plugged into.

*Common examples: Ip phones, wireless access points, network cameras ...*

For PoE to work, the switch must have PoE-supported ports that the device would connect to. The device would also have to be a PoE-enabled device that can receive a PoE connection.

### GBICs (gigabit interface converter)

an input/output (I/O) device that is plugged into a Gigabit Ethernet interface (and 10-Gbps connectors) and provides various interface connector types.

The advantage of GBICs is that when you purchase a device that supports GBICs, your device comes with a Gigabit Ethernet port and you buy the appropriate GBIC interface connector based on the cabling you'll be using.

that if you ever need to change your cable requirements, you need to swap only your current GBIC for one that matches your new cabling needs. Most GBICs are *hot-swappable* sometimes are not

### Troubleshooting Interface and Cable Issues

1. Collisions
2. Errors
3. Mismatch Duplex
4. Speed

## Virtualization Fundamentals

### Hypervisors

Virtualization of systems is provided by the hyprvisor. (virtual machine monitor, VMM)

* Type 1
	* runs directly on top of the hardware
	* baremetal hypervisors
	* Hypr-V (microsoft), VMware, ESXi servers
* Type 2
	* having the OS installed on top of the hardware
	* VMware Workstation, Oracle VM, VirtualBox

### Virtual Networking Components

* Virtual Switches
* Virtual Routers
* Virtual Firewall
* Virtual NICs
* Software-defined networking (SDN)
* Virtual desktops
* Virtual servers




