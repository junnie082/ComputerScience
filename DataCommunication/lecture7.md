# Data Communication 07

## Transmission Media, Switching

#### 2023.10.20.(금)

### Transmission Media

#### : Guided and Unguided Media

### Introduction

- Transmission media

  - Actually located below the physical layer and are directly controlled by the physical layer
  - Broadly defined as anything that can carry information from a source to a destination
  - Usually free space, metallic cable, or fiber-optic cable

- Transmission media and physical layer

![datacommunication1](/assets/images/2023-10-20/datacommunication1.png)

- History

  - Long-distance communication using electric signals
    - started with the invention of the telegraph by Morse in the 19th century
    - Communication by telegraph was slow and dependent on a metallic medium
  - Human voice
    - The telephone was invented in 1869
  - Wireless communication
    - Started in 1895 when Hertz was able to send high frequency signals.
    - Later, Marconi devised a method to send telegraph-type messages over the Atlantic Ocean

- Classes of transmission media

![datacommunication2](/assets/images/2023-10-20/datacommunication2.png)

### Guided Media

- Differences
  - material used for wiring, protective layers, bandwidth and data rates

![datacommunication3](/assets/images/2023-10-20/datacommunication3.png)

- Guided media

  - Provide a conduit from one device to another
  - Signal traveling directed and contained by the physical limits of medium

- Include
  - Twisted-pair cable
  - Coaxial cable
  - Fiber-optic cable

### Twisted-Pair Cable

- A twisted pair consists of two conductors (normally copper), each with its own plastic insulation, twisted together

  - One of the wires is used to carry signals to the receiver
  - The other is used only as a ground reference
  - The receiver users the difference between the two

- Twisted-pair cable

![datacommunication4](/assets/images/2023-10-20/datacommunication4.png)

- Unwanted signals

  - Interference (noise) and crosstalk

- Twisting

  - If the two wires are parallel, the effect of these unwanted signals is not the same in both wires
  - By twisting the pairs, a balance is maintained
    - Twisting makes it probable that both wires are equally affected by external influences (noise or crosstalk)
    - The unwanted signals are mostly canceled out.
    - The number of twists per unit of length (e.g., inch) has some effect on the quality of the cable.

- Unshielded Versus Shielded Twisted-Pair Cable
  - Unshielded twisted-pair (UTP).
    - Most common twisted-pair cable
  - Shielded twisted-pair (STP).
    - IBM has produced a version of twisted-pair cable
    - Although metal casing improves the quality of cable by preventing the penetration of noise or crosstalk, it is bulkier and more expensive.

![datacommunication6](/assets/images/2023-10-20/datacommunication6.png)

- Categories of UTP cables
  - The Electronic Industries Association (EIA) has developed standards to classify unshielded twisted-pair cable into seven categories
  - Categories are determined by cable quality
    - Cat1: not twisted
    - Cat3,4,5: 4 pairs
    - Cat5: more twists
    - Cat6: physical separtor between four pairs
    - Cat7: Additional cable shield

![datacommunication7](/assets/images/2023-10-20/datacommunication7.png)

### Twisted-Pair Cable: Categories of UTP Cables

![datacommunication8](/assets/images/2023-10-20/datacommunication8.png)

### Twisted-Pair Cable

- Connectors

  - RJ45 (registered jack): most common UTP connector

- UTP Connectors

![datacommunication9](/assets/images/2023-10-20/datacommunication9.png)

![datacommunication10](/assets/images/2023-10-20/datacommunication10.png)

- Performance

  - Compare attenuatioin versus frequency and distance
  - `Gauge` is a measure of the thickness of the wire

- UTP Performance

![datacommunication11](/assets/images/2023-10-20/datacommunication11.png)

- Applications
  - Twisted-pair cables are used in telephone lines to provide voice and data channels
  - Logical loop
    - Line that connects subscribers to the central telephone office
    - Commonly consists of unshielded twisted-pair cables
  - `DSL` lines
    - used by the telephone companies to provide high-data-rate connections
  - `Local-area networks`, such as 10Base-T and 100Base-T

### Coaxial Cable

- `Coaxial` cable (or coax)
  - Carries signals of higher frequency ranges than those in twisted pair cable
  - A central core conductor of solid or standard wire (usually copper) enclosed in an insulating sheath, which is, in turn, encased in an outer conductor of metal foil, braid, or a combination of the two
  - The outer metallic wrapping serves both as a shield against noise and as the second conductor

