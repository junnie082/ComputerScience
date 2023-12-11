# Data Communication 08

## Basics on Data Link and Upper Layers

#### 2023.12.09.(토)

### Transmission Media

#### : Guided and Unguided Media

### Communication at data link layer

- Communication at data link layer

![datacommunication1](/assets/images/2023-12-09/datacommunication1.png)

### Nodes and Links

- Communication at data link is node-to-node

- Refer to two end hosts and routers as nodes and the networks between them as links

- Nodes and Links

![datacommunication2](/assets/images/2023-12-09/datacommunication2.png)

### Services

- Datalink layer provides services to network layer

  - It also receives services from the physical layer

- Data-link layer of a node (host or router) is responsible for delivering a datagram to next node in the path

  - Data-link layer of sending node needs to encapsulate the datagram received from the network in a frame
  - Data-link layer of receiving node needs to decapsulate the datagram from the frame

- Encapsulate & decapsulation

  - Each intermediate node needs to both encapsulate and decapsulate
  - Each link may be using a different protocol with a different frame format
  - Even if one link and the next are using the same protocol, encapsulation and decapsulation are needed because the link-layer addresses are normally different

- A communication with only three nodes

![datacommunication3](/assets/images/2023-12-09/datacommunication3.png)

- Framing

  - The first service provided by the data link layer is `framing`

    - The data link layer at each node needs to encapsulate the datagram (packet received from the network layer) in a frame before sending it to the next node
    - A packet at the data-link layer is normally called a frame.

  - Frame may have both a header and a trailer

  - Different data link layers have different formats for framing

- Flow Control

* If the rate of produced frames is higher thatn the rate of consumed frames, frams at the receiving end need to be buffered while waiting to be consumed (processed).
  - Ask it to stop or slow down.
* Different data link layer protocols use different strategies for flow control.

- Error Control

  - Electromagnetic signals are susceptible to error
  - `Error detection` and `correction`
    - They are an issue in every layer

- Congestion Control
  - Although a link may be congested with frames, which may result in frame loss, most data link layer protocols do not directly use a congestion control to alleviate congestion

### Two Categories of Links

- Data link layer controls how the medium is used

- Point-to-point link

* The link is dedicated to the two devices
* E.g., traditional home phones

- Broadcast link

  - The link is shared between serveral pairs of devices
  - E.g., cellular phones, which are using a broadcast link (the air is shared among many cell phone users).

- Data link control (DLC) sublayer

  - All issues common to both point-to-point and broadcast links
  - Error detection and correction

- Media access control (MAC) sublayer

  - Only issue specific to broadcast links
  - How to access and share media

- Dividing the data-link into two sublayers

![dc1](/assets/images/2023-12-10/dc1.png)

### Link Layer Addressing

- Link-layer address vs. IP address

- In a connectionless internetwork such as the Internet we cannot make a datagram reach its destination using only IP addresses

  - The reason is that each datagram in the Internet, from the same source host to the same destination host, may take a different path.
  - The source and destinaton IP address define the two ends but cannot define which links the datagram should pass through.
  - IP addresses in a datagram should not be changed

- A link-layer address is sometimes called a link address, sometimes a physical address, and sometimes a MAC address

- When a datagram passes from the network layer to the data-link layer, the datagram will be encapsulated in a frame and two data-link addresses are added to the frame header

  - These two addresses are changed every time the frame moves from one link to another

- IP addresses and link-layer addresses in a small internet

![dc2](/assets/images/2023-12-10/dc2.png)

### Three Types of addresses

- Some link-layer protocols define three types of addresses: unicast, multicast, and broadcast.

- `Unicast` Address

  - Each host or each interface of a router is assigned a unicast address.
  - Unicasting means one-to-one communication

- Example
  - As we will see layer, the unicast link-layer addresses in the most common LAN, Ethernet, are 48 bits (six bytes) that are presented as 12 hexadecimal digits separated by colons.
  - For example, the following is a link-layer address of a computer.
    - The second digit needs to be an even number.

