# Data Communication 02

## Network Structures

#### : Network criteria, network structures, Internet

#### 2023.10.17.(화)

### Networks

- A network is the interconnection of a set of devices capable of communication.
  - In this definition, a device can be a host such as a large computer, desktop, laptop, workstation, cellular phone, or security system.
  - A device in this definition can also be a connecting device such as a router a switch, a modem that changes the form of data, and so on.

### Nework Criteria

- A network must be able to meet a certain number of criteria. The most important of these are performance, reliability, and security.
  - Performance: measured by
    - Transit time: the amount of time required for a message to travel from one device to another
    - Response time: the elapsed time between an inquiry and a response
    - Metrics: Throughput & Delay
  - Reliability: measured by
    - The frequency of failure
    - The time it takes a link to recover from a failure
    - The network's robustness in a catastrophe
  - Security
    - Protecting data from unauthorized access
    - Protecting data from damage and development
    - Implementing policies and procedures for recovery from breaches and data losses

### Physical Structures

- A network is two or more devices connected through `links`

  - Link: a communication pathway that transfers data from one device to another

- Two possibile types of connections
  - Point-to-point connection:
    - A dedicated link between two devices
    - Using an actual length of wire or cable, microwave or satellite links
  - Multipoint (also called multidrop) connection
    - More than two specific devices share a single link

![datacommunication1](/assets/images/2023-10-17/datacommunication1.png)

- Physical topology
  - The way in which a network is laid out physically
  - The topology of a network: the geometric representation of the relationship of all the links and linking devices (usually called nodes) to one another
  - Mesh, star, bus, and ring topologies

### Physical Structures: Mesh

- Mesh topology
  - Every device has a dedicated point-to-point link to every other device
  - The number of physical links in a fully connected mesh network with n nodes: n(n-1)
    - In duplex mode: n(n-1)/2
  - Advantages
    - Eliminating the traffic problems, Robust, Secure, Easy fault identification & isolation
  - Disadvantages
    - The amount of cabling and number of I/O ports
  - Practical example
    - The connection of telephone regional offices

![datacommunication2](/assets/images/2023-10-17/datacommunication2.png)

### Physical Structures: Star

- Star topology

  - Each device has a dedicated point-to-point link only to a central controller, usually called a hub
  - Advantages
    - Less expensive than a mesh topology
    - Robustness
  - Disadvantages
    - Dependency of the whole topology on one single point, the `hub`
    - More cabling compared with ring and bus
  - Used in local-area networks(LANs)

![datacommunication3](/assets/images/2023-10-17/datacommunication3.png)

### Physical Structures: Bus

- Bus topology
  - Connected to the bus cable by drop lines (connection) and taps (connector)
  - The energy becomes weaker and weaker as it travels farther and farther -> a limit on the number of taps a bus
  - Advantages
    - Ease of installation: only the backbone cable stretches through the entire facility
  - Disadvantages
    - Difficult reconnection and fault isolation
  - One of the first topologies used in the design of early local are networks
    - Traditional Ethernet LANs
    - Less popular now

![datacommunication4](/assets/images/2023-10-17/datacommunication4.png)

### Physical Structures: Ring

- Ring topology
  - Each device has a dedicated point-to-point connection with only the two devices on either side of it
  - Each device in the ring incorporates a repeater
  - Advantages
    - Relatively easy to install and reconfigure
    - Simple fault identification and isolation
  - Disadvantages
    - Unidirectional traffic
    - A break in the ring can disable the entire network -> dual ring or a switch capable of closing off the break
  - IBM local-area network, i.e., Token Ring
    - Now, less popular

![datacommunication5](/assets/images/2023-10-17/datacommunication5.png)

### Network Types

- Different types of networks in the world today
- The criteria of distinguishing one type of network from another is difficult and sometimes confusing.
- We use a few criteria such as size, geographical coverage, and ownership to make this distinction.

