# Data Communication 10

## Data Link Control & Medium Access Control

#### 2023.12.12.(화)

## Link Layer & IEEE 802 Project

## : DLC (IEEE 802.2) & MAC (IEEE 802.3, 802.11, 802.15)

### LinkLayer: Two Sublayers

- Dividing the data-link layer into two sublayers

![dc1](/assets/images/2023-12-12/dc1.png)

- `Data link control (DLC)` sublayer

  - All issues common to both point-to-point and broadcast links
  - Error detection and correction

- `Media access control (MAC)` sublayer
  - Only issues specific to broadcast links
  - How to access and share media
  - or `medium access control`, multiple access control

### IEEE Project 802

- `Project 802`

  - In 1985, the Computer Society of the IEEE started this project
  - It sets standards to enable intercommunication among equipment from a variety of manufacturers
    - Specifying functions of the physical layer and the data-link layer of major LAN protocols

- Logical Link Control (LLC)

* Data link control: framing, flow control, and error control
* IEEE Project 802: flow control, error control, and part of the framing duties
* LLC provides a single link-layer control protocol for all IEEE LANs

- Media Access Control (`MAC`)

* Defines the specific access method for each LAN

### Relationship of Project IEEE 802 Protocols to IP Stack

![dc2](/assets/images/2023-12-12/dc2.png)

## Data Link Control

## :Framing, Flow and error control, Retransmission

### DLC Services

- `Data link control` (DLC)
  - Procedures for communication between two adjacent nodes
  - Node-to-node communication
  - Primary function: `framing` and `flow and error control`

### Framing

- The physical layer provides bit synchronization to ensure that the sender and receiver use the same bit durations and timing

- The data-link layer, on the other hand, needs to pack bits into frames, so that each frame is distinguishable from another

* The envelope serves as the delimiter. In addition, each envelope defines the sender and receiver addresses, which is necessary since the postal system is a many-to-many carrier facility.

- Framing in the data-link layer

  - Separates a message from one source to a destination
  - By adding a sender address and a destination address
  - A very large frame: making flow and error control very inefficient
    - Need to be appropriate

- Frame Size

  - `Fixed-size` framing
    - No need for defining the boundaries of the frames
    - An example of this type of framing is the ATM WAN, which uses frames of fixed size called cells
  - `Variable-size` framing
    - Prevalent in local-area networks
    - a character-oriented approach and a bit-oriented approach

- `Character-Oriented` (or `byte-oriented`) Framing
  - Data to be carried are 8-bit characters from a coding system such as ASCII
  - The header is multiples of 8 bits
  - A frame in a character-oriented protocol

![dc3](/assets/images/2023-12-12/dc3.png)

- Character-Oriented (or byte-oriented) Framing [cont.]
  - `Byte stuffing` (or character stuffing)
    - A special byte added to the data section of the frame when there is a character with the same pattern as the flag
  - Byte stuffing and unstuffing

![dc4](/assets/images/2023-12-12/dc4.png)

- `Bit-Oriented Framing`
  - Data section of a frame is a sequence of bits to be interpreted by the upper layer as `text, graphic, audio, video, and so on`
  - Need a delimiter to separate one frame from the other
  - A frame in a bit-oriented protocol

![dc5](/assets/images/2023-12-12/dc5.png)

- `Bit stuffing`
- Process of adding one extra 0 whenever five consecutive 1s follow a 0 in the data, so that the receiver does not mistake the pattern 0111110 for a flag
- Bit stuffing and unstuffing

![dc6](/assets/images/2023-12-12/dc6.png)

### Flow and Error Control

- `Flow Control`
  - Balancing between production and consumption rates
  - Feedback from the receiving node to the sending node to stop or slow down pushing frames.
  - Flow control at the data link layer

![dc7](/assets/images/2023-12-12/dc7.png)

- Buffers
- The flow control communication can occur by sending signals from the consumer to the producer.
- When the buffer of the receiving data-link layer is full, it informs the sending data-link layer to stop pushing frames.

* `Error Control`
  - Prevent the receiving node from delivering corrupted packets to its network layer
  - `CRC` is added to the frame by the sender and checked by the receiver
  - Using one of the following two methods
  - In the first method, if the frame is corrupted, it is silently discarded; if it is not corrupted, the packet is delivered to the network layer. This method is used mostly in wired LANs such as Ethernet.
  - In the second method, if the frame is corrupted, it is silently discarded; if it is not corrupted, an acknowledgment is sent (for the purpose of both flow and error control) to the sender.