```
A2:34:45:11:92:F1
```

- `Multicast` Address

  - Multicasting means one-to-many communication.
  - However, the jurisdiction is local (inside the link).

- Example
  - As we will see layer, the multicast link-layer addresses in the most common LAN, Ethernet, are 48 bits (six bytes) that are presented as 12 hexadecimal digits separated by colons.
  - The second digit, however, needs to be an odd number in hexadecimal.
  - The following shows a multicast address:

```
A3:34:45:11:92:F1
```

- `Broadcast` Address

  - Broadcasting means one-to-all communication.

- Example
  - As we will see later, the broadcast link-layer addresses in the most common LAN, Ethernet, are 48 bits, all 1s, that are presented as 12 hexadecimal digits separated by colons.
  - The following shows a broadcast address:

```
FF:FF:FF:FF:FF:FF
```

### ARP

- Anytime a node has an IP datagram to send to another node in a link, it has the IP address of the receiving node

* The source host knows the IP address of the default router

- IP address of the next node is not helpful in moving a frame through a link; we need the link-layer address of the next node.

- Address Resolution Protocol (ARP)

* Auxiliary protocols defined in the network layer
* ARP accepts an IP address from the IP protocol, maps the address to the corresponding link-layer address, and passes it to the data-link layer.

- Position of ARP in TCP/IP protocol suite

![dc3](/assets/images/2023-12-10/dc3.png)

- ARP request & ARP reply

- ARP operation

![dc4](/assets/images/2023-12-10/dc4.png)

- Packet Format

* An `ARP packet` is encapsulated directly into a data-link frame.
* The frame needs to have a field to show that the payload belongs to the ARP and not to the network-layer datagram.

- ARP packet

![dc5](/assets/images/2023-12-10/dc5.png)

- Example
  - A host with IP address N1 and MAC address L1 has a packet to send to another host with IP address N2 and physical address L2 (which is unknown to the first host).
  - The two hosts are on the same network.

![dc6](/assets/images/2023-12-10/dc6.png)

### An Example of Communication

- To show how communication is done at the data-link layer and how link-layer addresses are found, let us go through a simple example

- The internet for our example

![dc7](/assets/images/2023-12-10/dc7.png)

- Flow of packets at Alice site

![dc8](/assets/images/2023-12-10/dc8.png)

- Flow of activities at router R1

![dc9](/assets/images/2023-12-10/dc9.png)

- Flow of activities at router R2

![dc10](/assets/images/2023-12-10/dc10.png)

- Activities at Bob's site

![dc11](/assets/images/2023-12-10/dc11.png)

## Network Layer : IP

- Transport segment from sending to receiving host

- On sending side encapsulates segments into datagrams

- On receiving side, delivers segments to transport layer

- Network layer protocols in every host, router

- Router examines header fields in all IP datagrams passing through it

![dc12](/assets/images/2023-12-10/dc12.png)

### Two Key Network Layer Functions

- Network layer functions

  - Forwarding: move packets from router's input to appropriate router output
  - Routing: determine route taken by packets from source to destination
  - Routing algorithms

- Analogy: taking a trip

* Forwarding: process of getting through single interchange
* Routing: process of planning trip from source to destination

### Network Layer: Data plane, Control plane

- Data plane
  - Local, per-router function
  - Determines how datagram arriving on router input port is forwarded to router output port
  - Forwarding function

![dc13](/assets/images/2023-12-10/dc13.png)

- Control plane

* Network-wide logic
* Determines how datagram is routed among routers along end-end path from source host to destination host
* Two control-plane approaches:
* Traditional routing algorithms: implemented in routers
* Software-defined networking (SDN): implemented in (remote) servers

### Per-router Control Plane

- Individual routing algorithm components in each and every router interact in the control plane

![dc14](/assets/images/2023-12-10/dc14.png)

### Logically Centralized Control Plane