### Network Types: Local Area Network

- A local area network (LAN)
  - usually privately owned and connects some hosts in a single office, building, or campus.
  - Depending on the needs of an organization, a LAN can be as simple as two PCs and a printer in someone's home office, or it cann extend throughout a company and include audio and video devices.
  - Each host in a LAN has an identifier, an address, that uniquely defines the host in the LAN.
  - A packet sent by a host to another host carries both the source host's and the destination host's addresses.

* The switch alleviates the traffic in the LAN
  - In the past, all hosts in a network were connected through a common cable

![datacommunication6](/assets/images/2023-10-17/datacommunication6.png)

### Network Types: Wide Area Network

- A wide area network (WAN)
  - A connection of devices capable of communication
  - Some differences between a LAN and a WAN
    - A LAN is normally limited in size; a WAN has a wider geographical span, spanning a town, a state, a country, or even the world.
    - A LAN interconnects hosts; a WAN interconnects connecting devices such as switches, routers, or modems.
    - A LAN is normally privately owned by the organization that uses it; a WAN is normally created and run by communication companies and leased by an organization that uses it.

### Network Types: Wide Area Network

- Point-to-point WAN
  - A network that connects two communicating devices through a transmission media (cable or air)

![datacommunication7](/assets/images/2023-10-17/datacommunication7.png)

- Switched WAN
  - A network with more than two ends
  - Used in the backbone of global communication today

![datacommunication8](/assets/images/2023-10-17/datacommunication8.png)

- Internetwork
  - Very rare to see a LAN and a WAN in isolation
  - When two or more networks are connected, they make an internetwork, or internet
  - E.g., to make the communication between employees at different offices possible, the management leases a point-to-point dedicated WAN
  - An internework make of two LANs and one point-to-point WAN

![datacommunication9](/assets/images/2023-10-17/datacommunication9.png)

- Another internet with several LANs and WANs connected
  - One of the WANs is a switched WAN with four switched
  - A heterogeneous network consists of four WANs and three LANs

![datacommunication10](/assets/images/2023-10-17/datacommunication10.png)

### Switching

- An internet is a switched network in which a switch connects at least two links together.
- A switch needs to forward data from a network to another network when required.
- The two most common types of switched networks are
  - Circuit-switched
  - Packet-switched networks...

### Circuit Switching

- End-end resources allocated to, reserved for "call" between source & destination:

* In diagram, each link has four circuits.

  - Call gets 2nd circuit in top link and 1st circuit in right link.

* Dedicated resources: no sharing

  - Circuit-like (guaranteed) performance

* Circuit segment idle if not used by call (no sharing)

* Commonly used in traditional telephone networks

![datacommunication11](/assets/images/2023-10-17/datacommunication11.png)

### Packet Switching

- The communication between the two ends is done in blocks of data called packets

- Not continuous communication, but exchange of individual data packets when they are being used

- Switch: both storing and forwarding

  - A packet is an independent entity that can be stored and sent later

- A router in a packet-switched network

  - A queue that can store and forward the packet

- More efficient than a circuit switched network, but some delays for packet transmission

![datacommunication12](/assets/images/2023-10-17/datacommunication12.png)

### Packet switching versus circuit switching

- Packet switching allows more users to use network!

- Example

  - 1 Mb/s link
  - each user
    - 100 kb/s when "active"
    - Active 10% of time

- Circuit switching:

  - 10 users

- Packet switching:
  - With 35 users, probability > 10 active at same time is less than 0.0004

![datacommunication13](/assets/images/2023-10-17/datacommunication13.png)

### Packet switching versus circuit switching

- Is packet switching always much better?

  - Great for bursty data
    - Resource sharing
    - Simpler, no call setup
  - Excessive congestion possible: packet delay and loss
    - Protocols needed for reliable data transfer, congestion control
  - How to provide circuit-like behavior?
    - Bandwidth guarantees needed for audio/video apps
    - Still an unsolved problem