![datacommunication12](/assets/images/2023-10-20/datacommunication12.png)

- Coaxial Cable Standards

  - Coaxial cables are categorized by their `Radio Government (RG)` ratings
  - Each RG number denotes a unique set of physical specifications,
    - including the wire gauge of the inner conductor, the thickness and type of the inner insulator, the construction of the shield, and the size and type of the outer casing

- Categories of coaxial cables

![datacommunication13](/assets/images/2023-10-20/datacommunication13.png)

- Coaxial Cable Connectors

  - Bayonet Neill-Concelman (BNC) connector
    - The most common type of connector used today
    - Used to connect the end of the cable to a device, such as a
    - TV set

- BNC connectors

![datacommunication14](/assets/images/2023-10-20/datacommunication14.png)

- Performance
  - the attenuation is much higher than in coaxial cable than in twisted-pair cable
  - Although coaxial cable has a much higher bandwidth, the signal weakens rapidly and requires the frequent use of repeaters.

![datacommunication15](/assets/images/2023-10-20/datacommunication15.png)

- Applications
  - Coaxial cable was widely used in analog telephone networks where a single coaxial network could carry 10,000 voice signals.
  - Later it was used in digital telephone networks where a single coaxial cable could carry digital data up to 600 Mbps.
  - Cable TV networks also use coaxial cables.
    - Cable TV uses RG-59 coaxial cable.
  - Another common application of coaxial cable is in traditional Ethernet LANs
  - Replaced today with fiber optic cable.

### Fiber-Optic Cable

- A fiber-optic cable is made of glass of plastic and transmits signals in the form of light.

- Nature of light

  - Light travels in a straight line as long as it is moving through a single uniform substance.
  - If the angle is greater than the critical angle, the ray reflects (makes a turn) and travels again in the denser substance
  - Critical angle is a property of the substance

- Bending of light ray

![datacommunication16](/assets/images/2023-10-20/datacommunication16.png)

- Optical fibers user reflection to guide light through a channel.

- A glass of plastic core is surrounded by a cladding of less dense glass or plastic.

- Optical fiber

![datacommunication17](/assets/images/2023-10-20/datacommunication17.png)

- Propagation Modes

  - Current technology supports two modes (multimode and single mode) for propagating light along optical channels
    - Each requiring fiber with different physical characteristics

- Propagation modes

![datacommunication18](/assets/images/2023-10-20/datacommunication18.png)

- Multimode

  - `Multiple beams` from a light source move through the core in different paths
  - Multimode step-index fiber
    - Density of the core remains constant from the center to the edges
  - Multimode graded-index fiber
    - Decrease this distortion of the signal through the cable.
    - The word index here refers to the index of refraction.

- Modes

![datacommunication19](/assets/images/2023-10-20/datacommunication19.png)

- Single-Mode

  - Single-mode uses step-index fiber and a highly focused source of light that limits beams to a small range of angles, all close to the horizontal.

- Fiber Sizes
  - Fiber types (hair thickness ~ 100μm)

![datacommunication20](/assets/images/2023-10-20/datacommunication20.png)

- Cable Composition

  - The outer jacket is made of either PVC

- Fiber construction

![datacommunication21](/assets/images/2023-10-20/datacommunication21.png)

- Fiber-Optic Cable Connectors

  - Subscriber channel (SC) connector: cable TV.
  - Straight-tip (ST) connector: cable to networking devices
  - MT-RJ connector: the same size as RJ45

- Fiber-optic cable connector

![datacommunication22](/assets/images/2023-10-20/datacommunication22.png)

- Performance

  - Attenuation is flatter than in the case of twisted-pair cable and coaxial cable
  - Fewer (actually one tenth as many) repeaters when we use fiber-optic cable

- Optical fiber performance

![datacommunication23](/assets/images/2023-10-20/datacommunication23.png)

- Applications

  - Fiber-optic cable is often found in `backbone networks` because its wide bandwidth is cost-effective.
  - Today, with wavelength-division multiplexing (WDM), we can transfer data at a rate of 1600 Gbps.
  - Some cable TV companies use a combination of optical fiber and coaxial cable, thus creating a hybrid network.