### Flow and Error Control

- `Combination` of Flow and Error Control
  - The acknowledgment (so-called `ACK`) that is sent for flow control can also be used for error control to tell the sender the packet has arrived uncorrupted

### Connectionless and Connection-Oriented

- `Connectionless` Protocol

  - Frames are sent from one node to the next without any relationship between the frames
  - Each frame is independent
  - Not numbered & no sense of ordering
  - Most of the data-link protocols for LANs are connectionless protocols

- `Connection-Oriented` Protocol
  - A `logical connection` should first be established
    - All frames are somehow related
  - Frames are numbered and sent in order
  - Connection-oriented protocols are rare in wired LANs

### Retransmission Protocols

- `ARQ` (Automatic Repeat Request) protocol

- Traditional four protocols have been defined for the data-link layer to deal with flow and error control

* Simple, `Stop-and-Wait`, `Go-Back-N`, and `Selective-Repeat`

- The behaviour of a data-link-layer protocol can be better shown as a `finite state machine (FSM)`
  - An FSM is thought of as a machine with a finite number of states
  - The machine is always in one of the states until an event occurs.

### Simple Protocol

- A simple protocol with neither flow nor error control

  - Assume that the receiver can immediately handle any frame it receives

- Simple protocol

![dc8](/assets/images/2023-12-12/dc8.png)

- FSM for the simple protocol

![dc9](/assets/images/2023-12-12/dc9.png)

- Example

* The following figure shows an example of communication using this protocol. It is very simple. The sender sends frames one after without even thinking about the receiver.
* Flow diagram for Example

![dc10](/assets/images/2023-12-12/dc10.png)

### Stop-and-Wait Protocol

- Using both flow and error control

- A primitive version of this protocol here, but we also have the more sophisticated version in when we have learned about sliding windows

- Sender sends one frame at a time and waits for an acknowledgment before sending the next one

- Stop-and-wait Protocol

![dc11](/assets/images/2023-12-12/dc11.png)

- FSM for the stop-and-wait protocol

![dc12](/assets/images/2023-12-12/dc12.png)

- Example

* The figure shows an example. The first frame is sent and acknowledged. The second frame is sent, but lost. After time-out, it is resent. The third frame is sent and acknowledged, but the acknowledgment is lost. The frame is resent.
* However, there is a problem with this scheme. The network layer at the receiver site receives two copies of the third packet, which is not right. In the next section, we will see how we can correct this problem using sequence numbers and acknowledgment numbers.

![dc13](/assets/images/2023-12-12/dc13.png)

- `Sequence` and `Acknowledgment Numbers`

  - A problem in Example 11.3: duplicate packets
  - Need to add sequence numbers to the data frames and acknowledgment numbers to the ACK frames
  - An acknowledgment number always defines the sequence number of the next frame to receive

- Example
  - The figure shows how adding sequence numbers and acknowledgment numbers can prevent duplicates. The first frame is sent and acknowledged. The second frame is sent, but lost. After time-out, it is resent. The third frame is sent and acknowledged, but the acknowledgment is lost. The frame is resent

![dc14](/assets/images/2023-12-12/dc14.png)

### Pipelined Retransmission Protocols: Overview

- `Go-back-N`

  - Sender can have up to N unacked packets in pipeline

  - Receiver only sends cumulative ack
  - Doesn't ack packet if there's a gap

  - Sender has timer for oldest unacked packet
  - When timer expires, retransmit all unacked packets

- `Selective Repeat`

  - Sender can have up to N unacked packets in pipeline

  - Rcvr sends individual ack for each packet

  - Sender maintains timer for each unacked packet
    - When timer expires, retransmit only that unacked packet

### Go-Back-N

- `Go-back-N` protocol (GBN)

  - "window" of up to N, consecutive unacked pkts allowed

- Send window for Go-back-N

![dc15](/assets/images/2023-12-12/dc15.png)

### GBN in Action

![dc16](/assets/images/2023-12-12/dc16.png)

### Selective Repeat

- Receiver individually acknowledges all correctly received pkts

  - `Buffers` pkts, as needed, for eventual in-order delivery to upper layer

- Sender only resends pkts for which ACK not received
  - Sender timer for each unACKed pkt

![dc17](/assets/images/2023-12-12/dc17.png)