- Human analogies of reserved resources (circuit switching) versus on-demand allocation (packet-switching)

### Internet Structure: Network of Networks

- End systems connect to Internet via access ISPs (Internet Service Providers)

  - Residential, company and university ISPs

- Access ISPs in turn must be interconnected

  - So that any two hosts can send packets to each other

- Resulting network of networks is very complex

  - Evolution was driven by economics and national policies

- Let's take a stepwise approach to describe current Internet structure

- Given millions of access ISPs, how to connect them together?

- Option: connect each access ISP to every other access ISP?

![datacommunication14](/assets/images/2023-10-17/datacommunication14.png)

- Option: connect each access ISP to one global transit ISP?

- Customer and provider ISPs have economic agreement

![datacommunication15](/assets/images/2023-10-17/datacommunication15.png)

- But if one global ISP is viable business, there will be competitors ...

![datacommunication16](/assets/images/2023-10-17/datacommunication16.png)

- But if one global ISP is viable business, there will be competitors ... which must be interconnected

![datacommunication17](/assets/images/2023-10-17/datacommunication17.png)

- ... and regional networks may arise to connect access nets to ISPs

![datacommunication18](/assets/images/2023-10-17/datacommunication18.png)

- ... and content provider networks (e.g., Google, Microsoft, Akamai) may run their own network, to bring services, content close to end users

![datacommunication19](/assets/images/2023-10-17/datacommunication19.png)

- At center: small # of well-connected large networks
- "Tier-1" commercial ISPs (e.g., Level 3, Sprint, AT&T, NTT), national & international coverage
- Content provider network (e.g., Google): private network that connects it data centers to Internet, often bypassing tier-1, regional ISPs

![datacommunication20](/assets/images/2023-10-17/datacommunication20.png)

### Important Standards

- RFC: Internet protocols (application, transport, network, (link) layers)

  - RFC791 (IP), RFC817(TCP)

- IEEE 802 standars: physical & link layer protocols

  - 802.3 (Ethernet), 802.11 (WiFi), 802.15 (Wireless PAN)

- 3GPP: focusing on wireless cellular networks (Big market)
  - LTE (Release 8 @ 2008), LTE-Advanced (Release 10 @ 2011), 5G (Release 15 @2018, 16 @2019)
  - Now, Release 17

## Network Models

#### : Protocol layering, TCP/IP protocol suite, OSI models

### Protocol Layering

- A word we hear all the time when we talk about the Internet is `protocol`.

- A protocol defines the `rules` that both the sender and receiver and all intermediate devices need to follow to be able to communicate effectively.

- When communication is simple, we may need only one simple protocol; when the communication is complex, we need a protocol at each layer, or protocol layering.

### Scenarios

- Two simple scenarios to better understand the need for protocol layering.
  - In the first scenario, communication is so simple that it can occur in only one layer.
  - In the second, the communication between Maria and Ann takes place in three layers.

### Scenarios: A Single Layer

- A single-layer protocol
  - Greet each other when they meet
  - Confine their vocabulary to the level of their friendship
  - Refrain from speaking when the other party speaking
  - A dialog, not a monolog
  - Exchange some nice words when they leave
    -> Different from the communication between a professor and the student in a lecture hall

![datacommunication21](/assets/images/2023-10-17/datacommunication21.png)

- A three-layer protocol
  - Far communication between Maria and Ann

![datacommunication22](/assets/images/2023-10-17/datacommunication22.png)

### Scenarios

- Advantages of protocol layering

  - Division a complex task into several smaller and simpler task
  - Modularity
    - If two machines provide the same outputs when given the same inputs, they can replace each other
  - Separation of the services from the implementation
  - Intermediate systems, but not all layers
    - The whole system more expensive

- Disadvantage of protocol layering
  - A single layer makes the job easier
  - Cross-layer optimization

