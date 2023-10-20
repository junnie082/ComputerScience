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