- A distinct (typically remote) controller interacts with local control agents (CAs)

![dc15](/assets/images/2023-12-10/dc15.png)

### The Internet Network Layer

- Host, router network layer functions

![dc16](/assets/images/2023-12-10/dc16.png)

### IP Datagram Format

![dc17](/assets/images/2023-12-10/dc17.png)

### IP Fragmentation, Reassembly

- Network links have MTU (max. transfer size) - largest possible link-level frame

  - Different link types, different MTUs

- Large IP datagram divided ("fragmented") within net
  - One datagram becomes several datagrams
  - "Reassembled" only at final destination
  - IP header bits used to identify, order related fragments

![dc18](/assets/images/2023-12-10/dc18.png)

### IP Addressing: Introduction

- IP address: 32-bit identifier for host, router interface

- Interface: connection between host/router and physical link

* Router's typically have multiple interfaces
* Host typically has one or two interfaces (e.g., wired Ethernet, wireless 802.11)

- IP addresses associated with each interface

![dc19](/assets/images/2023-12-10/dc19.png)

![dc20](/assets/images/2023-12-10/dc20.png)

### Subnets

- IP address:

  - Subnet part - high order bits
  - Host part - low order bits

- What's a subnet?
  - Device interfaces with same subnet part of IP address
  - Can physically reach each other without intervening router

![dc21](/assets/images/2023-12-10/dc21.png)

- Recipe
  - To determine the subnets, detach each interface from its host or router, creating islands of isolated networks
  - Each isolated network is called a subnet

![dc22](/assets/images/2023-12-10/dc22.png)

### IP Addressing: CIDR

- CIDR: Classless InterDomain Routing

* Subnet portion of address of arbitrary length
* Address format: a,b,c,d/x, where x is # bits in subnet portion of address

![dc23](/assets/images/2023-12-10/dc23.png)

### IP Addresses: How to Get One?

- Q: How does a host get IP address?
  - Hard-coded by system admin in a file
  - DHCP (Dynamic Host Configuration Protocol) dynamically get address from as server
    - "plug-and-play"

### DHCP: Dynamic Host Configuration Protocol

- Goal: allow host to dynamically obtain its IP address from network server when it joins network

  - Can renew its lease on address in use
  - Allows reuse of addresses (only hold address while connected/"on")
  - Support for mobile users who want to join network (more shortly)

- DHCP overview:
  - Host broadcasts "DHCP discover" msg
  - DHCP server responds with "DHCP offer" msg
  - Host requests IP address: "DHCP request" msg
  - DHCP server sends address: "DHCP ack" msg

### DHCP Client-Server Scenario

![dc24](/assets/images/2023-12-10/dc24.png)

### NAT: Network Address Translation

![dc25](/assets/images/2023-12-10/dc25.png)

![dc26](/assets/images/2023-12-10/dc26.png)

### IPv6: Motivation

- Initial motivation: 32-bit address space soon to be completely allocated.

- Additional motivation:

  - Header format helps speed processing/forwarding
  - Header changes to facilitate QoS

- IPv6 datagram format
  - Priority: identify priority among datagrams in flow
  - Flow Label: identify datagrams in same "flow".
  - Next header: identify upper layer protocol for data

![dc27](/assets/images/2023-12-10/dc27.png)

## Transport Layer: TCP, UDP

### Transport Services and Protocols

- Provide logical communication between app processes running on different hosts

- Transport protocols run in end systems

- Send side: breaks app messages into segments, passes to network layer

- Receive side: reassembles segments into messages, passes to app layer

- More than one transport protocol available to apps

- Internet: `TCP` and `UDP`

![dc28](/assets/images/2023-12-10/dc28.png)

### Transport vs. Network layer

- Network layer: logical communication between `hosts`

- Transport layer: logical communication between `processes`
  - Relies on, enhances, network layer services

![dc29](/assets/images/2023-12-10/dc29.png)

### Internet Transport Layer Protocols