### Principles of Protocol Layering

- Two principles of protocol layering
  - The first principle dictates that if we want bidirectional communication, we need to make each layer so that it is able to perform two opposite tasks, one in each direction.
  - The second principle that we need to follow in protocol layering is that the two objects under each layer at both sites should be identical.

### Logical Connections

- Logical connection between each layer

  - This means that we have layer-to-layer communication
  - Maria and Ann can think that there is a logical (imaginary) connection at each layer through which they can send the object created from that layer.
  - We will see that the concept of logical connection will help us better understand the task of layering we encounter in data communication and networking.

- Logical connection between peer layers

![datacommunication23](/assets/images/2023-10-17/datacommunication23.png)

### TCP/IP Protocol Suite

- Protocol
  - A protocol defines the rules that both the sender and receiver and all intermediate devices need to follow to be able to communicate effectively.
  - When communication is simple, we may need only one simple protocol; when the communication is complex, we need a protocol at each layer, or protocol layering.

![datacommunication24](/assets/images/2023-10-17/datacommunication24.png)

### Layered Architecture

- Communication through an internet
  - The suite in a small internet made up of three LANs (links), each with a link-layer switch.
  - The links are connected by one router
  - Router
    - Only three layers
    - Always involvedin one network layer
    - involved in n combinations of link and physical layers in which n is the number of links the router is connected to
  - A link layer switch in a link
    - Only two layers
    - Using only one set of protocols

![datacommunication25](/assets/images/2023-10-17/datacommunication25.png)

### Layers in the TCP/IP Protocol Suite

- Layers in the TCP/IP protocol suite
  - Each layer is discussed in detail in the next five parts of the book.
  - Logical connections between layers.

![datacommunication26](/assets/images/2023-10-17/datacommunication26.png)

### Layers in the TCP/IP Protocol Suite

- End-to-end logical connection (domain: internet)

  - The duty of the application, transport, and network layers

- Hop-to-hop logical connection (domain: link)

  - Data link and physical layers
  - Hop: host or router

- Identical objects in the TCP/IP protocol suite

![datacommunication27](/assets/images/2023-10-17/datacommunication27.png)

### Description of Each Layer

- The discussion will be very brief, but we come back to the duty of each layer in next classes.

- Physicallayer or PHY layer

  - Carrying individual bits in a frame across the link
  - Still logical communication
    - Hidden layer: the transmission media under the physical layer
    - The transmission media carries electrical or optical signals
  - The logical unit between two physical layers in two devices is a bit
  - Several protocols that transform a bit to a signal

- Data link layer

  - Taking the datagram and moving it across the link
    - Routers: choosing the best links
    - An internet is make up of serverl links connected by routers
  - E.g., wired LAN (Ethernet), wireless LAN, wired WAN, wireless WAN (LTE, 5G NR)
  - This layer takes a datagram and encapsulates it in a packet called a frame
  - Some link layer protocols provide
    - Complete error detection and correction
    - Or only error correction

- Network layer

  - Host-to-host communication and routing the packet through possible routes
    - Communication at the network is host-to-host
    - Choosing the best route for each packet
  - The network layer in the Internet: Internet Protocol (IP)
    - Connectionless protocol
      - No flow control, no error control, and no congestion control services
      - Unicast (one-to-one), multicast (one-to-many)
  - Auxiliary protocols that help IP in its delivery and routing tasks
    - Internet Control Message Protocol (ICMP)
    - Internet Group Management Protocol (IGMP)
    - Dynamic Host Configuration Protocol (DHCP)
    - Address Resolution Protocol (ARP)