### Selective Repeat in Action

![dc18](/assets/images/2023-12-12/dc18.png)

### Piggybacking

- To make the communication more efficient,

* The data in one direction is piggybacked with the acknowledgment in the other direction.
* In other words, when node A is sending data to node B, node A also acknowledges the data received from node B.

## Medium Access Control

## : Channel partitioning, Random access, Taking turns

- `MAC`

  - `Medium access control` (in 3GPP, IEEE 802.11)
  - Also, media access control protocol or multiple access control

- Two types of "links":
  - `Point-to-point`
  - Point-to-point link between `Ethernet` switch, host
  - `Broadcast` (shared wire or medium)
  - Old-fashioned Ethernet
  - 802.11 wireless LAN

![dc19](/assets/images/2023-12-12/dc19.png)

### Medium Access Control Protocols

- Single shared broadcast channel

- Two or more simultaneous transmissions by nodes: `interference`

  - Collision if node receives two or more signals at the same time

- Medium access control (MAC) protocols

* Distributed algorithm that determines how nodes share channel, i.e., determine when node can transmit
* Also, centralized algorithm (scheduling)
* c.f., centralized scheduling vs. distributed scheduling
* Communication about channel sharing must use channel itself!
* No out-of-band channel for coordination

### An Ideal MAC Protocol

- Given: `broadcast channel` of rate R bps

- Desiderata:
  1. When one node wants to transmit, it can send at rate R.
  2. When M nodes want to transmit, each can send at average rate R/M
  3. Fully decentralized:
  - No `special node` to coordinate transmissions
  - No `synchronization` of clocks, slots
  4. `Simple`

### MAC Protocols: Taxonomy

- Three broad classes

- `Channel partitioning`

* Divide channel into smaller "pieces" (time slots, frequency, code)
* Allocate piece to node for exclusive use (dedicated)

- `Random access`

* Channel not divided, allow `collisioins`
* "Recover" from collisions

- "Taking turns", "controlled access protocols", "collicion-free", "contention-free"

* Nodes take turns, but nodes with more to send can take longer turns

### Channel Partitioning MAC Protocols: TDMA

- `TDMA`: time division multiple access
  - Access to channel in "rounds"
  - Each station gets fixed length slot (length = packet transmission time) in each round
  - Unused slots go idle
  - Example: 6-station LAN, 1,3,4 have packets to send, slots 2,5,6 idle

![dc20](/assets/images/2023-12-12/dc20.png)

### Channel Partitioning MAC Protocols: FDMA

- `FDMA`: frequency division multiple access
  - Channel spectrum divided into frequency bands
  - Each station assigned fixed frequency band
  - Unused transmission time in frequency bands go idle
  - Example: 6-station LAN, 1,3,4 have packet to send, frequency bands 2,5,6 idle

![dc21](/assets/images/2023-12-12/dc21.png)

- Also, `CDMA` (code division multiple access), `SDMA` (space division multiple access)

### Random Access Protocols

- When node has packet to send

  - Transmit at full channel data rate R
  - No a priori coordination among nodes

- Two or more transmitting nodes => "`collision`",

- `Random access` MAC protocol specifies:

  - How to detect collisions
  - How to recover from collisions (e.g., via delayed retransmissioins)

- Examples of random access MAC protocols:
  - (Pure) `ALOHA`
  - `Slotted ALOHA`
  - `CSMA, CSMA/CD, CSMA/CA`

### ALOHA

- ALOHA

  - The earliest random access method
  - Developed at the University of Hawaii in early 1970
    - It was designed for a radio (wireless) LAN

- `Pure ALOHA`

  - A simple but elegant protocol
  - Each station sends a frame whenever it has a frame to send (multiple access)
  - Possibility of `collision`
  - If collided, need to resend the frames that have been destroyed during transmission.

- Pure Aloha (cont.)
  - Frames in a pure ALOHA network

![dc22](/assets/images/2023-12-12/dc22.png)

- Pure Aloha (cont.)

  - Pure ALOHA protocol relies on acknowledgments from the receiver

    - If the acknowledgment does not arrive after a time-out period, the station assumes that the frame (or the acknowledgment) has been destroyed and resends the frame.

  - If all these stations try to resend their frames after the time-out, the frames will collide again
    - each station waits a random amount of time before resending its frame
    - Backoff time TB