- Advantages of Optical Fiber

  - Higher bandwidth
  - Less signal attenuation
  - Immunity to electromagnetic interference
  - Resistance to corrosive materials
  - Light weight
  - Greater immunity to tapping

- Disadvantages of Optical Fiber
  - Installatioin and maintenance
  - Unidirectional light propagation
  - Cost

### Unguided Media

- `Unguided` medium

  - Transport electromagnetic waves without using a physical conductor.
  - Often referred to as wireless communication
  - Signals are normally broadcast through free space and thus are available to anyone who has a device capable of receiving them.

- Electromagnetic spectrum for wireless communication

![datacommunication24](/assets/images/2023-10-20/datacommunication24.png)

- Propagation methods
  - In ground propagation, radio waves travel through the lowest portion of the atmosphere, hugging the earth.
  - In sky propagation, higher-frequency radio waves radiate upward into the ionosphere (the layer of atmosphere where particles exist as ions) where they are reflected back to earth.
  - In line-of-sight propagation, very high-frequency signals are transmitted in straight lines directly from antenna to antenna.

![datacommunication25](/assets/images/2023-10-20/datacommunication25.png)

- Radio waves and microwaves is divided into eight ranges

  - Called bands, each regulated by government authorities

- Bands
  - Almost the entire band is regulated by authorities (e.g., the FCC in the United States).

![datacommunication26](/assets/images/2023-10-20/datacommunication26.png)

### Radio Waves

- Electromagnetic waves ranging in frequencies between 3 kHz and 1 GHz are normally called radio waves

  - Omnidirectional
  - Waves that propagate in the sky mode, can travel long distances
  - Low and medium frequencies, can penetrate walls

- Omnidirectional Antenna

  - Radio waves use omnidirectional antennas that send out signals in all directions

- Applications
  - Radio waves are used for multicast communications, such as `radio (AM, FM), television, and paging systems`

### Microwaves

- Electromagnetic waves having frequencies between 1 and 300 GHz are called microwaves.

  - Unidirectional
  - Microwave propagation is line-of-sight
    - Repeaters are often needed for long distance communication.
  - Very high-frequency microwaves cannot penetrate walls
  - The microwave band is relatively wide, almost 299 GHz

- For mobile communications

  - Sub-6GHz for 4G, 5G: 2, 3.5, 4, 6 GHz
  - Millimeter waves (mmWave): eg.g., 28 GHz
  - Upper mid-band for 6G: 7-24 GHz
  - Sub-THz for 6G: 92-300 GHz

- Application: used for unicast communication such as

  - cellular telephones, satellite networks, and wireless LANs

- Various antennas
  - Unidirectioinal Antenna

![datacommunication1](/assets/images/2023-10-21/datacommunication1.png)

- Antenna for mobile communications (omni-directional, sectorized, multiple antennas)

![datacommunication2](/assets/images/2023-10-21/datacommunication2.png)

### Infrared

- With frequencies from 300 GHz to 400 THz (wavelengths from 1 mm to 770 nm)

- Used for short-range communication

  - Infrared remote control
  - Useless for long-range communication
  - In addition, we cannot use infrared waves outside a building because the sun's rays contain infrared waves that can interfere with the communication

- Applications
  - The infrared band, almost 400 THz, has an excellent potential for data transmission
  - The Infrared Data Association (IrDA), an association for sponsoring the use of infrared waves
  - used for short-range communication in a closed area using line-of-sight propagation

![datacommunication3](/assets/images/2023-10-21/datacommunication3.png)

![datacommunication4](/assets/images/2023-10-21/datacommunication4.png)

## Switching & Performances

### : Circuit vs. Packet Switching, Delay/Packet Loss/Throughput

### Introduction to Switching

- A network is a set of connected devices

- Problem of how to connect them to make one-to-one communication possible

  - Point-to-point connection
    - Mesh topology, or a star topology
    - Too much infrastructure to be cost-efficient
  - Multipoint connections, such as a bus
    - Distances between devices and the total number of devices are limited

- A better solution: switching

  - A series of interlinked nodes, called switches
  - Switches are devices capable of creating temporary connections between two or more devices linked to the switch.

- Switched network

![datacommunication6](/assets/images/2023-10-21/datacommunication6.png)

### [Review] Switchiing

- An `internet` is a switched network in which a switch connects at least two links together.

- A `switch` needs to forward data from a network to another network when required.

- The two most common types of switched networks are
  - `Circuit-switched` networks
  - `Packet-switched` networks

