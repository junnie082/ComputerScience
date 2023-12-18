# Data Communication 11

## Wired LAN (Ethernet) & Wireless LAN (WiFi)

#### 2023.12.12.(화)

## Wired LAN (Ethernet)

## : 802.3, CSMA/CD, Ethernet switch

### Ethernet

- "Dominant" `wired LAN` technology:
  - Single chip, multiple speeds
  - `First widely` used LAN technology
  - Simpler, cheap
  - Kept up with speed race: 10 Mbps - 10 Gbps

![dc40](/assets/images/2023-12-12/dc40.png)

### Ethernet: Physical Topology

- `Bus`: popular through mid 90s

  - All nodes in same collision domain (can collide with each other)

- `Star`: prevails today
  - Active switch in center
  - Each "spoke" runs a (separate) Ethernet protocol (nodes do not collide with each other)

![dc41](/assets/images/2023-12-12/dc41.png)

### Ethernet: Connectionless & Unreliable

- `Connectionless` and `Unreliable` Service

* Each frame sent is independent of the previous or next frame
* No connection establishment or connection termination phases
* If a frame drops, the sender will not know about it
  - If the transport layer is also a connectionless protocol, such as UDP, the frame is lost and salvation may only come from the application layer.
  - However, if the transport layer is TCP, the sender TCP does not receive acknowledgment for its segment and sends it again
* Unreliable like IP and UDP
* CRC-32
* Receiver drops the frame silently.
* Duty of high-level protocols to find out about it

### Frame Format

- Frame format of Ethernet (IEEE 802.3)

![dc42](/assets/images/2023-12-12/dc42.png)

- Preamble: alternating 0s and 1s

  - Alert the receiving system to the coming frame
  - Enable the receiver to synchronize its clock

- Start frame delimiter (SFD): 1 byte of 10101011

  - (11)2 and alert the receiver that the next field is the destination address
  - SFD field is added at the physical layer

- `Destination address` (DA)

  - Unicast, multicast, and broadcast
  - Decapsulate the data from the frame and passes the data to the upperlayer protocol defined by the value of the type field

- `Source address` (SA)

  - Link-layer address of the sender

- `Type`

  - Upper-layer protocol whose packet is encapsulated
  - IP, ARP, OSPF, and so on

- Data

  - A minimum of 46 and a maximum of 1500 bytes
  - If the data coming from the upper layer is more than 1500 bytes, it should be fragmented
  - If it is less than 46 bytes, it needs to be padded with extra 0s

- CRC: CRC-32

  - CRC is calculated over the addresses, types, and data field

- Frame Length
  - Minimum frame length: 64 bytes
  - Maximum frame length: 1518 bytes
  - Minimum data length: 46 bytes
  - Maximum data length: 1500 bytes
  - Maximum length restriction
    - Memory was very expensive
    - Preventing one station from momopolizing the shared medium

### Collision Detection for CSMA/CD

- Energy Level
  - Energy level during transmission, idleness, or collision

![dc43](/assets/images/2023-12-12/dc43.png)

### Minimum Frame Size for CSMA/CD

- Minimum Frame Size

  - Need a restriction on the frame size
  - Once the entire frame is sent, the station does not keep a copy of the frame and does not monitor the line for collision detection
  - Therefore, the frame transmission time Tfr must be at least two times the maximum propagation time Tp

- Example
  - A network using CSMA/CD has a bandwidth of 10 Mbps. If the maximum propagation time (including the delays in the devices and ignoring the time needed to send a jamming signal, as we see later) is 25.6 μs, what is the minimum size of the frame?
  - Solution
    - The minimum frame transmissin time is Tfr = 2 x Tp = 51.2 . This means, in the worst case, a station needs to transmit for a period of 51.2 μs to detect the collision. The minimum size of the frame is 10 Mbps x 51.2 μs = 512 bits or 64 bytes. This is actually the minimum size of the frame for Standard Ethernet, as we will see later in the chapter.

### Addressing

- Each station on an Ethernet network (such as a PC, workstation, or printer) has its own network interface card (NIC)

- The `Ethernet address` is 6 bytes (48 bits), normally written in hexadecimal notation

  - E.g., 4A:30:10:21:10:1A

- Transmission of Address Bits

  - Left to right, byte by byte
  - For each byte, the least significant bit (LSB) is sent first and the most significant bit (MSB) is sent last
  - Immediately know if the packet is `unicast` or `multicast`

- Example
  - Show how the address 47:20:1B:2E:08:EE is sent out online.
  - Solution
    - The address is sent left to right, byte by byte; for each byte, it is sent right to left, bit by bit, as shown below