- `Binary exponential backoff`

  - For each retransmission, a multiplier R = 0 to 2^k - 1 is randomly chosen and multiplied by Tp (maximum propagation time) or Tfr (the average time required to send out a frame) to find TB.

- Pure Aloha (cont.)
  - Procedure for pure ALOHA protocol

![dc23](/assets/images/2023-12-12/dc23.png)

- `Vulnerable time`
  - Length of time in which there is a possibility of collision
  - Vulnerable time for pure ALOHA protocol

![dc24](/assets/images/2023-12-12/dc24.png)

### [Review] Bernoulli Process

- A sequence of indenpendent Bernoulli trials

- At each trial i:

  - Pr[success] = Pr[Xi = 1] = p
  - Pr[failure] = Pr[Xi = 0] = 1-p

- Examples
  - Sequence of lottery wins/losses
  - Sequence of ups and downs of the Dow Jones
  - Arrivals (each second) to a bank
  - Arrivals (at each time slot) to server

![dc25](/assets/images/2023-12-12/dc25.png)

- `Time homogeneity`

  - `Pr[𝜅, 𝜏]` = Probability of 𝜅 arrivals in interval of duration 𝜏

- Numbers of arrivals in disjoint time intervals are independent

- Small interval probabilities
  - For very small 𝛿

![dc26](/assets/images/2023-12-12/dc26.png)

    - 𝜆: arrival time

- An arrival process is called a Poisson process with 𝜆 if it has the following properties

  - [`Time-homogeneity`] The probability Pr[𝑘,𝜏] of 𝑘 arrivals is the same for all intervals of the same length 𝜏

  - [`Independence`] The number of arrivals during a particular interval is independent of the history of arrivals outside this interval

  - [`Small interval probabilities`] The probabilities Pr[𝑘,𝜏] satisfy

![dc27](/assets/images/2023-12-12/dc27.png)

### [Review] PMF of Number of Arrivals N

- Finely discretize [0,t]: approximately Bernoulli

- Nt (of discrete approximation): binomial

![dc28](/assets/images/2023-12-12/dc28.png)

### [Suppl.] Derivation of Poisson PMF from Binomial Distribution

- Average number of arrivals during t: 𝜆𝑡 = 𝑁𝑝

![dc29](/assets/images/2023-12-12/dc29.png)

### [Review] Bernoulli as the Discrete-Time Version of Poisson

![dc30](/assets/images/2023-12-12/dc30.png)

|                                |     Poisson     |  Bernoulli  |
| :----------------------------: | :-------------: | :---------: |
|        Times of arrival        |   Continuous    |  Discrete   |
|          Arrival rate          | λ per unit time | p per trial |
|      PMF of # of arrivals      |     Poisson     |  Binomial   |
| Interarrival time distribution |   Exponential   |  Geometric  |
|      Time to 𝑘th arrival       |     Erlang      |   Pascal    |

- Pure Aloha (cont.)
  - Throughput

```
The throughput for pure ALOHA is S = G x e^(-2G).
The maximum throughput S(max) = 1/(2e) = 0.184 when G = (1/2).
```

- During vulnerable time 2Tfr, transmission only by a single node

  - Define offered load G = 𝜆𝑇𝑓r
  - Throughput (during 𝑇𝑓r) for pure ALOHA = [Offered load] x [No transmission prob of other nodes]

  ```
  S = G x e^(-2G)
  ```

  - Max throughput

![dc31](/assets/images/2023-12-12/dc31.png)

- Example

* A pure ALOHA network transmits 200-bit frames on a shared channel of 200 kbps. What is the throughput if the system (all stations together) produces
  a. 1000 frames per second?
  b. 500 frames per second?
  c. 250 frames per second?

* Solution
  - The `frame` transmission time is 200/200 kbps or `1 ms`.
  - If the system creates 1000 frames per second, or 1 frame per millisecond, then G = 1. In this case S = G x e^(-2G) = 0.135 (13.5 percent).
  - If the system creates 500 frames per second, or 1/2 frames per millisecond, then G = 1/2. In this case S = G x e^(-2G) = 0.184 (18.4 percent). This means that the throughput is 500 x 0.184 = 92 and that only 92 frames out of 500 will probably survive. Note that this is the maximum throughput case, percentage-wise.
  - If the system creates 250 frames per second, or 1/4 frames per millisecond, then G = 1/4. In this case S = G x e^(-2G) = 0.152 (15.2 percent). This means that the throughput is 250 x 0.152 = 38. Only 38 frames out of 250 will probably survive