### [Review] Circuit Switching

- End-end resources allocated to, reserved for "call" between source & destination:

- In diagram, each link has four circuits.

  - Call gets 2nd circuit in top link and 1st circuit in right link.

- Dedicated resources: no sharing

  - Circuit-like (guaranteed) performance

- Circuit segment idle if not used by call (no sharing)

- Commonly used in traditional telephone networks

![datacommunication7](/assets/images/2023-10-21/datacommunication7.png)

### Circuit switching: FDM vs. TDM

![datacommunication8](/assets/images/2023-10-21/datacommunication8.png)

### Circuit-Switched Networks

- Setup phase (connection established)

  - When end system A needs to communicate with end system M, system A needs to request a connection to M that must be accepted by all switches as well as by M itself.
  - A circuit (channel) is reserved on each link, and the combination of circuits or channels defines the dedicated path
  - Resources: such as channels (bandwidth in FDM and time slots in TDM), switch buffers, switch processing time, and switch input/output ports

- Data-transfer phase

  - After the dedicated path made of connected circuits (channels) is established, the data-transfer phase can take place
  - Data transferred between the two stations are not packetized
  - No addressing involved during data transfer

- Teardown phase (connection termination)
  - After all data have been transferred, the circuits are torn down

### Efficiency

- Circuit-switched networks are not efficient

- In a telephone network,

  - People normally teminate the communication when they have finished their conversation.

- However, in computer networks,
  - A computer can be connected to another computer even if there is no activity for a long time
  - In this case, allowing resources to be dedicated means that other connections are deprived

### Delay

- Low efficiency, but the delay in this type of network is minimal

- No waiting time at each switch

- The total delay is due to the time needed to create the connection, transfer data, and disconnect the circuit

  - Delay caused by the setup
    - Propagation time of the source computer request
    - Request signal transfer time
    - Propagation time of the acknowledgment from the destination computer
    - Signal transfer time of the acknowledgment
  - Delay due to data transfer
    - Propagation time
    - Data transfer time
  - Time needed to tear down the circuit

- Delay in a circuit-switched network

![datacommunication9](/assets/images/2023-10-21/datacommunication9.png)

### [Review] Packet Switching

- The communication between the two ends is done in blocks of data called `packets`.

- Not continuous communication, but exchange of individual data packets when they are being used

- Switch: both storing and forwarding

  - A packet is an independent entity that can be stored and sent later

- A router in a packet-swithced network

  - A queue that can store and forward the pacekt

- More efficient than a circuit swithced network, but some delays for packet transmission

### [Review] Packet switching vs. circuit switching

- `Packet` switching allows more users to use network!

- Example
  - 1 Mb/s link
  - each user
    - 100 kb/s when "active"
    - Active 10% of time

![datacommunication11](/assets/images/2023-10-21/datacommunication11.png)

- Circuit switching:

  - 10 users

- Packet switching:

  - With 35 users, probability > 10 active at same time is less than 0.0004

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

### Network Core

- Mesh of interconnected routers

- `Packet-switching`: hosts break applicatioin-layer messages into packets
  - Forward packets from one router to the next, across links on path from source to destination
  - Each packet transmitted at full link capacity

### Datagram Networks (Packet Switching)

- In a datagram network, each packet is treated independently of all others

- The switches in a datagram network are traditionally referred to as routers.

- A Datagram network with four switches (routers)

![datacommunication12](/assets/images/2023-10-21/datacommunication12.png)

### Datagram Networks

- Traveling different paths to reach their destination

  - This approach can cause the datagrams of a transmission to arrive at their destination out of order with different delays between the packets.
  - Packets may also be lost or dropped because of a lack of resources.
  - In most protocols, it is the responsibility of an upper-layer protocol to reorder the datagrams or ask for lost datagrams

- Referred to as connectionless networks

  - Switch (packet switch) does not keep information about the connection state.
  - No setup or teardown phases.
  - Each packet is treated the same by a switch regardless of its source or destination.

- `Routing Table`

  - A switch in a datagram network uses a routing table that is based on the destination address.
  - Dynamic and updated periodically
  - Destination addresses and the corresponding forwarding output ports are recorded in the tables.
  - In a circuit switched network
    - Each entry is created when the setup phase is completed and deleted when the teardown phase is over

- Routing table in a datagram network