- Transport layer

  - Giving services to the application layer
    - Getting message from the application layer, encapsulating it in a transport layer packet (called a segment or a user datagram), and sending it
  - Transmission Control Protocol (TCP)
    - Connection-oriented protocol
    - Creating a logical pipe between two TCPs for transferring a stream of bytes
    - Flow control: matching the sending data rate of the source host with the receiving data rate of the destinatioin host to prevent overwhelming the destination
    - Error control: guaranteeing that the segments arrive at the destination without error and resending the corrupted ones
    - Congestion control: reducing the loss of segments due to congestion in the network

  * User Datagram Protocol (UDP)
    - Connectionless protocol, small overhead (e.g., short messages)
  * Stream Control Transmission Protocol (STCP) for multimedia

* Application layer
  - End-to-end logical connection
  - Process-to-process communication
  - Hypertext Transfer Protocol (HTTP): a vehicle for accessing the World Wide Web(WWW)
  - Simple Mail Transfer Protocol (SMTP): the main protocol used in e-mail
  - File Transfer Protocol (FTP): transferring files from one host to another
  - Terminal Network (TELNET), Secure Shell (SSH): for accessing a site remotely
  - Simple Network Management Protocol (SNMP): used by an administrator to manage the Internet at global and local levels
  - Domain Name System (DNS): used by other protocols to find the network-layer address of a computer

### Encapsulation and Decapsulation

- Encapsulation & decapsulation
  - One of the important concepts in protocol layering in the Internet
  - Encapsulation in the source host
  - Decapsulation in the destination host
  - Encapsulation and decapsulation in the router

![datacommunication28](/assets/images/2023-10-17/datacommunication28.png)

- Application layer: message

  - Normally not contain any header or trailer

- Transport layer: segment (in TCP), user datagram (in UDP)

  - Header: identifiers of source and destinatio programs for flow, error control, congestion control

- Network layer: datagram

  - Header: source and destination addresses, information for error checking and fragmentation

- Data link layer: frame
  - Link layer address of the host or next hop

### Addressing

- Addressing
  - Any communication that involves two parties needs two address:
    - Source address and destination address
    - Normally have only four because the physical layer does not need addresses; the unit of data exchange at the physical layer is a bit, which definitely cannot have an address

![datacommunication29](/assets/images/2023-10-17/datacommunication29.png)

### Multiplexing and Demultiplexing

- Multiplexing at the source and demultiplexing at the destination

  - TCP/IP protocol suite uses several protocols at some layers
  - Multiplexing in this case means that a protocol at a layer can encapsulate a packet from several next-higher layer protocols (one at a time)
  - Demultiplexing means that a protocol can decapsulate and deliver a packet to several next-higher layer protocols (one at a time)
  - The following shows the concept of multiplexing and demultiplexing at the three upper layers.

  ![datacommunication30](/assets/images/2023-10-17/datacommunication30.png)

### OSI Model

- A protocol defines the rules that both the sender and receiver and all intermediate devices need to follow to be able to communicate effectively.

- When communication is simple, we may need only one simple protocol; when the communication is complex, we need a protocol at each layer, or protocol layering.

- International organization for standardization (ISO)

  - A multinational body dedicated to worldwide agreement on international standards

- Open systems interconnection (OSI) model

  - An ISO standard that covers all aspects of network communications

- Open system

  - A set of protocols that allows any two different systems to communicate regardless of their underlying architecture

- The OSI Model

![datacommunication31](/assets/images/2023-10-17/datacommunication31.png)

### OSI versus TCP/IP

- Two layers: `session` and `presentation`
  - The application layer in the suite is usually considered to be the combination of three layers in the OSI model.

![datacommunication32](/assets/images/2023-10-17/datacommunication32.png)

### Lack of OSI Model's Success

- The OSI model appeared after the TCP/IP protocol suite.

- Most experts were at first excited and thought that the TCP/IP protocol would be fully replaced by the OSI model.

- This did not happen for several reasons, but we describe only three, which are agreed upon by all experts in the field.
  - OSI was completed when TCP/IP was fully in place
  - Some layers in the OSI model were nevery fully defined
  - Not a high enough level of performance