- Reliable, in-order delivery (TCP)

  - Congestion control
  - Flow control
  - Connection setup

- Unreliable, unordered delivery (UDP)

  - No-frills extension of "best-effort" IP

- Services not available:
  - Delay guarantees
  - Bandwidth guarantees

![dc30](/assets/images/2023-12-10/dc30.png)

### Multiplexing/Demultiplexing

![dc31](/assets/images/2023-12-10/dc31.png)

### How Demultiplexing Works

- Host receives IP datagrams

- Each datagram has source IP address, destination IP address

- Each datagram carries one transport-layer segment

- Each segment has source, destination port number

- Host uses IP address & port numbers to direction segment to appropriate socket

![dc32](/assets/images/2023-12-10/dc32.png)

### TCP Overview: RFCs 793, 1122, 1323, 2018, 2581

- Point-to-point:

  - One sender, one receiver

- Reliable, in-order byte stream:

  - No "message boundaries"

- Pipelined:

  - TCP congestion and flow control set window size

- Full duplex data:

  - Bi-directional data flow in same connection
  - MSS: maximum segment size

- Connection-oriented:

  - Handshaking (exchange of control msgs) inits sender, receiver state before data exchange

- Flow controlled

* Sender will not overwhelm receiver

### TCP Sequence Numbers, ACKs

- Sequence numbers:

  - Byte stream "number" of first byte in segment's data

- Acknowledgements:

  - Seq # of next byte expected from other side
  - Cumulative ACK

- Q: how receiver handles out-of-order segments
  - A: TCP spec doesn't say, - up to implementor

![dc33](/assets/images/2023-12-10/dc33.png)

![dc34](/assets/images/2023-12-10/dc34.png)

### TCP: Retransmission Scenarios

![dc35](/assets/images/2023-12-10/dc35.png)

### TCP Fast Retransmit

![dc36](/assets/images/2023-12-10/dc36.png)

### TCP Congestion Control

- Approach: sender increases transmission rate (window size), probing for usable bandwidth, until loss occurs
  - Additive increase: increase cwnd by 1 MSS every RTT (Round Trip Time) until loss detected
  - Multiplicative decrease: cut cwnd in half after loss

![dc37](/assets/images/2023-12-10/dc37.png)

### TCP Slow Start

- When connection begins, increase rate exponentially until first loss event:

  - Initially cwnd = 1 MSS
  - Double cwnd every RTT
  - Done by incrementing cwnd for every ACK received

- Summary: initial rate is slow but ramps up exponentially fast

![dc1](/assets/images/2023-12-11/dc1.png)

### TCP: Detecting, Reacting to Loss

- Loss indicated by timeout:

  - cwnd set to 1 MSS;
  - Window then grows exponentially (as in slow start) to threshold, then grows linearly

- Loss indicated by 3 duplicate ACKs: TCP RENO

  - Dup ACKs indicate network capable of delivering some segments
  - cwnd is cut in half window then grows linearly some(CA, congestion avoidance)

- TCP Tahoe always sets cwnd to 1 (timeout or 3 duplicate acks)

### TCP: Switching from Slow Start to CA

- CA(Congestion Avoidance)

- Q: When should the exponential increase switch to linear?

- A: When cwnd gets to 1/2 of its value before timeout.

- Implementation:

* Variable ssthresh
* On loss event, ssthresh is set to 1/2 of cwnd just before loss event

![dc2](/assets/images/2023-12-11/dc2.png)

### UDP: User Datagram Protocol [RFC 768]

- "No frills", "bare bones" Internet transport protocol

- "Best effort" service, UDP segments may be:

  - Lost
  - Delivered out-of-order to app

- Connectionless:

  - No handshaking between UDP sender, receiver
  - Each UDP segment handled independently of others

- UDP use:

  - Streaming multimedia apps (loss tolerant, rate sensitive)
  - `DNS`
  - SNMP

- Reliable transfer over UDP:
  - Add reliability at application layer
  - Application-specific error recovery!