![datacommunication13](/assets/images/2023-10-21/datacommunication13.png)

- Destination Address

  - Every packet in a datagram network carries a header that contains the destination address of the packet.
  - The destination address in the header of a packet in a datagram network remains the same during the entire journey of the packet.

- Efficiency

  - The efficiency of a datagram network is better than that of a circuit-switched network

- Delay

  - Greater delay in a datagram network than in a virtual-circuit network
  - Since not all packets in a message necessarily travel through the same switches, the delay is not uniform

- Delays in a datagram network

![datacommunication14](/assets/images/2023-10-21/datacommunication14.png)

- How Do Loss and Delay Occur?

- Packets queue in router buffers
  - Packet arrival rate to link (temporarily) exceeds output link capacity
  - Packets queue, wait for turn

![datacommunication15](/assets/images/2023-10-21/datacommunication15.png)

### Four Sources of Packet Delay

![datacommunication16](/assets/images/2023-10-21/datacommunication16.png)

> d(nodal) = d(proce) + d(queue) + d(trans) + d(prop)

- d(proc): nodal processing

  - Check bit errors
  - Determine output link
  - Typically < msec

- d(queue): queueing delay

  - Time waiting at output link for transmission
  - Depends on congestion level of router

- d(trans): transmission delay

  - L: packet length(bits)
  - R: link bandwidth(bps)
  - d(trans) = L/R

- d(prop): propagation delay
  - d: length of physical link
  - s: propagation speed(~2x10^8 m/sec)
  - d(prop) = d/s

### Caravan Analogy

![datacommunication17](/assets/images/2023-10-21/datacommunication17.png)

- Cars "propagate" at 100 km/h
- Toll booth takes 12 sec to service car(bit transmission time)
- Car ~ bit; caravan ~ packet
- Q: How long until caravan is lined up before 2nd toll booth?

-> Time to "push" entire caravan through toll booth onto highway = 12\*10 = 120 sec

-> Time for last car to propagate from 1st to 2nd toll booth: 100km/(100km/h) = 1 hour

-> A: 62 minutes

- Suppose cars now "propagate" at 1000 km/h
- And suppose toll booth now takes one min to service a car
- Q: Will cars arrive to 2nd booth before all cars serviced at first booth?
  - A: Yes! after 7 min, first car arrives at second booth, three cars still at first booth

### Queueing Delay

- R: link bandwidth (bpt)
- L: packet length (bits)
- a: average packet arrival rate

- La/R ~ 0: avg. queueing delay small
- La/R -> 1: avg. queueing delay large
- La/R > 1: more "work" arriving than can be serviced, average delay infinte!

![datacommunication18](/assets/images/2023-10-21/datacommunication18.png)

### Packet Loss

- Queue (a.k.a. buffer) preceding link in buffer has finite capacity

- Packet arriving to full queue dropped (a.k.a. lost)

- Lost packet may be retransmitted by previous node, by source end system, or not at all

![datacommunication19](/assets/images/2023-10-21/datacommunication19.png)

### Throughput

- Throughput: data rate (bits/time unit) at which bits transferred between sender/receiver
  - Instantaneous: data rate at given point in time
  - Average: data rate over longer period of time

![datacommunication20](/assets/images/2023-10-21/datacommunication20.png)

- R(s) < R(c) What is average end-end throughput? -> R(s)

![datacommunication21](/assets/images/2023-10-21/datacommunication21.png)

- R(s) > R(c) What is average end-end throughput? -> R(c)

![datacommunication22](/assets/images/2023-10-21/datacommunication22.png)

> Bottleneck Link:
> Link on end-end path that constrains end-end throughput

### Throughput: Internet Scenario

- Per-connection end-end throughput:
  -> min(R(c), R(s), R/10)

- In practice: R(c) or R(s) is often bottleneck

![datacommunication23](/assets/images/2023-10-21/datacommunication23.png)

### Switching and TCP/IP Layers

- Switching at Physical Layer

  - Only circuit switching
  - Allow signals to travel in one path or another

- Switching at Data-Link Layer

  - Packet switching
  - The term packet in this case means frames or cells

- Switching at Network Layer

  - Packet switching
  - Either a virtual-circuit approach or a datagram approach can be used.
  - Currently the Internet uses a datagram approach

- Switching at Application Layer
  - Only message switching
  - Communication using e-mail is a kind of message-switched communication