- `Slotted ALOHA`
  - Invented to improve the efficiency of pure ALOHA
  - Divide the time into slots of Tfr seconds and force the station to send only at the beginning of the time slot.
  - Frames in a slotted ALOHA network

![dc32](/assets/images/2023-12-12/dc32.png)

### Slotted ALOHA

- Slotted ALOHA (cont.)
  - `Vulnerable time` is now reduced to one-half
  - Vulnerable time for slotted ALOHA protocol

![dc33](/assets/images/2023-12-12/dc33.png)

- Throughput

```
The throughput for slotted ALOHA is S = G x e^(-G).
The maximum throughput S(max) = 0.368 when G = 1.
```

- During `vulnerable time Tfr`, transmission only by a single node
  - Define offered load 𝐺 = 𝜆𝑇𝑓r
  - Throughput (during 𝑇𝑓r) for pure ALOHA = [Offered load] x [No transmission prob of other nodes]

```
S = G x e^(-G)
```

- Max throughput

![dc34](/assets/images/2023-12-12/dc34.png)

- Example

  - A slotted ALOHA network transmits 200-bit frames using a shared channel with a 200-kbps bandwidth.
    Find the throughput if the system (all stations together) produces
    a. 1000 frames per second.
    b. 500 frames per second.
    c. 250 frames per second.

  - Solution
    - This situation is similar to the previous exercise except that the network is using slotted ALOHA instead of pure ALOHA. The frame transmission time is 200/200 kbps or 1 ms.
      a. In this case G is 1. So S = G x e^(-G) = 0.368 (36.8 percent). This means that the throughput is 100 x 0.0368 = 368 frames. Only 368 out of 1000 frames will probably survive. Note that this is the maximum throughput case, percentage-wise.
      b. here G is 1/2. In this case S = G x e^(-G) = 0.303 (30.3 percent). This means that the throughput is 500 x 0.0303 = 151. Only 151 frames out of 500 will probably survive.
      c. Now G is 1/4. In this case S = G x e^(-G) = 0.195 (19.5 percent). This means that the throughput is 250 x 0.195 = 49.
      Only 49 frames out of 250 will probably survive.

- Carrier sense multiple access (`CSMA`)

  - To minimize the chance of collision and, therefore, increase the performance
  - The chance of collision can be reduced if a station senses the medium before trying to use it.
  - CSMA is based on the principle "sense before transmit" or "`listen before talk`"
  - The possibility of collision still exists because of `propagation delay`

- Space/time model of a collisioin in CSMA

![dc35](/assets/images/2023-12-12/dc35.png)

- Vulnerable Time

![dc36](/assets/images/2023-12-12/dc36.png)

### CSMA/CD

- Carrier sense multiple access with collision detection (CSMA/CD)

* Augmenting the algorithm to handle the collision
* A station monitors the medium after it sends a frame to see if the transmission was successful
  - If collided, abort transmission
* Collision and abortion in CSMA/CD -> More on that later (for old Ethernet, IEEE 802.3)

![dc37](/assets/images/2023-12-12/dc37.png)

### CSMA/CA

- Carrier sense multiple access with collision avoidance (CSMA/CA)

* Invented for wireless networks
* Collisions are avoided through using CSMA/CA's three strategies:
  - Interframe space, Contention window, and Acknowledgments
* More on that later (for WiFi, IEEE 802.11)

### "Taking turns" MAC protocols

- Channel partitioning MAC protocols:

* Share channel efficiently and fairly at high load
* Inefficient at low load: 1/N bandwidth allocated even if only 1 active node

- Random access MAC protocols

* Efficient at low load: single node can fully utilize channel
* High load: collision overhead

- "Taking turns" protocols

* "Taking turns", "controlled access protocols", "collision-free", "contention-free", "scheduled"

- Polling
  - Master node "invites" slave nodes to transmit in turn
  - Typically used with "dumb" slave devices
  - Concerns
    - Polling overhead
    - Latency
    - Single point to failure (master)

![dc38](/assets/images/2023-12-12/dc38.png)

- `Token` passing

  - Control token passed from one node to next sequentially.

  - Token message

  - Concerns
    - Token overhead
    - Latency
    - Single point of failure (token)

![dc39](/assets/images/2023-12-12/dc39.png)