![dc44](/assets/images/2023-12-12/dc44.png)

- Unicast, Multicast, and Broadcast Addresses
  - Unicast and multicast addresses

![dc45](/assets/images/2023-12-12/dc45.png)

- Example

  - Define the type of the following destination addresses:
    a. 4A:30:10:21:10:1A
    b. 47:20:1B:2E:08:EE
    c. FF:FF:FF:FF:FF:FF
  - Solution
    a. This is a `unicast` address because A in binary is 1010 (even).
    b. This is a `multicast` address because 7 in binary is 0111 (odd).
    c. This is a `broadcast` address because all digits are F's in hexadecimal.

- Distinguish Between Unicast, Multicast, and Broadcast Transmission (cont.)
  - Transmission in the standard Ethernet is always broadcast
  - If hub is a passive element
    - Not check the destination address of the frame;
    - Regenerate the bits (if they have been weakened) and send them to all stations except station A
  - In a unicast transmission,
    - All stations will receive the frame, the intended recipient keeps and handles the frame; the rest discard it.
  - In a multicast transmission,
    - All stations will receive the frame, the stations that are members of the group keep and handle it; the rest discard it.
  - In a broadcast transmission
    - All stations (except the sender) will receive the frame and all stations (except the sender) keep and handle it.

### 802.3 Ethernet standards: link & physical layers

- Many different Ethernet standards

* Common MAC protocol and frame format
* Different speeds: 2 Mbps, 10 Mbps, 100 Mbps, 1 Gbps, 10 Gbps, 40 Gbps, 100 Gbps
* Different physical layer media: fiber, cable
* E.g., 100 Mbps Ehternet standards: a common link layer, different physical layers

![dc46](/assets/images/2023-12-12/dc46.png)

### Various 802.3 Ethernet Standards

![dc47](/assets/images/2023-12-12/dc47.png)

### Ethernet Switch

- Link-layer device: takes an active role

  - Store, forward Ethernet frames
  - Examine incoming frame's MAC address, selectively forward frame to one-or-more outgoing links when frame is to be forward on segment

- Transparent

  - Hosts are unaware of presence of switches

- Plug-and-play, self-learning
  - Switches do not need to be configured

### Switch: Multiple Simultaneous Transmissions

- Hosts have dedicated, direct connection to switch

- Switches buffer packets

- Ethernet protocol used on each incoming link, but no collisions; full duplex

  - Each link is its own collision domain

- Switching: A-to-A' and B-to-B' can transmit simultaneously, without collisions

![dc48](/assets/images/2023-12-12/dc48.png)

### Switch Forwarding Table

- Q: How does switch know A' reachable via interface 4, B' reachable via interface 5?

- A: Each switch has a `switch table`, each entry:

  - MAC address of host, interface to reach host, time stamp
  - Looks like a routing table!

- Q: how are entries created, maintained in switch table?
  - Something like a routing protocol?

### Switch: Self-Learning

- Switch learns which hosts can be reached through which interfaces
  - When frame received, switch "learns" location of sender: incoming LAN segment
  - Records sender/location pair in switch table

![dc49](/assets/images/2023-12-12/dc49.png)

| MAC addr | interface | TTL |
| :------: | :-------: | :-: |
|    A     |     1     | 60  |

Switch table (initially empty)

### Switch: Frame Filtering/Forwarding

- When frame received at switch:

1. Record incoming link, MAC address of sending host
2. Index switch table using MAC destination address
3. If entry found for destination
   then {
   if destination on segment from which frame arrived then drop frame
   else forward frame on interface indicated by entry
   }
   Else flood /_ forward on all interfaces except arriving interface _/

### Self-learning, Forwarding: Example

- frame destination, A', location unknown: floot

- destination A location known: selectively send on just one link

![dc50](/assets/images/2023-12-12/dc50.png)

| MAC addr | interface | TTL |
| :------: | :-------: | :-: |
|    A     |     1     | 60  |
|    A'    |     4     | 60  |

switch table (initially empty)

## Wireless LAN (WiFi)

## : 802.11, CSMA/CA

### Wireless Transmission Rates and Range

- Wireless transmission rates and range
  - For WiFi, cellular 4G/5G and Bluetooth standards (note: axes are not linear)

### IEEE 802.11 Project

- IEEE has defined the specifications for a wireless LAN

  - Called IEEE 802.11, which covers the physical and data-link layers
  - Sometimes called wireless Ethernet

- WiFi (short for wireless fidelity) as a synonym for wireless LAN
  - WiFi is a wireless LAN
  - Certified by the WiFi Alliance, a global, nonprofit industry association of more than 300 member companies devoted to promoting the growth of wireless LANs.
